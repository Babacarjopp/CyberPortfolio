# Architecture du Laboratoire - Semaine 0

##  Objectif
Mettre en place un environnement isolé et sécurisé , couvrant le Pentest, le SOC, le Forensic et le GRC.

## 🌐 Réseau
Toutes les machines virtuelles communiquent sur un réseau **Host-Only** isolé.
- **Sous-réseau :** `192.168.56.0/24`
- **Passerelle (Gateway) :** `192.168.56.1` (Interface virtuelle de l'hôte)
- **DHCP :** Désactivé (IPs statiques pour plus de clarté)

## 🖥️ Inventaire des Machines Virtuelles

1. Kali Linux

Rôle : Machine d'attaque / Administration

Adresse IP : 192.168.56.103

Système d'exploitation : Kali Linux (2026)

RAM : 4 Go

Disque : 40 Go

2. Ubuntu Server

Rôle : Serveur SIEM (Wazuh)

Adresse IP : 192.168.56.108

Système d'exploitation : Ubuntu Server 24.04

RAM : 6 Go

Disque : 50 Go

3. Windows 10

Rôle : Cible / Endpoint (Agent Wazuh)

Adresse IP : 192.168.56.110

Système d'exploitation : Windows 10 Pro

RAM : 4 Go

Disque : 50 Go

4. Metasploitable

Rôle : Cible vulnérable (Pentest)

Adresse IP : À définir (ex: 192.168.56.111)

Système d'exploitation : Linux

RAM : 1 Go

Disque : 10 Go

5. OWASP Juice Shop

Rôle : Cible Web vulnérable

Adresse IP : Docker sur Ubuntu Server

Système d'exploitation : Docker (conteneur)

RAM : Partagée avec l'hôte

Disque : Partagé avec l'hôte

## 🛡️ Stack Technique Déployée

### 1. Hyperviseur
- **VirtualBox** (ou VMware)
- Réseau Host-Only pour l'isolation.

### 2. SIEM (Security Information and Event Management)
- **Wazuh 4.12.0** déployé via **Docker Compose** sur Ubuntu Server.
- Composants :
  - `wazuh-indexer` (Stockage et recherche des logs)
  - `wazuh-manager` (Analyse et corrélation)
  - `wazuh-dashboard` (Interface web)
- **Accès :** `https://192.168.56.108` 

### 3. Agents
- **Agent Ubuntu Server :** Installé en local pour l'auto-surveillance.
- **Agent Windows 10 :** Installé via MSI pour la télémétrie.

## 🔄 Flux de Données
1. Les agents (Ubuntu, Windows) collectent les logs.
2. Ils les envoient au `wazuh-manager` (port 1514).
3. Le manager analyse les logs et les stocke dans l'`indexer`.
4. L'analyste (vous) consulte les alertes via le `dashboard`.