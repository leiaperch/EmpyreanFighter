# EMPYREAN FIGHTER — Rebelles vs Loyalistes

Jeu de combat 2D dark fantasy, dans l'esprit de Street Fighter, jouable sur PC dans n'importe quel navigateur moderne
(Chrome, Edge, Firefox). Aucune installation, aucun serveur : **double-cliquer sur `index.html`**.

## Lancer le jeu

- Ouvrir `index.html` dans le navigateur.
- Écran titre : ↑/↓ choisit le mode (1 joueur vs IA / 2 joueurs), ←/→ la difficulté de l'IA,
  Entrée / Espace / Start lance la sélection.
- Sélection : ←/→ pour choisir, n'importe quel bouton d'attaque pour valider. Échap : retour.
- Match au meilleur des 3 rounds, 99 s par round.

## Commandes (clavier, remappables)

Écran titre → **TOUCHES** : choisir un préréglage par joueur (← →) ou redéfinir chaque touche
(Entrée puis la touche voulue). Sauvegardé dans le navigateur. Suppr remet les valeurs par défaut.
Les touches sont **physiques** : le jeu affiche ce qui est écrit sur *votre* clavier (AZERTY ou QWERTY).

| Préréglage             | Déplacer          | Léger | Lourd | Pied | Spécial | Start        |
|------------------------|-------------------|-------|-------|------|---------|--------------|
| Flèches + AZER (P1 défaut) | ← → ↑ ↓        | A     | Z     | E    | R       | Entrée       |
| Flèches + pavé num. (P2 défaut) | ← → ↑ ↓   | Num 1 ou `;` | Num 2 ou `:` | Num 3 ou `!` | Num 5 ou Maj droite | Entrée num. |
| ZQSD + FGHJ            | Z Q S D           | F     | G     | H    | J       | Espace       |
| ZQSD + JKL I           | Z Q S D           | J     | K     | L    | I       | Espace       |

En **2 joueurs**, si P1 et P2 partagent des touches (flèches), P1 bascule automatiquement sur
« ZQSD + FGHJ ». Une manette (optionnelle) est aussi reconnue : stick/croix, X léger, Y lourd,
A pied, B ou RB spécial, Start. **M** coupe la musique, **Échap** revient au titre.

### Techniques

- **Bloquer** : reculer. **Bloquer bas** : accroupi + reculer. Les coups bas se bloquent
  accroupi, les coups sautés / overheads se bloquent debout.
- **Balayette** (met à terre) : accroupi + pied. **Anti-air** (envoie en l'air) : accroupi + lourd.
- **Spécial** : ↓↘→ + n'importe quel coup, ou la touche Spécial directement.
- **Super** (jauge pleine, la barre clignote) : accroupi + Spécial.
- Un bouton pressé pendant la fin d'un coup est mémorisé (buffer de 12 frames) et part dès
  que le personnage est libre : on peut enchaîner sans « rater » d'entrée.
- Les combos réduisent les dégâts (−12 % par coup, plancher 30 %). Un adversaire acculé dans
  le coin repousse l'attaquant.

## Jouer en ligne (deux PC)

Écran titre → **EN LIGNE**. Un joueur choisit **Héberger** et obtient un code de 5 caractères ;
l'autre choisit **Rejoindre** et le saisit. Connexion directe entre les deux navigateurs
(WebRTC via PeerJS, aucun serveur de jeu) ; l'hôte est P1, l'invité P2, chacun joue avec ses
touches « Joueur 1 ».

- Netcode **lockstep** : chaque PC simule le même combat et n'échange que les entrées, retardées
  d'un délai fixé au ping mesuré à la connexion (2 à 8 frames, soit 33 à 133 ms). Un contrôle
  d'état toutes les 60 frames signale une éventuelle désynchronisation.
- Il faut internet pour la mise en relation (serveur PeerJS public) ; ensuite le trafic est P2P.
  Derrière un réseau très fermé (entreprise, 4G stricte) la connexion peut échouer : essayez en
  inversant hôte / invité.
- Fonctionne avec le fichier `index.html` / `empyrean-fighter-standalone.html` ouvert dans
  Chrome, Edge ou Firefox. **Pas dans l'Artifact Claude** (les connexions réseau y sont bloquées).

## Roster

| Faction    | Personnage | Rôle        | Arme / pouvoir                 | Spécial (↓↘→ + coup ou Spécial) | Super (jauge pleine, ↓ + Spécial)     |
|------------|------------|-------------|--------------------------------|----------------------------------|----------------------------------------|
| Rebelles   | EIRA       | Half Caster | Hache, ondes de choc           | Onde de choc (vague au sol)      | Séisme (onde des deux côtés)           |
| Rebelles   | DANTE      | Warrior     | Énorme épée, monstre de lave   | Éruption (avec armure)           | Forme de lave (15 s : +30 % dégâts, aura) |
| Loyalistes | SAERYNN    | Caster      | Épée longue, contrôle du métal | Éclats d'acier (projectile)      | Forêt de lames (pics sur 560 px)       |
| Loyalistes | CASSIUS    | Half Caster | Épée noire, ombres             | Pas d'ombre (ruée)               | Nuit tombée (6 coups, écran assombri)  |

Personnages créés sur Artlist par l'auteur ; planches d'animation (9 actions × 8 frames chacun)
et VFX (8 effets × 8 frames) générés par image-à-image à partir de ces références.

## Pipeline d'animation

- `assets/sheets/anim/<perso>_<action>.png` : planches 2K, grille 4×2, fond vert (sources).
- `python .claude/cut_sheet.py <planche> <perso> <action>` : chroma key, découpe par gouttières,
  échelle uniforme, export WebP dans `assets/anim/<perso>/<action>/f1..f8.webp`.
- `python .claude/cut_vfx.py assets/sheets/anim/vfx_<nom>.png <nom>` : VFX sur fond noir → alpha
  par luminance (rendu additif) dans `assets/vfx/<nom>/`.
- `python .claude/gen_meta.py` : manifeste `assets/anim_meta.js` (taille et ancrage de chaque frame).
- `python .claude/build.py` : bundle mono-fichier avec tous les assets en data URI.

## Assets (générés avec Artlist)

- `assets/stage_rebels.jpg`, `assets/stage_loyalists.jpg` — décors des deux factions.
- `assets/portrait_*.jpg` — portraits de sélection (recadrés depuis les créations Artlist de l'auteur).
- `assets/anim/`, `assets/vfx/` — frames d'animation et d'effets (WebP).
- `assets/theme.mp3` — thème de combat (boucle de 30 s).

Le jeu reste jouable sans ces fichiers (décor et portraits de repli dessinés par le code).

## Structure

- `index.html` — tout le jeu (moteur, personnages, IA, rendu, audio synthétisé pour les SFX).
- `GDD.md` — document de design : piliers, boucle de jeu, fiches de coups, valeurs à équilibrer.
