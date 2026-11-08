# Plan d'adressage IP : MTF Consulting

Convention : la passerelle est la 1re adresse utilisable de chaque réseau.

| Site | VLAN | Nom | Réseau | Masque | Passerelle | IP utilisables | Broadcast | Postes |
|---|---|---|---|---|---|---|---|---|
| Rabat | 10 | Direction | 192.168.10.0/26 | 255.255.255.192 | 192.168.10.1 | .1 à .62 | 192.168.10.63 | 62 |
| Rabat | 20 | Employés | 192.168.20.0/25 | 255.255.255.128 | 192.168.20.1 | .1 à .126 | 192.168.20.127 | 126 |
| Rabat | 30 | Serveurs | 192.168.30.0/28 | 255.255.255.240 | 192.168.30.1 | .1 à .14 | 192.168.30.15 | 14 |
| Rabat | 99 | Gestion | 192.168.99.0/29 | 255.255.255.248 | 192.168.99.1 | .1 à .6 | 192.168.99.7 | 6 |
| Salé | 40 | Employés | 192.168.40.0/26 | 255.255.255.192 | 192.168.40.1 | .1 à .62 | 192.168.40.63 | 62 |
| Liaison | — | Rabat ↔ Salé | 10.0.0.0/30 | 255.255.255.252 | — | .1 (routeur Rabat), .2 (routeur Salé) | 10.0.0.3 | 2 |

## Méthode de calcul
1. Bits pour les hôtes = 32 − préfixe
2. Taille du bloc = 2^(bits hôtes)
3. Masque (dernier octet) = 256 − taille du bloc
4. Broadcast = adresse du réseau + taille − 1
5. Postes utilisables = taille − 2 (adresse du réseau et broadcast exclues)

## Choix de conception
- Un VLAN par service : on peut filtrer les flux entre services (ACL) et limiter les broadcasts.
- Les serveurs ont leur propre VLAN, pour contrôler qui y accède.
- Le VLAN 99 (gestion) est réservé à l'administration des équipements (SSH).
- La liaison entre les sites est en /30 : 2 adresses utilisables, une par routeur, sans gaspillage.
