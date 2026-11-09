# GÉNIE LOGICIEL — LIVRABLE TP1
**Cadrage du Projet & Analyse des Premières Exigences**  
**Sujet 4 :** Parking Payant d'un Centre Commercial  
**Fichier attendu :** `TP1/TP1-cadrage` | **Cadre :** Centre commercial (40 boutiques, 6 agents, 1 directrice)

---

## 1. Vision

### Q1. Problème Métier vs Solution Technique
- **Formulation du problème métier :** Le centre commercial rencontre des difficultés pour réguler les flux d'accès au parking, tarifier équitablement la durée de stationnement, prévenir les sorties et ré-entrées abusives contournant la gratuité et concilier les attentes divergentes de ses acteurs internes (agents, commerces, direction).
- **Phrase décrivant une solution plutôt que le problème :** « Le centre commercial veut un logiciel pour son parking souterrain. »
- **Justification :** Cette phrase énonce un moyen technique d'implémentation (créer un logiciel), et non la cause racine ou le dysfonctionnement opérationnel.

### Q2. Parties Prenantes du Projet

| Partie prenante | Statut | Intérêt / Attente vis-à-vis du projet |
| :--- | :--- | :--- |
| **Directrice** | Explicite | Superviser en temps réel l'occupation du parking, optimiser la gestion globale et garantir la rentabilité. |
| **Agents de parking (6)** | Explicite | Disposer d'outils et de règles claires pour régler vite les litiges, notamment un prix unique forfaitaire en cas de ticket perdu. |
| **Boutiques (40)** | Explicite | Offrir le stationnement pour fidéliser leurs clients et éviter qu'un client ayant perdu son ticket ne paie plus que son temps réel. |
| **Clients automobilistes** | Non citée (impactée) | Profiter d'une tarification lisible et juste (gratuité courte durée, tarif raisonnable longue durée) et d'une circulation fluide. |
| **Automobilistes abusifs** | Non citée (ciblée) | Usagers contournant la tarification en ressortant juste avant la fin de gratuité pour ré-entrer aussitôt (comportement à bloquer). |
| **Service technique / Maintenance** | Non citée (impactée) | Assurer la disponibilité matérielle et logicielle des bornes et barrières en temps réel. |

### Q3. Objectifs SMART du Projet
- **OBJ-01 :** Réduire le nombre de ré-entrées abusives successives de [valeur actuelle à mesurer] à [valeur cible à valider, ex. 0 incident] d'ici [délai à valider après mise en service].
- **OBJ-02 :** Réduire le temps moyen de traitement d'un véhicule à la borne de sortie de [valeur actuelle à mesurer] à [valeur cible à valider] d'ici [délai à valider].
- **OBJ-03 :** Maintenir le taux de disponibilité opérationnelle du système en temps réel à au moins [valeur cible à valider, ex. 99.8%] d'ici [délai à valider].

### Q4. Objectif le plus difficile à mesurer
- **Énoncé critique :** « Le tarif doit être avantageux pour les clients qui restent peu de temps, sans être ruineux pour ceux qui restent longtemps. »
- **Justification :** « Avantageux » et « non ruineux » sont des adjectifs subjectifs et qualitatifs. Sans grille tarifaire chiffrée, sans seuils de durées (définition d'un séjour court vs long) et sans montant plafond validé par la directrice, il est impossible de tester formellement si l'exigence est satisfaite.

---

## 2. Périmètre

### Q5. Tableau Connu / Inconnu

| Élément | Catégorie | Source ou Justification |
| :--- | :--- | :--- |
| Enregistrer chaque véhicule à l'entrée | Dans le périmètre | Phrase 2 (« À l’entrée, le système doit enregistrer chaque véhicule ») |
| Calculer le montant à payer en sortie | Dans le périmètre | Phrase 2 (« à la sortie, il doit calculer le montant à payer. ») |
| Permettre aux agents de traiter les cas particuliers | Dans le périmètre (à préciser) | Phrase 3 (« Les agents doivent pouvoir traiter les cas particuliers ») |
| Offrir le parking aux bons clients par les boutiques | Dans le périmètre (à préciser) | Phrase 3 (« les boutiques veulent pouvoir offrir le parking... ») |
| Suivi de l'occupation du parking par la directrice | Dans le périmètre | Phrase 3 (« La directrice veut suivre l’occupation du parking. ») |
| Arrêter les abus de ré-entrée immédiate | Dans le périmètre (règle) | Phrase 5 (« ressortent juste avant la fin... il faut arrêter ces abus. ») |
| Fiabilité et fonctionnement en temps réel | Contrainte (BNF) | Phrase 7 (« Le logiciel doit être fiable et fonctionner en temps réel. ») |
| Véhicules identifiés par reconnaissance de plaque (LAPI) | Hypothèse | À valider (Le texte ne cite que des tickets physiques). |
| Les boutiques disposent d'un terminal web dédié | Hypothèse | À valider (Le canal d'enregistrement de l'offre n'est pas spécifié). |
| Durée exacte de la franchise de gratuité initiale | Point ouvert | À valider auprès du client (durée non chiffrée dans la demande). |
| Délai de carence minimal anti-abus avant ré-entrée | Point ouvert | À valider auprès du client (délai d'attente non défini). |
| Arbitrage du prix ticket perdu : forfait vs réel | Point ouvert | À valider (Contradiction directe entre agents et boutiques). |
| Comportement en cas de parking plein (samedi) | Point ouvert | À valider (Faut-il refuser physiquement l'accès à l'entrée ?) |

### Q6. Analyse Hors Périmètre
- **Élément hors périmètre :** La réservation à distance d'une place de parking en amont via une application mobile grand public.
- **Justification :** La demande se limite strictement aux événements d'accès physiques (« À l’entrée... à la sortie... »). Aucune fonctionnalité de réservation préalable n'est justifiée par le texte.
- **Rigueur méthodologique :** L'encaissement monétaire automatique (borne CB / espèces) est classé en point ouvert : le texte indique « calculer le montant », mais ne précise pas si le logiciel pilote les automates bancaires ou transmet le montant à un système de paiement tiers.

---

## 3. Exigences

### Q7. Tableau des Premières Exigences Spécifiées

| ID | Exigence (« Le système doit... ») | Type | Source dans la demande |
| :--- | :--- | :--- | :--- |
| **REQ-F-01** | Le système doit enregistrer l'horodatage d'arrivée et l'identifiant du véhicule lors du passage à la borne d'entrée. | F | Phrase 2 (« À l’entrée, le système doit enregistrer chaque véhicule ») |
| **REQ-F-02** | Le système doit calculer le montant dû par l'automobiliste à la borne de sortie selon sa durée effective de stationnement. | F | Phrase 2 (« à la sortie, il doit calculer le montant à payer. ») |
| **REQ-F-03** | Le système doit permettre d'appliquer une exonération tarifaire (totale ou partielle) sur le stationnement du client d'une boutique. | F | Phrase 3 (« les boutiques veulent pouvoir offrir le parking... ») |
| **REQ-F-04** | Le système doit afficher à la directrice le nombre de places occupées et disponibles en temps réel. | F | Phrase 3 (« La directrice veut suivre l’occupation du parking. ») |
| **REQ-F-05** | Le système doit refuser l'application d'une nouvelle gratuité si le véhicule ré-entre dans un délai inférieur à [délai à valider] après sa sortie. | F | Phrase 5 (« ressortent juste avant la fin... il faut arrêter ces abus. ») |
| **REQ-NF-01** | Le système doit actualiser le compteur d'occupation en moins de [1 seconde à valider] après chaque franchissement d'entrée ou de sortie. | NF | Phrase 7 (« Le logiciel doit être fiable et fonctionner en temps réel. ») |

### Q8. Critique et Correction des Exigences Ambiguës

#### 1. Critique de : « Les agents doivent pouvoir traiter les cas particuliers »
- **Problème :** « Cas particuliers » est une formule vague et non atomique. On ignore la liste des exceptions autorisées.
- **Point ouvert :** « Quelles sont les exceptions exactes qu'un agent doit pouvoir régulariser (ticket perdu, ticket illisible, panne de barrière, litige), et avec quelles dérogations tarifaires autorisées ? »

#### 2. Critique de : « Le logiciel doit être fiable et fonctionner en temps réel »
- **Problème :** Les adjectifs « fiable » et « temps réel » sont qualitatifs et non mesurables.
- **Versions corrigées :**
  - **REQ-NF-02a (Disponibilité) :** Le système doit présenter un taux de disponibilité opérationnelle d'au moins [99.8% à valider] sur les heures d'ouverture du centre.
  - **REQ-NF-02b (Performance) :** Le système doit valider le ticket et déclencher l'ouverture de la barrière en moins de [800 ms à valider] après l'insertion/scan.

### Q9. Analyse de « Tarif avantageux sans être ruineux »
- **Pourquoi ce n'est pas vérifiable :** Il n'existe aucune formule mathématique ni barème chiffré dans la demande.
- **Questions à poser au client :**
  1. Quelle est la durée exacte de la période de gratuité initiale (ex. 30 min, 1h) ?
  2. Quelle est la formule de tarification horaire par tranche au-delà de la gratuité ?
  3. Existe-t-il un tarif plafond journalier à ne jamais dépasser pour les longues durées ?

---

## 4. Analyse Critique Assistée par IA

### Q10. Tableau de Classement des Propositions de l'IA

| Proposition de l'IA | Décision | Justification (Preuve dans la demande ?) |
| :--- | :--- | :--- |
| **1. Conflit sur le ticket perdu :** Définir la règle d'arbitrage entre forfait unique et temps réel. | Acceptée | Directement appuyée sur la phrase 6 (conflit explicite entre agents et boutiques). |
| **2. Délai anti-abus (anti-passback) :** Fixer une durée minimale d'absence avant une nouvelle gratuité. | Acceptée | Directement issue de la phrase 5 (« ressortent juste avant... il faut arrêter ces abus »). |
| **3. Identification par caméras LAPI :** Installer des caméras de lecture automatique des plaques. | Rejetée / À valider | Choix technique prématuré non demandé par le client (le texte cite uniquement des « tickets »). |
| **4. Portail de fidélité boutique :** Créer une interface web pour enregistrer les réductions boutiques. | À valider | Plausible pour offrir le parking, mais le canal technique n'a pas été défini par le client. |
| **5. Capteurs lumineux de place :** Installer des voyants vert/rouge au-dessus de chaque place. | Rejetée | Fonctionnalité totalement inventée et hors sujet (le besoin porte sur l'occupation globale du parking). |

### Q11. Analyse de la Proposition Plausible mais Inventée
- **Proposition identifiée :** L'installation de caméras à reconnaissance automatique des plaques d'immatriculation (LAPI) aux bornes pour bloquer automatiquement les véhicules ré-entrants.
- **Pourquoi elle est convaincante :** Elle règle de façon technologique moderne le problème des allers-retours frauduleux sans dépendre du jet d'un ticket physique.
- **Pourquoi elle est dangereuse sans vérification :**
  1. Surcoût financier majeur en caméras industrielles et serveurs OCR non budgétés.
  2. Contraintes juridiques et RGPD strictes sur le traitement des données d'immatriculation.
  3. Risque de dérive de périmètre (*Scope Creep*) éloignant l'équipe du besoin logiciel initial.

### Q12. Ambiguïté Majeure Révélée
- **Ambiguïté centrale :** Le conflit direct d'intérêts sur le ticket perdu entre les agents (forfait dissuasif pour traiter vite les litiges) et les boutiques (temps réel pour préserver la relation client). Ce conflit bloque la spécification de l'algorithme tant que la direction générale n'a pas tranché formellement.