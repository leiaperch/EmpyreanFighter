# GDD — EMPYREAN FIGHTER

Version 0.1 — 2026-09-05. Toutes les valeurs numériques sont des hypothèses `[PLACEHOLDER]`
à confirmer en playtest ; elles vivent dans `CHARS` (`index.html`) et nulle part ailleurs.

## Piliers

1. **Lisible en 3 secondes** — deux couleurs de faction (rouge / bleu), une silhouette par
   personnage, chaque coup a une pose distincte.
2. **Le blocage est une décision** — haut / bas / overhead comme dans SF2 ; on ne gagne pas en
   martelant, on gagne en lisant.
3. **Chaque faction a une identité** — Rebelles : mobilité et pression ; Loyalistes : masse,
   armure et contrôle d'espace.
4. **Jouable tout de suite** — un fichier, un navigateur, clavier ou manette, 1P ou 2P.

## Boucle de jeu

- **Instant (0–3 s)** : approcher / reculer / sauter → frapper ou bloquer → feedback
  (hitstop, étincelles, tremblement, son) → jauge qui monte.
- **Round (≤ 99 s)** : vider la barre adverse. Tension : la jauge de super (300 pts) qui
  s'accumule des deux côtés, et le coin de l'écran.
- **Match** : meilleur des 3 rounds, écran de victoire de faction, retour sélection.

Hypothèse de fun : *le hitstop + la pose d'impact + le son lourd rendent chaque coup lourd
satisfaisant même avec des personnages en formes simples.*

## Mécaniques

### Frame data
Toutes en frames à 60 Hz : `startup` (avant que la boîte ne soit active), `active`,
`recovery`. Chaque coup a `hitstun` / `blockstun` (frames d'immobilisation du défenseur) et
`kb` (recul). Un jab (4/3/8, hitstun 14) laisse +4 : on peut enchaîner des jabs, ce que le
scaling et la repoussée de coin limitent volontairement.

### Blocage
Tenir arrière. Coup `low` → doit être bloqué accroupi. Coup `overhead` (tous les coups
aériens, le Marteau de Drace) → doit être bloqué debout. Bloquer inflige 8 % des dégâts (chip)
et donne 6 pts de jauge au défenseur, 8 à l'attaquant.

### Jauge de super (300)
Sources : dégâts infligés × 0.4 (ou `meterGain` du spécial), dégâts subis × 0.25, blocage.
Sink : le super la vide entièrement. Conservée entre les rounds.

### Coups multi-impacts
`dmg` d'un coup `multi` est le **total** ; chaque impact inflige `dmg / multi` (puis scaling).
Un super plein (300–360) vaut donc 30–35 % d'une barre, jamais un K.O. depuis 100 %.

### Scaling de combo
`dmg × max(0.3, 1 − 0.12 × hitsDéjàDansLeCombo)`. Défini comme « cassé » si un combo
atteint > 45 % de la barre sans super : repasser le coefficient à 0.15.

### Armure (Drace)
Pendant startup + active du spécial/super : subit 50 % des dégâts, aucun stun. Contre-jeu :
balayette (met à terre) ou projectile en retrait.

### En ligne (lockstep P2P)
Simulation déterministe pilotée uniquement par les entrées (pas fixe 60 Hz, aucun `Math.random`
dans le gameplay hors IA — le stage vient d'une graine envoyée par l'hôte). Délai d'entrée
`delay = clamp(ceil(RTT/2/16.7 ms) + 1, 2, 8)`. Si l'entrée distante manque, la simulation
s'arrête (⏳) plutôt que de diverger. Rollback : hors périmètre v0.4, à envisager si le délai
gêne au-delà de 80 ms de ping.

### IA (3 niveaux)
Réaction 34 / 20 / 9 frames, agressivité 0.3 / 0.5 / 0.75. Saute ou recule devant un
projectile, bloque (haut/bas selon le coup) à 55 % + 15 %/niveau, anti-air quand on saute,
**pause forcée après 3 coups de combo** pour laisser une fenêtre de contre-attaque.

## Animation et VFX
Chaque action a 8 frames ; les coups mappent startup → frames 0..k1, active → k1+1..k2,
recovery → k2+1..7 (`frames` par coup, défaut [3,5]). Les multi-hits bouclent sur la plage active.
Le saut choisit sa frame d'après la vitesse verticale, la chute d'après `vy`, le sol d'après
l'état (knockdown = frame 8 de `hurt`). VFX en rendu additif (`lighter`), déclenchés au
premier frame actif du coup ; les projectiles VFX avancent leur frame avec leur durée de vie.

## Roster — leviers d'équilibrage

| Perso   | PV   | Marche | Rôle        | Levier principal                                  |
|---------|------|--------|-------------|---------------------------------------------------|
| Eira    | 1050 | 3.8    | Half Caster | Vitesse / durée de l'onde de choc (6.5, 64 f)     |
| Dante   | 1150 | 3.1    | Warrior     | Durée de la forme de lave (900 f), bonus ×1.3     |
| Saerynn | 1000 | 3.6    | Caster      | Vitesse des éclats (9), portée des pics (560 px)  |
| Cassius | 950  | 4.3    | Half Caster | Distance de la ruée (11 × 12 f), hits du super (6)|

## Playtest — critères de succès v0.1

- Un joueur qui bloque correctement ne meurt pas en moins de 20 s contre l'IA Normal.
- Aucun combo infini dans le coin (vérifier la repoussée).
- Les 4 spéciaux et 4 supers se lancent au clavier et à la manette, en 1P et en 2P.
- Le super de Seraph (rayon plein écran) ne doit pas être une victoire garantie : blockstun 20,
  bloquable debout ou accroupi.

## Onboarding — tutoriel par personnage
Mannequin `TutorAI` piloté par l'étape courante (`idle` / `attack` / `sweep`). Validation par
observation de l'état du joueur (déplacement, saut, coups touchés via `resolveHit`, blocages,
spécial/super déclenchés). Aucun échec possible : PV régénérés à chaque frame, chrono figé.
Conseils finaux = `CHAR_TIPS` (deux phrases par personnage : outil principal, usage du super).

## Changelog

- 0.6 (2026-09-05) : mode tutoriel, découpage par composantes (lames non tronquées), VFX sans voile.
- 0.5.1 (2026-09-05) : animation de marche arrière dédiée (walkback).
- 0.5 (2026-09-05) : roster final (Eira, Dante, Saerynn, Cassius), sprites animés 9 actions × 8 frames, VFX en frames, forme de lave de Dante.

- 0.4 (2026-09-05) : mode en ligne P2P (PeerJS), écran Touches et préréglages clavier PC, buffer d'entrée.

- 0.1 (2026-09-05) : première version jouable. Ajout scaling de combo, repoussée de coin et
  pause IA après combo suite au premier playtest (IA Normal tuait en 4 s dans le coin).
