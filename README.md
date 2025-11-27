# Automatisation vSphere – Précheck, Déploiement & Post-install

## Structure du projet

automatisation/
├─ playbooks/
│  ├─ deploy_vms.yml
│  └─ install_packages.yml
├─ roles/
│  ├─ precheck_vsphere/
│  │  ├─ defaults/main.yml
│  │  └─ tasks/main.yml
│  ├─ create_vm_vsphere/
│  │  ├─ defaults/main.yml
│  │  └─ tasks/main.yml
│  └─ install_packages/
│     ├─ defaults/main.yml
│     └─ tasks/main.yml
└─ inventory/

## Fonctionnement

1. **precheck_vsphere** lit un CSV et vérifie :
   - si la VM existe déjà
   - si l’IP répond au ping
   - si le VLAN existe dans vSphere  
   Il produit `valid_vms` et `invalid_vms`.

2. **create_vm_vsphere** :
   - déploie uniquement les VMs valides
   - génère un inventaire `inventory/inventaire_deploiement_TIMESTAMP`

3. **install_packages**
   - s’exécute sur les VM nouvellement déployées
   - utilise l’inventaire généré précédemment

## Utilisation

### Déploiement complet
```
ansible-playbook playbooks/deploy_vms.yml -e csv_file=/chemin/vms.csv
```

### Post-install seule
```
ansible-playbook -i inventory/inventaire_deploiement_xxxxx playbooks/install_packages.yml
```

