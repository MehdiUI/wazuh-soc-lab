# 🛡️ Lab SOC N1 : Déploiement Wazuh & Détection de Brute-Force

## 📌 Contexte du Projet
Ce projet documente la mise en place d'un laboratoire SOC (Security Operations Center) personnel. L'objectif est de simuler le travail quotidien d'un analyste SOC Niveau 1 : de l'ingestion des logs à la détection d'une attaque, en passant par l'investigation et la rédaction d'un rapport d'incident.

## 🏗️ Architecture du Lab
- **Hyperviseur :** Oracle VirtualBox
- **Système :** Ubuntu Server 22.04 LTS
- **SIEM :** Wazuh (Indexer, Manager, Dashboard v4.13)
- **Machine cible :** Le serveur lui-même (logs locaux via `/var/log/auth.log`)

## ⚔️ Scénario d'Attaque : Brute-Force SSH (MITRE T1110)
Pour valider le bon fonctionnement du SIEM, j'ai simulé une attaque par force brute sur le service SSH (6 tentatives en quelques secondes).

### 🧠 Ingénierie de Détection (Detection Engineering)
Plutôt que de me contenter des alertes par défaut, j'ai créé une **règle personnalisée** (`local_rules.xml`) pour détecter un seuil spécifique : 5 échecs de connexion en 60 secondes.

```xml
<group name="local,syslog,sshd,">
  <rule id="100001" level="10" frequency="5" timeframe="60">
    <if_matched_sid>5710</if_matched_sid>
    <description>Brute-Force SSH personnalise (5+ echecs en 60s)</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>

<img width="1920" height="925" alt="FireShot Capture 096 - Wazuh -  127 0 0 1" src="https://github.com/user-attachments/assets/a8831fd0-d184-4af9-ab07-4a68f945a6a7" />
