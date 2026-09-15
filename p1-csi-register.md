# TP ITIL 5 — Partie 1 : Diagnostic (4 dimensions + Continual Improvement)

**Service :** Helpdesk interne
**Symptômes :** lenteur de traitement, tickets perdus, rappels multiples des utilisateurs.

## 1. Diagnostic par les 4 dimensions

**Organisations & personnes :** les rappels répétés montrent qu'aucun agent n'est propriétaire unique du ticket jusqu'à sa clôture — le Service Desk (N1, SPOC censé être le point de contact unique de l'utilisateur) perd le contexte dès qu'un ticket change de mains, faute de règle claire de propriété du dossier.

**Information & technologie :** les « tickets perdus » indiquent un outil sans cycle de vie strict — pas d'identifiant unique par ticket, pas de statut fiable (Nouveau → Assigné → En cours → Résolu → Clos), pas de traçabilité entre les canaux de saisie. GLPI couvre nativement ce besoin (ticket comme unité de traçabilité, historique partagé) : le problème n'est pas l'outil mais son paramétrage actuel.

**Partenaires & fournisseurs :** aucune dépendance externe n'est mentionnée dans les symptômes — dimension à vérifier en partie 2 (existe-t-il un UC, contrat fournisseur externe, derrière une partie du délai constaté ?), non concluante à ce stade.

**Value Streams & processus :** la combinaison des trois symptômes dessine un flux sans étapes standardisées ni points de contrôle — pas de SLA formalisé, pas de règle d'escalade N1/N2 explicite, pas de critère écrit pour qualifier et prioriser un ticket.

## 2. CSI Register

| # | Amélioration | Effort | Impact | Priorité |
|---|---|---|---|---|
| 1 | ID de ticket unique + statut en libre-service (notification auto) dans GLPI | Faible | Fort | 1 |
| 2 | Value stream formalisé + propriétaire unique du ticket jusqu'à résolution | Moyen | Fort | 2 |
| 3 | SLA par catégorie + dashboard de charge avec alertes | Fort | Moyen | 3 |

**Justification :** l'action 1 règle le symptôme le plus visible à faible coût, en s'appuyant sur des fonctionnalités déjà présentes dans GLPI ; l'action 2 traite la cause structurelle des rappels mais coûte plus cher en organisation (rôles, règles de transfert N1/N2) ; l'action 3 n'a de sens qu'une fois le flux stabilisé par les deux premières.

## 3. Principe directeur mobilisé

**« Progresser de manière itérative avec du feedback »** — chaque amélioration est un incrément testable avant d'investir dans la suivante, plutôt qu'une refonte globale risquée et difficile à diagnostiquer en cas d'échec. C'est aussi la logique de la boucle PDCA (Plan-Do-Check-Act) sur laquelle repose l'amélioration continue (CSI) : on ne peut vérifier (Check) que ce qu'on a d'abord mesuré, d'où l'ordre de priorité retenu ci-dessus.

---
*Diagnostic validé après relecture des symptômes rapportés par les utilisateurs.*
