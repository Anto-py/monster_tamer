# CLAUDE.md — MONSTER TAMER : Le Défi du 1:1

## Contexte pédagogique

Application ludique destinée aux **enseignants et formateurs du CRBTP (Centre de Référence des Formations aux Technologies du Professionnel) à Bruxelles**, dans le cadre du déploiement 1:1 (un appareil numérique par élève).

L'objectif est de **dédramatiser les peurs face au numérique** en les matérialisant sous forme de monstres à dompter dans un jeu de type runner horizontal. Chaque monstre dompté révèle un levier pédagogique concret et ancré dans la réalité du CRBTP.

**Mode de diffusion** : lien envoyé aux enseignants, jouable seul, sans explication préalable, sur navigateur desktop ou mobile. **L'interface doit être 100% auto-explicative.**

---

## Concept de jeu

### Genre
**Runner horizontal** (side-scrolling) avec pauses d'interaction à chaque rencontre de monstre.

### Structure générale
1. **Écran d'accueil** → titre, bouton START, baseline ("Apprivoise tes peurs, libère ton potentiel pédagogique")
2. **Sélection de personnage** → grille de 6 enseignants cyberpunk illustrés (CSS/SVG)
3. **Runner** → le personnage court vers la droite sur un décor urbain bruxellois futuriste. 5 monstres à rencontrer, dans l'ordre.
4. **Séquence de combat** (pour chaque monstre) :
   - Monstre surgit et avance vers le personnage
   - Quand il franchit la zone de tir → timer de 4s démarre
   - Tir trop tôt (monstre hors zone) → raté, pas de perte de vie, on peut retirer
   - Timer expiré → monstre attaque → perte d'une vie
   - Tir dans la zone et dans le temps → succès → transformation
   - **Panneau d'explication** apparaît (peur → levier pédagogique + exemples CRBTP)
   - Bouton "Continuer" → le monstre dompté rejoint la **galerie de collection**
5. **Game Over** (si 0 vie) → message + bouton "Réessayer" (depuis le monstre en cours)
6. **Écran de fin** → collection complète des 5 monstres domptés, message de conclusion, bouton "Rejouer"

### Progression visuelle
- Barre de progression en haut (5 segments, un par monstre)
- Galerie de monstres domptés qui s'enrichit au fur et à mesure (persistante visible pendant tout le jeu)

---

## Personnages jouables

**6 enseignants cyberpunk**, dessinés en SVG inline flat, **représentatifs de la diversité** du corps enseignant : genres, origines, âges différents.

| # | Description | Accessoire cyberpunk |
|---|-------------|----------------------|
| 1 | Femme noire, 35 ans | Lunettes HUD orange, tresse augmentée |
| 2 | Homme blanc, 50 ans | Implant temporal teal, veste à circuits |
| 3 | Femme basanée, 28 ans | Bras droit mécanique, badge holographique |
| 4 | Homme noir, 42 ans | Casque audio rétro, manteau retrofuturiste |
| 5 | Femme blanche, 60 ans | Canne connectée, broches de données |
| 6 | Homme basané, 32 ans | Yeux augmentés teal, tablette intégrée |

Chaque personnage est représenté en **SVG inline** (pas d'images externes). Les carnations de peau sont rendues avec des tons précis (voir tableau ci-dessous).

| Peau | Hex |
|------|-----|
| Noire | #3D1A00 |
| Blanche | #F5D5B0 |
| Basanée | #C68642 |

**Projectile lancé** : le personnage lance un livre. Animation : arc de lancer CSS keyframes.

---

## Les 5 Monstres

### Structure de données

```javascript
const MONSTRES = [
  {
    id: "distractus",
    nom: "DISTRACTUS",
    peur: "Les élèves vont être sur TikTok au lieu de travailler",
    nomDompte: "Distractus l'Engagé",
    levier: "L'engagement bat la distraction",
    description: "Un élève distrait cherche du sens. Avec le 1:1, tu peux proposer des activités qui rivalisent avec le scroll : recherches guidées sur leur métier, création de contenu pro, simulations de chantier.",
    exemplesCRBTP: [
      "En Construction : les élèves filment et commentent leurs gestes techniques pour créer un tutoriel partagé sur Teams.",
      "En Vente : ils analysent des vraies campagnes Instagram de marques belges et créent leur propre pitch.",
      "Règle d'or : si l'activité est plus intéressante que TikTok, TikTok perd."
    ]
  },
  {
    id: "pannus",
    nom: "PANNUS",
    peur: "Je ne maîtrise pas la technique — je vais me ridiculiser devant mes élèves",
    nomDompte: "Pannus le Sage",
    levier: "L'enseignant n'est plus l'expert unique",
    description: "Ne pas tout savoir sur l'outil n'est pas une faiblesse — c'est une opportunité pédagogique. L'élève expert devient une ressource. Tu gardes le cap pédagogique, tu délègues la technique.",
    exemplesCRBTP: [
      "En Informatique de gestion : identifier les 2-3 élèves les plus à l'aise comme 'ambassadeurs numériques' de la classe.",
      "En Cuisine : tu maîtrises la recette, pas l'appli. L'élève qui gère l'outil numérique, toi tu valides le fond.",
      "Formule magique : 'Je ne sais pas, on cherche ensemble' — c'est de la modélisation cognitive en action."
    ]
  },
  {
    id: "aequalis",
    nom: "AEQUALIS",
    peur: "Certains élèves n'ont pas de connexion à la maison — c'est injuste",
    nomDompte: "Aequalis le Juste",
    levier: "L'équité numérique se construit, elle ne se subit pas",
    description: "Le 1:1 au CRBTP, c'est un appareil fourni par l'établissement. Le problème de connexion à domicile est réel, mais il ne doit pas bloquer les usages en classe — et peut devenir un projet collectif.",
    exemplesCRBTP: [
      "Tous les travaux lourds se font en classe, sur le réseau CRBTP. Le domicile = révision légère uniquement.",
      "Les apps Microsoft 365 fonctionnent hors ligne : OneNote, Word, Teams (en cache). Zéro blocage possible.",
      "Le CRBTP dispose d'une charte de prêt d'appareils — la connaître résout 80% du problème."
    ]
  },
  {
    id: "substitutor",
    nom: "SUBSTITUTOR",
    peur: "L'écran va remplacer le contact humain et le geste professionnel",
    nomDompte: "Substitutor le Médiateur",
    levier: "Le numérique prépare et prolonge le geste — il ne le remplace pas",
    description: "Dans les formations professionnelles, le geste est roi. Le numérique intervient avant (préparer, comprendre) et après (tracer, évaluer) — jamais à la place du faire.",
    exemplesCRBTP: [
      "En Électricité : simuler le schéma sur tablette avant le câblage réduit les erreurs. C'est l'échafaudage cognitif (Vygotsky).",
      "En Esthétique : vidéo d'un geste en slow-motion, analysée ensemble, puis reproduite sur mannequin. Le numérique = loupe.",
      "Règle du pouce : si l'activité numérique ne prépare ou ne prolonge pas un geste professionnel concret, questionne sa place."
    ]
  },
  {
    id: "evaluator",
    nom: "ÉVALUATOR",
    peur: "Comment savoir ce qu'ils ont vraiment fait eux-mêmes ?",
    nomDompte: "Évaluator le Révélateur",
    levier: "Évaluer le processus, pas seulement le produit",
    description: "La vraie question n'est pas 'a-t-il triché ?' mais 'est-ce qu'il sait faire ?'. Le numérique offre des traces de processus que le papier n'a jamais pu capturer.",
    exemplesCRBTP: [
      "Historique de versions sur OneNote ou Word : tu vois comment le travail a évolué, pas juste le résultat final.",
      "L'oral bref de 3 minutes après un travail numérique : 'explique-moi ton raisonnement' — imparable et formateur.",
      "Portfolio progressif sur Teams : l'élève documente son parcours depuis septembre. L'authenticité est dans la trajectoire."
    ]
  }
];
```

---

## Mécanique de combat (détail)

### Système de vies
- Le joueur commence avec **3 vies**, affichées en haut à droite sous forme de 3 icônes (cœur ou bouclier cyberpunk, SVG inline)
- Une vie est perdue dans deux cas : timer expiré OU projectile raté (monstre trop loin)
- À **0 vie** : écran "Game Over" avec message d'encouragement et bouton "Réessayer" (reprend depuis le monstre en cours, pas depuis le début)
- Les vies sont **persistantes** entre les monstres (une vie perdue sur Distractus reste perdue face à Pannus)

### Phase 1 — Approche et zone de tir
- Le personnage court en boucle (animation CSS `translateX` sur le décor en défilement)
- Le monstre surgit depuis la droite et **avance lentement** vers le personnage
- **Zone de tir valide** : définie par une distance minimale. Tant que le monstre est dans la moitié droite du stage, le tir est invalide ("trop loin")
- Indicateur visuel de la zone de tir : une ligne verticale pointillée teal qui marque le seuil. Quand le monstre la franchit, elle devient orange et pulse → signal d'action

```javascript
// Logique de distance (valeurs en % de la largeur du stage)
const ZONE_TIR = {
  monstre_spawn_x: 95,     // position initiale du monstre (% depuis la gauche)
  seuil_tir_x: 55,         // en dessous de ce seuil = tir valide
  personnage_x: 15,        // position fixe du personnage
  vitesse_approche: 0.08   // % par frame (environ 4s pour traverser le stage)
};
```

### Phase 2 — Timer de pression
- Dès que le monstre franchit la zone de tir (seuil_tir_x), un **timer de 4 secondes** démarre
- Le timer est affiché visuellement : barre de progression qui se vide (couleur jaune → orange → rouge)
- **Si le timer expire** : animation "le monstre attaque" (le monstre bondit sur le personnage, flash rouge, shake de l'écran), le joueur perd une vie, le monstre recule et recommence son approche depuis le spawn
- Le timer se reset à chaque nouvelle tentative sur le même monstre

```javascript
const TIMER_COMBAT = {
  duree_ms: 4000,
  couleurs: ['#E3A535', '#E4632E', '#CC0000'], // jaune → orange → rouge
  seuils: [0.6, 0.3]  // transitions de couleur à 60% et 30% du temps restant
};
```

### Phase 3 — Résolution du tir

#### Tir trop tôt (monstre au-delà du seuil)
- Animation : le projectile part, voyage, puis **tombe à mi-chemin** (arc qui retombe avant d'atteindre le monstre)
- Le monstre ricane (animation de rire : bounce + expression moqueuse)
- Message bref : "Trop loin !" (texte orange, disparaît après 1s)
- **Pas de perte de vie** — le joueur peut retirer immédiatement
- Le monstre continue son approche (pas de reset de position)

#### Tir dans la zone valide
- Animation du projectile (livre) : arc parabolique qui atteint le monstre
- À l'impact : flash teal, le monstre tremble (shake)

### Phase 4 — Transformation
- Le monstre "fond" (opacity 0) puis se reconstruit en version domptée
- Version agressive : couleurs chaudes (orange/rouge), expression menaçante
- Version domptée : même forme de base, couleurs teal/jaune, expression apaisée, petite auréole

### Phase 5 — Révélation pédagogique
Panneau modal (carte retrofuturiste) :
- En-tête : nom monstre barré → nom dompté
- Peur formulée à la 1re personne (texte barré orangé)
- Levier pédagogique (titre teal, Oswald)
- 3 exemples CRBTP (Inter, corps normal)
- Bouton pill "CONTINUER →"

### Phase 6 — Collection
- Miniature du monstre dompté s'ajoute à la galerie en bas de l'écran
- Animation d'ajout : `scale(0) → scale(1.2) → scale(1)` avec glow teal

---

## Écran Game Over

Déclenché quand les 3 vies sont épuisées.

- Fond ink, titre "GAME OVER" en Oswald orange
- Sous-titre encourageant : "Les monstres résistent... mais tu n'as pas dit ton dernier mot."
- Affichage des monstres déjà domptés (collection partielle)
- **Bouton "RÉESSAYER"** → reprend depuis le monstre en cours (pas depuis le début), avec 3 vies rechargées
- **Bouton "RECOMMENCER"** → retour à la sélection de personnage

---

## Design visuel

### Charte : Retrofuturisme

Appliquer strictement la charte définie dans `/mnt/skills/user/retrofuturisme/SKILL.md`.

**Palette :**
```css
--retro-teal:   #127676;
--retro-orange: #E4632E;
--retro-jaune:  #E3A535;
--retro-ink:    #0D1617;
--retro-paper:  #F2EFE6;
```

**Typographie (Google Fonts CDN) :**
- Titres : `Oswald`, fallback `Impact, sans-serif`
- Corps : `Inter`, fallback `system-ui, sans-serif`

### Fond du runner
Décor urbain bruxellois stylisé en SVG inline :
- Silhouettes d'immeubles Art Nouveau (inspiration Horta : courbes, bow-windows)
- Lampadaires néon teal
- Ciel en dégradé `ink → teal` (haut → bas)
- Défilement en parallaxe simple (arrière-plan lent, sol rapide)

### Monstres (visuels SVG inline)
Chaque monstre est un SVG construit à partir de formes géométriques simples.

| Monstre | Forme dominante | Couleur agressive | Couleur domptée |
|---------|----------------|-------------------|-----------------|
| Distractus | Téléphone géant avec tentacules | #E4632E | #127676 |
| Pannus | Engrenage avec visage paniqué | #8B0000 | #E3A535 |
| Aequalis | Balance déséquilibrée | #4B0082 | #127676 |
| Substitutor | Robot avec cœur barré | #006400 | #E3A535 |
| Évaluator | Œil géant avec point d'interrogation | #8B4513 | #127676 |

### Interface générale
- Fond global : `--retro-ink`
- Barre de progression : 5 segments qui passent de ink à teal
- Galerie de collection : bandeau `--retro-paper` en bas, bordure `--retro-teal`
- Boutons : style pill (jaune texte + icône orange) conformément à la charte
- Cartes modales : bordure 3px teal, double bordure interne orange, fond paper, coins asymétriques `24px 8px 24px 8px`

---

## Architecture technique

### Stack
- **HTML/CSS/JS pur, fichier unique** : `index.html`
- Google Fonts via CDN
- **Zéro dépendance JS externe** (pas de jQuery, pas de framework, pas de librairie de jeu)
- Tout le contenu (données monstres, SVG, logique) est **inline dans le fichier**

### Structure du fichier
```
index.html
├── <head>
│   ├── meta charset, viewport, title
│   ├── Google Fonts link (Oswald + Inter)
│   └── <style> : CSS complet inline
├── <body>
│   ├── #screen-welcome    — Écran d'accueil
│   ├── #screen-select     — Sélection du personnage
│   ├── #screen-game       — Zone de jeu principale
│   │   ├── #hud           — Vies (3 icônes) + barre de progression + timer
│   │   ├── #runner-stage  — Canvas de jeu (décor + personnage + monstre)
│   │   ├── #modal-reveal  — Panneau pédagogique (hidden par défaut)
│   │   └── #collection    — Galerie des monstres domptés
│   ├── #screen-gameover   — Game Over (0 vie)
│   └── #screen-end        — Écran de fin
└── <script> : logique JS complète inline
```

### Machine à états
```javascript
const STATES = {
  WELCOME:       'welcome',     // écran d'accueil
  SELECT:        'select',      // sélection personnage
  RUNNING:       'running',     // runner actif, monstre hors zone
  ENCOUNTER:     'encounter',   // monstre dans la zone, timer actif
  THROWING_OK:   'throwing_ok',    // tir valide, animation en cours
  THROWING_MISS: 'throwing_miss',  // tir raté (trop tôt) — cosmétique, pas de perte de vie
  TIMEOUT:       'timeout',     // timer expiré, monstre attaque
  TAMING:        'taming',      // animation transformation
  REVEALING:     'revealing',   // panneau pédagogique affiché
  GAMEOVER:      'gameover',    // 0 vie restante
  END:           'end'          // tous monstres domptés
};

// État du joueur
const playerState = {
  vies: 3,
  monstreActuel: 0,  // index dans MONSTRES[]
  collection: [],    // ids des monstres domptés
  monstreX: 95       // position courante du monstre en % du stage
};
```

### Animations
Uniquement CSS `@keyframes` et `transition` — aucun canvas HTML5, aucun JS d'animation.

| Animation | Technique |
|-----------|-----------|
| Runner qui court | `@keyframes` sur les jambes SVG |
| Défilement du décor | `@keyframes translateX` en boucle sur le SVG de fond |
| Monstre surgit | `scale(0) → scale(1.2) → scale(1)` + shake |
| Monstre avance | `setInterval` sur `monstreX` → `left` CSS du SVG monstre |
| Timer (barre) | `transition width` + changement de couleur via JS |
| Projectile valide | `@keyframes` arc parabolique jusqu'au monstre |
| Projectile raté | `@keyframes` arc court qui retombe à mi-chemin |
| Monstre ricane | `@keyframes` bounce + expression JS (swap SVG face) |
| Monstre attaque | `translateX` vers le personnage + flash rouge + screen shake |
| Perte de vie | Icône cœur passe en grisé + animation `scale(0)` |
| Transformation | `opacity 0 → 1` avec `filter: hue-rotate()` |
| Ajout collection | `scale(0) → scale(1.1) → scale(1)` avec glow teal |

### Responsive
- Priorité **desktop** (largeur cible : 900px max, centré)
- Fonctionnel sur mobile : layout adaptatif, touch events sur le stage de jeu
- Pas de media queries complexes — simplicité prioritaire

---

## Déploiement GitHub Pages

### Structure du repo
```
/
└── index.html    ← fichier unique
```

### Configuration
- Branch : `main`
- Source : `/ (root)`
- URL : `https://[username].github.io/[repo-name]/`

### Checklist pré-déploiement
- [ ] Aucune dépendance locale (tout CDN ou inline)
- [ ] Google Fonts avec fallback explicite
- [ ] Test navigateur : Chrome, Firefox, Safari, Edge
- [ ] Test mobile : iOS Safari, Chrome Android
- [ ] Zéro `localhost`, zéro chemin absolu
- [ ] Fonctionne en double-clic local sur index.html

---

## Contenu éditorial — Règles de ton

- **Langue** : français, professionnel mais chaleureux
- **Peurs** : à la 1re personne ("Je ne maîtrise pas...") pour que les enseignants se reconnaissent
- **Leviers** : affirmations positives, jamais culpabilisantes
- **Exemples** : nommer les sections du CRBTP (Construction, Cuisine, Vente, Électricité, Esthétique, Informatique de gestion)
- **Références pédagogiques** : citées légèrement entre parenthèses (Vygotsky, Dehaene) pour crédibiliser sans alourdir
- **Humour** : permis dans les noms de monstres et leurs descriptions agressives

---

## Ce que ce jeu ne fait PAS

- Pas de score numérique
- Pas de timer de pression
- Pas de mauvaise réponse possible (un seul clic suffit)
- Pas de login ni collecte de données
- Pas de son (usage sans casque dans des espaces partagés)
- Pas de backend

---

## Livrables attendus

1. `index.html` — fichier unique, déployable immédiatement sur GitHub Pages
2. Commentaires de code en français
3. README.md minimal (titre, description, lien démo, instructions déploiement)

---

## Critères de succès

- Un enseignant qui reçoit le lien comprend quoi faire en moins de 10 secondes
- Le jeu est finissable en 3-4 minutes
- Chaque panneau donne au moins un exemple immédiatement actionnable au CRBTP
- Le design est cohérent avec la charte retrofuturiste
- Le fichier fonctionne localement avant tout déploiement
