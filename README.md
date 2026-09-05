# EMPYREAN FIGHTER — Rebelles vs Loyalistes

Jeu de combat 2D dark fantasy, dans l'esprit de Street Fighter, jouable sur PC dans n'importe quel navigateur moderne
(Chrome, Edge, Firefox). Aucune installation, aucun serveur : **double-cliquer sur `index.html`**.

## Lancer le jeu

- Ouvrir `index.html` dans le navigateur.
- Écran titre : ↑/↓ choisit le mode (1 joueur vs IA / 2 joueurs), ←/→ la difficulté de l'IA,
  Entrée / Espace / Start lance la sélection.
- Sélection : ←/→ pour choisir, n'importe quel bouton d'attaque pour valider. Échap : retour.
- Match au meilleur des 3 rounds, 99 s par round.

## Commandes

| Action            | Joueur 1 (clavier)      | Joueur 2 (clavier)        | Manette (P1 = manette 1, P2 = manette 2) |
|-------------------|-------------------------|---------------------------|------------------------------------------|
| Déplacement       | A / D (Q / D en AZERTY) | ← / →                     | Stick gauche / croix                     |
| Saut              | W (Z en AZERTY)         | ↑                         | Haut                                     |
| S'accroupir       | S                       | ↓                         | Bas                                      |
| Coup léger        | J                       | Pavé num. 1               | X (bouton 2)                             |
| Coup lourd        | K                       | Pavé num. 2               | Y (bouton 3)                             |
| Coup de pied      | L                       | Pavé num. 3               | A (bouton 0)                             |
| Spécial           | I                       | Pavé num. 5               | B (bouton 1) ou RB                       |
| Start             | Entrée / Espace         | Pavé num. 0 / Entrée num. | Start                                    |
| Musique on/off    | M                       |                           |                                          |

Les touches clavier sont détectées par **position physique** : WASD reste au même endroit
sur un clavier AZERTY (Z-Q-S-D).

### Techniques

- **Bloquer** : reculer. **Bloquer bas** : ↓ + reculer. Les coups bas doivent être bloqués
  accroupi, les coups sautés / overheads doivent être bloqués debout.
- **Balayette** (met à terre) : ↓ + coup de pied. **Anti-air** (envoie en l'air) : ↓ + lourd.
- **Spécial** : ↓↘→ + n'importe quel coup, ou la touche Spécial directement.
- **Super** (jauge pleine, la barre clignote) : ↓ + Spécial.
- Les combos réduisent les dégâts (−12 % par coup, plancher 30 %). Un adversaire acculé dans
  le coin repousse l'attaquant.

## Roster

| Faction      | Personnage | Style                        | Spécial                              | Super                        |
|--------------|------------|------------------------------|--------------------------------------|------------------------------|
| Rebelles     | KAEL       | Rushdown rapide, 1000 PV     | Tempête de rue — ruée poing          | Émeute — ruée 6 coups        |
| Rebelles     | MIRA       | Zoneuse, 900 PV              | Charge éclair — bombe en cloche      | Feu d'artifice — 3 impacts   |
| Loyalistes   | DRACE      | Lourd, 1200 PV, lent         | Marteau impérial — frappe avec armure| Jugement royal — onde de choc|
| Loyalistes   | SERAPH     | Équilibrée, 1000 PV          | Lance solaire — projectile rapide    | Aube éternelle — rayon plein écran |

## Assets (générés avec Artlist)

- `assets/stage_rebels.jpg`, `assets/stage_loyalists.jpg` — décors des deux factions.
- `assets/portrait_*.jpg` — portraits de sélection.
- `assets/theme.mp3` — thème de combat (boucle de 30 s).

Le jeu reste jouable sans ces fichiers (décor et portraits de repli dessinés par le code).

## Structure

- `index.html` — tout le jeu (moteur, personnages, IA, rendu, audio synthétisé pour les SFX).
- `GDD.md` — document de design : piliers, boucle de jeu, fiches de coups, valeurs à équilibrer.
