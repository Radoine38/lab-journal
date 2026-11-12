# Labo 0.1 : VMware, créer et ajuster une VM

## Objectif
Créer une VM Ubuntu Server, puis augmenter son CPU, sa RAM et son disque, et prouver que le système voit le nouvel espace.

## Environnement
- Hôte : PC Windows, Intel Core i7-11800H (8 cœurs, 16 Go de RAM), virtualisation activée
- VMware Workstation Pro 17
- VM : Ubuntu Server 26.04.1 LTS (amd64), réseau NAT

## Étapes
1. Création de la VM : 2 Go de RAM, 1 CPU, disque de 20 Go (partition ext4 simple, sans LVM).
2. Installation d'Ubuntu Server (OpenSSH activé).
3. Preuves « avant » : `lsblk`, `df -h`, `free -h`.
4. Arrêt de la VM (`sudo poweroff`), puis dans VMware : 4 Go de RAM, 2 CPU, disque agrandi à 30 Go.
5. Redémarrage : le disque `sda` fait 30 Go mais la partition `sda2` reste à 20 Go.
6. `sudo growpart /dev/sda 2` pour étendre la partition, puis `sudo resize2fs /dev/sda2` pour étendre le système de fichiers.
7. Vérification avec `df -h`.

## Preuves
Voir le dossier `captures/` (01 à 06, dans l'ordre).
- Avant : `/dev/sda2` 20G, mémoire ~2 Go, 1 CPU.
- Après : `/dev/sda2` 30G (5,5G utilisés, 23G libres), 4 Go de RAM, 2 CPU.

## Problèmes rencontrés
- Faute de frappe dans la mémoire (248 Mo au lieu de 2048 Mo) : repérée avant le démarrage, sinon l'installateur aurait échoué.
- LVM décoché volontairement pour que `growpart` et `resize2fs` suffisent.
- Mot de passe `sudo` erroné deux fois : sans conséquence.

## Ce que j'ai appris
- Agrandir le disque dans VMware ne suffit pas : il faut aussi étendre la partition, puis le système de fichiers.
- On arrête la VM proprement avant de changer le matériel.
- Une VM modifiable demande de partir d'une base volontairement petite pour pouvoir prouver l'avant et l'après.
