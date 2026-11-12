# OASIS — Communiquer dans votre langue

### 👉 Le site : **https://oasis-crf.github.io**

Application d'une page pour l'accueil OASIS de la Croix-Rouge : on choisit la langue de
la personne en face en cliquant sur un drapeau, puis on lui montre les phrases traduites
et on peut écouter l'enregistrement audio de chacune.

**Les trois choses à savoir :**

- Les traductions se modifient dans un **tableur**, pas dans le code.
- Une modification du tableur est visible sur le site **au rechargement de la page**.
- Le site se republie tout seul à chaque envoi de fichier sur GitHub (branche `main`).

## Contenu du dossier

```
index.html            ← l'application (ne demande aucune installation)
logo.png              ← le logo OASIS, dans l'en-tête du site
logo-crf.png          ← le logo Croix-Rouge, à côté
data_traduction.xlsx  ← la base de données : langues, drapeaux, traductions, audio
flags/                ← les drapeaux (48), stockés en local pour la rapidité
audio/                ← les fichiers son (.mp3), à déposer ici
README.md             ← ce guide
```

## Utilisation au quotidien

Ouvrez le site, tapez le début du nom de la langue ou faites défiler, cliquez sur la
vignette. Vous voyez les 6 phrases : le texte français (pour vous repérer), la
traduction en grand (pour la personne), et un bouton **Écouter** quand l'audio existe.

---

# 1. Modifier les textes ou ajouter une langue

Tout se fait dans le tableur. **Il n'y a pas de code à toucher.**

- Onglet **« Traductions »** : **une ligne par langue**. Remplissez les colonnes `P1`…`P6`
  avec les traductions.
- `Code` : identifiant court et unique (`pt`, `ar-sd`…). Sert à nommer les fichiers
  audio — ne le changez plus une fois fixé.
- `Langue_native` : le nom de la langue dans sa propre écriture (c'est ce que la personne
  reconnaît, à soigner).
- `Pays` : les pays concernés en toutes lettres. Sert à s'y retrouver dans le tableur ;
  le site, lui, n'affiche que les drapeaux.
- `Drapeaux` : codes pays séparés par des virgules, ex. `pt,br,ao`
  (codes ISO à 2 lettres : `fr`, `gb`, `sd`…). Laissez vide si aucun ne convient.
  Un drapeau absent du dossier `flags/` est téléchargé automatiquement, rien à faire.
  Deux exceptions, pour des drapeaux sans pays : `kurd` (Kurdistan) et `tibet`.
  Ceux-là n'existent sur aucun serveur extérieur, ils sont dans `flags/`.
- `Sens` : `ltr` par défaut, ou `rtl` pour les langues qui s'écrivent de droite à gauche
  (arabe, hébreu, farsi, dari, ourdou, pachto…).
- `StatutGeneral` : calculée toute seule, **ne rien y écrire**. C'est le compte des
  12 cases d'une langue — 6 traductions, 6 enregistrements — affiché `9/12`. Une
  « Ancienne version » vaut une demie, d'où les `4,5/12`. La cellule se teinte de
  l'orange au vert selon l'avancement : la colonne se lit sans lire les chiffres.
- `StatutTrad1`…`StatutTrad6` : où en est la **traduction écrite** de chaque phrase,
  à choisir dans la liste déroulante.
  **« Ancienne version »** = la traduction existe mais suit l'ancien texte français
  de la phrase 5 → elle est à refaire (voir l'onglet *Mode d'emploi*).
- `StatutAudio1`…`StatutAudio6` : où en est l'**enregistrement audio** de chaque phrase
  (`Non enregistré` / `Ancienne version` / `Enregistré`). Les deux avancements se suivent
  séparément : une langue peut avoir l'audio sans le texte, et l'inverse.
- `Contact` : la personne à qui demander ou redemander la traduction.

- L'onglet **« Mode d'emploi »** rappelle tout ça dans le fichier lui-même.

> La casse des en-têtes n'a pas d'importance pour le site (`Code` ou `code`), mais
> l'orthographe, si : ne renommez pas les colonnes.

Rechargez la page : c'est à jour.

**Où modifier :** le site lit `data_traduction.xlsx`, qui est *dans* le dépôt. Les bénévoles
corrigent dans un Google Sheet, mais le site ne le lit pas (voir §3) : il faut renvoyer le
fichier sur GitHub pour que la correction apparaisse.

# 2. Ajouter un enregistrement audio

1. Nommez le fichier ainsi : **`CODE_pN.mp3`** — ex. `es_p3.mp3` (phrase 3 en espagnol).
2. Déposez-le dans le dossier **`audio/`** du dépôt GitHub.
3. Dans le tableur, colonne `Audio3` de la ligne `es`, écrivez : `audio/es_p3.mp3`,
   et passez `StatutAudio3` à **« Enregistré »**.

Le bouton **Écouter** apparaît automatiquement. Tant qu'une case audio est vide,
aucun bouton ne s'affiche (mention « Audio à enregistrer »).

> **Pas de lien Google Drive dans les colonnes `AudioN`.** Le bouton « Écouter » resterait
> muet : le domaine Croix-Rouge n'autorise pas le partage public des fichiers Drive, donc
> le site ne peut pas les lire. Les enregistrements arrivent en général dans un `.zip` du
> Drive : il faut les convertir en `.mp3` et les déposer dans `audio/`.

# 3. Le Google Sheet des bénévoles

Les bénévoles corrigent les traductions dans un **Google Sheet**. Mais le domaine
Croix-Rouge interdit de le partager publiquement (« Tous les utilisateurs disposant du
lien ») : **le site ne peut donc pas le lire**, et `SHEET_ID` reste vide dans `index.html`.
Le site ne sert que `data_traduction.xlsx`, et rien ne synchronise les deux : c'est une
opération manuelle.

**Du Sheet vers le site** (récupérer leurs corrections) :

1. Dans le Sheet : **Fichier → Télécharger → Microsoft Excel**.
2. Remplacer `data_traduction.xlsx` dans le dépôt par ce fichier, puis l'envoyer sur GitHub.

**Du dépôt vers le Sheet** (après avoir ajouté une langue ou un audio dans le fichier) :
Fichier → **Importer** → *Importer* → « **Remplacer la feuille de calcul** ». Attention, ça
écrase ce qui a été tapé en ligne depuis le dernier téléchargement : partez donc toujours
d'un export récent, jamais d'une vieille copie du dépôt.

À savoir :

- **Ne renommez pas les onglets** (`Phrases`, `Traductions`) : le site les cherche par leur nom.
  Après un passage par Google, « Mode d'emploi » devient « Mode demploi » : sans effet.
- Toute mise en forme conditionnelle doit rester en **dégradé de couleurs** : Google
  Sheets perd les barres de données.

# 4. Tester sur votre ordinateur

Ouvrir `index.html` par double-clic **ne marche pas** : le navigateur bloque la lecture
du fichier Excel en local (`file://`). Lancez un mini-serveur depuis le dossier :

```bash
python -m http.server 8000
```

puis ouvrez `http://localhost:8000`.

# 5. Faire évoluer l'apparence

Ouvrez le dossier dans Claude Code et décrivez la modification — `index.html` est le seul
fichier « code ». Testez en local, puis :

```bash
git push
```

GitHub Pages republie la nouvelle version automatiquement, en une minute environ.

---

**À vérifier :** faites toujours relire chaque traduction par une personne qui parle la
langue avant de la diffuser — surtout pour les langues à écriture complexe.
