# Semaine 0 : Montage de l'Atelier (Lab Setup)

## Objectif de la semaine
Mettre en place l'infrastructure technique nécessaire . L'objectif est d'avoir un environnement isolé, fonctionnel et documenté pour pratiquer le Pentest, le SOC, le Forensic et le GRC en toute sécurité.

##  Architecture Mise en Place
Retrouvez le détail complet dans le fichier(architecture.md).

- **Réseau :Host-Only `192.168.56.0/24` (Isolation totale).
- **Kali Linux (192.168.56.103) :** Machine d'attaque et d'administration.
- **Ubuntu Server (192.168.56.108) : Serveur hébergeant le SIEM Wazuh.
- **Windows 10 (192.168.56.110) :Cible pour les agents et l'Active Directory.

## 🚀 Déploiement de Wazuh (SIEM)
L'installation native de Wazuh sur Ubuntu 24.04 a échoué à plusieurs reprises (problèmes de paquets corrompus, incompatibilité). J'ai donc opté pour un déploiement via **Docker Compose**, qui est plus robuste et reproductible.

**Commandes clés utilisées :**
```bash
git clone https://github.com/wazuh/wazuh-docker.git -b v4.12.0
cd wazuh-docker/single-node
sudo docker compose -f generate-indexer-certs.yml run --rm generator
sudo docker compose up -d