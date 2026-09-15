# TP ITIL 5 — Partie 2 : Pilotage du service (Service Level Management + Event Management)

## 1. SLA proposés

### SLA 1 — Délai de première réponse

| Priorité | SLO (objectif mesurable) |
|---|---|
| Critique (P1) | 95% des tickets répondus en moins de 30 minutes |
| Élevée (P2) | 95% des tickets répondus en moins de 2 heures |
| Normale (P3) | 90% des tickets répondus en moins de 8 heures ouvrées |
| Basse (P4) | 90% des tickets répondus en moins de 24 heures ouvrées |

**Définition :** le délai de première réponse court de la création du ticket jusqu'à la première prise de contact d'un agent avec l'utilisateur (accusé de réception ou début de traitement), et non jusqu'à la résolution. La priorité elle-même doit être calculée (Impact × Urgence) et non déduite du seul ton de l'appel, pour éviter qu'un ticket bruyant ne dépasse en file un ticket réellement critique.

### SLA 2 — Délai de résolution

| Priorité | SLO (objectif mesurable) |
|---|---|
| Critique (P1) | 90% des tickets résolus en moins de 4 heures |
| Élevée (P2) | 90% des tickets résolus en moins de 1 jour ouvré |
| Normale (P3) | 85% des tickets résolus en moins de 3 jours ouvrés |
| Basse (P4) | 85% des tickets résolus en moins de 5 jours ouvrés |

**Définition :** le délai de résolution court de la création du ticket jusqu'à la clôture validée par l'utilisateur (ou clôture automatique après confirmation sans retour sous 48h).

**Soutenabilité des SLA (OLA/UC) :** un SLA n'est tenable que s'il est décomposé en engagements internes plus courts. Exemple pour le SLA de résolution P1 (4h) : OLA interne du helpdesk à 2h30 (diagnostic + résolution N1/N2), marge de 1h, et le cas échéant un UC fournisseur (éditeur du logiciel métier) à 30 min si une expertise N3 externe est requise. Sans cette décomposition, le SLA client reste un engagement de façade.

## 2. Classification des logs (Event Management)

| Log | Classification | Justification |
|---|---|---|
| `AUTH user=jdupont action=login status=success` | **Informational** | Authentification réussie, comportement attendu, aucune anomalie, aucune action requise. |
| `DISK host=SRV-FILE01 usage=82% threshold=80%` | **Warning** | Le seuil d'alerte (80%) est déjà dépassé (82%) mais le service reste fonctionnel — nécessite une action préventive avant saturation complète. |
| `SVC name=helpdesk-portal status=unreachable duration=00:04:12` | **Exception** | Le portail helpdesk lui-même est inaccessible depuis plus de 4 minutes — impact direct sur la capacité des utilisateurs à créer des tickets, nécessite une action immédiate. |
| `BACKUP job=nightly-backup host=SRV-DB01 status=completed size=45GB` | **Informational** | Sauvegarde terminée avec succès, comportement nominal, aucune action requise. |
| `NET link=switch-3F-port12 status=down flapping=true count=6/10min` | **Exception** | Un lien réseau qui bascule 6 fois en 10 minutes (flapping) indique une instabilité matérielle ou de configuration active, avec risque de coupures répétées pour les postes connectés à ce port — nécessite une intervention immédiate. |

### Actions à déclencher

**Warning — Disque SRV-FILE01 (82%/80%) :**
Ouvrir une tâche préventive (Problem Management proactif) dans GLPI pour identifier les fichiers volumineux ou obsolètes à archiver/supprimer, ou planifier une extension de volume avant d'atteindre un seuil critique (ex. 95%) qui impacterait le service.

**Exception — Portail helpdesk inaccessible :**
Ouvrir un **Incident** dans GLPI en priorité critique (P1) immédiatement : le portail est le SPOC applicatif des utilisateurs pour créer des tickets, son indisponibilité aggrave directement les symptômes du TP (tickets perdus, lenteur perçue). Vérifier l'état du service applicatif, redémarrer si nécessaire, et notifier les utilisateurs via un canal alternatif (email, téléphone) tant que le portail est down.

**Exception — Flapping switch-3F-port12 :**
Ouvrir un **Incident** en priorité élevée (P2) et escalader vers l'équipe réseau (N2) : isoler le port si possible pour éviter une propagation, vérifier le câble/transceiver physique et la configuration du port (spanning-tree, duplex mismatch), et surveiller si d'autres CI de la CMDB rattachés au même switch sont affectés.
