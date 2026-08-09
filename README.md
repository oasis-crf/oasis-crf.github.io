# OASIS — Communiquer dans votre langue

Petite application (une page web) pour l'accueil OASIS de la Croix-Rouge : on choisit
la langue de la personne en face en cliquant sur un drapeau, puis on lui montre les
phrases traduites et on peut écouter l'enregistrement audio de chacune.

Tout tient dans un dossier, **sans base de données ni serveur**. Les langues, les
traductions et les liens audio vivent dans un tableur ; la page web les lit toute seule.

## Contenu du dossier

```
index.html      ← l'application (ne demande aucune installation)
data.xlsx       ← la base de données locale (secours si le Google Sheet est indisponible)
flags/          ← les drapeaux (43 pays), pour que le site soit rapide
audio/          ← les fichiers son (.mp3), à déposer ici
README.md       ← ce guide
```

## Utilisation au quotidien

Ouvrez le site, tapez le début du nom de la langue ou faites défiler, cliquez sur la
vignette. Vous voyez les 6 phrases : le texte français (pour vous repérer), la
traduction en grand (pour la personne), et un bouton **Écouter** quand l'audio existe.

---

# 1. Mettre le site en ligne — adresse `oasis-crf`

L'objectif : une adresse au nom de l'association, **pas à votre nom**, et sans toucher
au site que vous avez déjà sur votre compte GitHub.

La solution : une **organisation GitHub** appelée `oasis-crf`. Une organisation est
gratuite, elle a ses propres dépôts et sa propre adresse. Votre compte personnel et
votre site actuel ne bougent pas ; vous en êtes simplement administratrice.

Adresse finale : **https://oasis-crf.github.io**

### Les étapes

1. ✅ **Organisation créée** : `oasis-crf`.

2. **Renommer le dépôt.** Pour que l'adresse soit la version courte
   (`oasis-crf.github.io` et non `oasis-crf.github.io/oasis-crf/`), le dépôt doit
   porter **exactement** le nom de l'organisation suivi de `.github.io`.
   Dépôt → **Settings** → **General** → *Repository name* →
   remplacer `oasis-crf` par **`oasis-crf.github.io`** → **Rename**.
   GitHub redirige automatiquement l'ancien nom, rien ne casse.

3. **Envoyer les fichiers.** Depuis ce dossier :

   ```bash
   git push -u origin main
   ```

   (Le dépôt local est déjà prêt : branche `main`, fichiers ajoutés, `origin`
   configuré.) GitHub demandera vos identifiants : utilisez votre nom d'utilisateur
   et un **jeton d'accès personnel** comme mot de passe — GitHub n'accepte plus le
   mot de passe du compte. Créez-le sur
   *Settings → Developer settings → Personal access tokens → Fine-grained tokens*,
   avec accès en écriture au dépôt.

4. **Activer la publication.** Dépôt → **Settings** → **Pages** →
   *Source* : « Deploy from a branch » → branche `main`, dossier `/ (root)` → **Save**.

5. **Attendre une minute**, puis ouvrir **https://oasis-crf.github.io** 🎉

> **Nom de domaine personnalisé (optionnel).** Si vous préférez `oasiscroixrouge.fr`
> (environ 10 €/an chez un registrar), il se branche sur GitHub Pages en ajoutant le
> domaine dans Settings → Pages → *Custom domain*. Pas nécessaire pour démarrer.

> **Avant de rendre le site public :** le nom et l'emblème « Croix-Rouge » sont
> protégés. Faites valider la mise en ligne par votre délégation — c'est une formalité,
> mais elle vaut mieux avant qu'après.

---

# 2. Les traductions sur Google Drive (modifiables par les bénévoles)

Par défaut, le site lit `data.xlsx`, qui est *dans* le site : le modifier demande de
repasser par GitHub. Pour que **plusieurs bénévoles puissent corriger les traductions
directement**, on met la base sur Google Sheets. Le site la relit à chaque
ouverture de page : une correction dans le tableur est visible tout de suite.

### Mise en place (une seule fois)

1. **Importer le tableur.** Sur [drive.google.com](https://drive.google.com) →
   **Nouveau** → *Importer un fichier* → choisissez `data.xlsx`.
   Puis clic droit sur le fichier importé → *Ouvrir avec* → **Google Sheets** →
   Fichier → **Enregistrer au format Google Sheets**.
   Vérifiez que les trois onglets sont bien là : `Mode d'emploi`, `Phrases`,
   `Traductions`. **Ne renommez pas les onglets**, le site les cherche par leur nom.

2. **Partager en lecture.** Bouton **Partager** → *Accès général* →
   « **Tous les utilisateurs disposant du lien** » → rôle **Lecteur**.
   (C'est ce qui permet au site de lire le tableur. Le contenu n'a rien de
   confidentiel : ce sont des phrases d'accueil.)

3. **Donner le droit de modifier aux bénévoles.** Toujours dans **Partager**,
   ajoutez leurs adresses e-mail en **Éditeur**. Eux seuls pourront écrire.

4. **Copier l'identifiant du tableur.** Dans l'adresse du Google Sheet :

   ```
   https://docs.google.com/spreadsheets/d/1a2B3c4D5e6F7g8H9i0J.../edit#gid=0
                                          └────── ceci ──────┘
   ```

5. **Le coller dans `index.html`.** Vers la ligne 190, au début des `RÉGLAGES` :

   ```js
   SHEET_ID: "1a2B3c4D5e6F7g8H9i0J...",
   ```

   Enregistrez, renvoyez `index.html` sur GitHub (dépôt → `index.html` → crayon
   *Edit* → coller → *Commit changes*). Une minute plus tard, c'est actif.

En bas du site s'affiche alors « Données : Google Sheets », avec un lien direct vers
le tableur — pratique pour les bénévoles.

### Ce qui se passe si Google est injoignable

Le site **repasse automatiquement sur `data.xlsx`** et affiche un bandeau orange
d'avertissement. Il ne tombe jamais en panne. Pensez donc, de temps en temps, à
retélécharger le Google Sheet (Fichier → Télécharger → *Microsoft Excel*) et à
remplacer `data.xlsx` dans le dépôt : ça garde la copie de secours à jour.

---

# 3. Modifier les textes ou ajouter une langue

Tout se fait dans le tableur (Google Sheets, ou `data.xlsx` si vous n'avez pas encore
fait l'étape 2). **Il n'y a pas de code à toucher.**

- Onglet **« Traductions »** : **une ligne par langue**. Remplissez les colonnes `p1`…`p6`
  avec les traductions. **Une cellule jaune = traduction encore à faire.**
- `code` : identifiant court et unique (`pt`, `ar-sd`…). Sert à nommer les fichiers
  audio — ne le changez plus une fois fixé.
- `langue_native` : le nom de la langue dans sa propre écriture (c'est ce que la personne
  reconnaît, à soigner).
- `drapeaux` : codes pays séparés par des virgules, ex. `pt,br,ao`
  (codes ISO à 2 lettres : `fr`, `gb`, `sd`…). Laissez vide si aucun ne convient.
  Un drapeau absent du dossier `flags/` est téléchargé automatiquement, rien à faire.
- `sens` : `ltr` par défaut, ou `rtl` pour les langues qui s'écrivent de droite à gauche
  (arabe, hébreu, farsi, dari, ourdou, pachto…).
- L'onglet **« Mode d'emploi »** rappelle tout ça dans le fichier lui-même.

Rechargez la page : c'est à jour. Les lignes `fr`, `en` et `es` sont déjà remplies en
entier et servent d'exemple.

# 4. Ajouter un enregistrement audio

1. Nommez le fichier ainsi : **`CODE_pN.mp3`** — ex. `es_p3.mp3` (phrase 3 en espagnol).
2. Déposez-le dans le dossier **`audio/`** du dépôt GitHub.
3. Dans le tableur, colonne `audio3` de la ligne `es`, écrivez : `audio/es_p3.mp3`.

Le bouton **Écouter** apparaît automatiquement. Tant qu'une case audio est vide,
aucun bouton ne s'affiche (mention « Audio à enregistrer »).

> **Et les liens Google Drive ?** Si un bénévole colle un lien de partage Drive dans une
> colonne `audioN`, le site essaie de le convertir en lecture directe. **Ça marche
> souvent, mais pas toujours** (Google intercale parfois une page d'avertissement).
> Pour les enregistrements définitifs, déposez les `.mp3` dans `audio/` : c'est plus
> rapide et toujours fiable.

# 5. Tester sur votre ordinateur (avant mise en ligne)

Ouvrir `index.html` par double-clic **ne marche pas** : le navigateur bloque la lecture
du fichier Excel en local (`file://`). Lancez un mini-serveur depuis le dossier :

```bash
python -m http.server 8000
```

puis ouvrez `http://localhost:8000`. (Avec VS Code, l'extension « Live Server » fait pareil.)

# 6. Faire évoluer l'apparence avec Claude Code

Clonez le dépôt en local, ouvrez le dossier dans Claude Code, décrivez la modification
(`index.html` est le seul fichier « code »), testez avec `python -m http.server`, puis
`git push` — GitHub Pages publie la nouvelle version automatiquement.

Les **traductions**, elles, se gèrent dans le tableur (par vos bénévoles/traducteurs),
indépendamment du code.

---

**À vérifier :** faites toujours relire chaque traduction par une personne qui parle la
langue avant de la diffuser — surtout pour les langues à écriture complexe.
