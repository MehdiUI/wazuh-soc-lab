# Rapport d'Investigation - Incident SOC

## 1. Informations Générales
- **ID de l'incident :** INC-2026-001
- **Date :** 13 Novembre 2026
- **Analyste :** [Ton Nom/Prénom]
- **Statut :** Clôturé (Vrai Positif)
- **Sévérité :** Élevée (Niveau 10)

## 2. Détection
- **Règle déclenchée :** `100001` (Personnalisée)
- **Description :** Brute-Force SSH personnalise (5+ echecs en 60s)
- **Technique MITRE ATT&CK :** T1110 (Brute Force)

## 3. Analyse et Triage
L'alerte a été déclenchée suite à la détection de multiples tentatives d'authentification SSH échouées. 
- **Source de l'attaque :** `192.168.1.100`
- **Cible :** `faux-utilisateur` sur le serveur `wazuh-manager`
- **Volume :** 6 tentatives en moins de 3 secondes.

**Conclusion :** L'alerte est un **Vrai Positif**. La vitesse des requêtes et l'utilisation d'un nom d'utilisateur inexistant indiquent clairement une attaque automatisée. Il ne s'agit pas d'une erreur d'un utilisateur légitime.

## 4. Recommandations (Remédiation)
En environnement de production, les actions suivantes devraient être immédiatement appliquées :
1. **Blocage réseau :** Ajouter l'IP `192.168.1.100` sur liste noire au niveau du pare-feu (UFW/iptables).
2. **Durcissement :** Désactiver l'authentification par mot de passe pour SSH et passer exclusivement à l'authentification par clé publique.
3. **Protection active :** Installer et configurer `Fail2Ban` pour bannir automatiquement les IP après un certain nombre d'échecs.
