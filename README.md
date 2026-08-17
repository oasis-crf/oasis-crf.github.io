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
data_traduction.xlsx  ← la base de données : langues, drapeaux, traductions, audio
flags/                ← les drapeaux (46 pays), stockés en local pour la rapidité
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
- `Sens` : `ltr` par défaut, ou `rtl` pour les langues qui s'écrivent de droite à gauche
  (arabe, hébreu, farsi, dari, ourdou, pachto…).
- `Statut1`…`Statut6` : où en est chaque phrase, à choisir dans la liste déroulante.
  **« Ancienne version »** = la traduction existe mais suit l'ancien texte français
  de la phrase 5 → elle est à refaire (voir l'onglet *Mode d'emploi*).
- `Contact` : la personne à qui demander ou redemander la traduction.

- L'onglet **« Mode d'emploi »** rappelle tout ça dans le fichier lui-même.

> La casse des en-têtes n'a pas d'importance pour le site (`Code` ou `code`), mais
> l'orthographe, si : ne renommez pas les colonnes.

Rechargez la page : c'est à jour.

**Où modifier, selon la configuration :**

- *Tableur sur Google Sheets* (voir §3) → modifiez en ligne, c'est immédiat.
- *Pas encore configuré* → modifiez `data_traduction.xlsx` et renvoyez-le sur GitHub
  (dépôt → `data_traduction.xlsx` → *Add file* → *Upload files* → glisser la nouvelle version).

# 2. Ajouter un enregistrement audio

1. Nommez le fichier ainsi : **`CODE_pN.mp3`** — ex. `es_p3.mp3` (phrase 3 en espagnol).
2. Déposez-le dans le dossier **`audio/`** du dépôt GitHub.
3. Dans le tableur, colonne `Audio3` de la ligne `es`, écrivez : `audio/es_p3.mp3`.

Le bouton **Écouter** apparaît automatiquement. Tant qu'une case audio est vide,
aucun bouton ne s'affiche (mention « Audio à enregistrer »).

> **Et les liens Google Drive ?** Si un bénévole colle un lien de partage Drive dans une
> colonne `AudioN`, le site le convertit tout seul en lecture directe. **Ça marche
> souvent, mais pas toujours** : Google intercale parfois une page d'avertissement, et
> limite les téléchargements d'un fichier très consulté. Pour les enregistrements
> définitifs, déposez les `.mp3` dans `audio/` : c'est plus rapide et toujours fiable.

# 3. Mettre les traductions sur Google Drive *(optionnel, non configuré)*

Aujourd'hui le site lit `data_traduction.xlsx`, qui est *dans* le site : le modifier demande de
repasser par GitHub. Pour que **plusieurs bénévoles corrigent les traductions
directement**, on met la base sur Google Sheets.

1. **Importer le tableur.** [drive.google.com](https://drive.google.com) → **Nouveau** →
   *Importer un fichier* → `data_traduction.xlsx`. Puis clic droit → *Ouvrir avec* →
   **Google Sheets** → Fichier → **Enregistrer au format Google Sheets**.
   **Ne renommez pas les onglets** (`Phrases`, `Traductions`) : le site les cherche par
   leur nom.

2. **Partager en lecture.** Bouton **Partager** → *Accès général* →
   « **Tous les utilisateurs disposant du lien** » → rôle **Lecteur**.
   C'est ce qui permet au site de lire le tableur.

3. **Donner le droit de modifier aux bénévoles.** Toujours dans **Partager**, ajoutez
   leurs adresses e-mail en **Éditeur**. Eux seuls pourront écrire.

4. **Copier l'identifiant** dans l'adresse du tableur :

   ```
   https://docs.google.com/spreadsheets/d/1a2B3c4D5e6F7g8H9i0J.../edit#gid=0
                                          └────── ceci ──────┘
   ```

5. **Le coller dans `index.html`**, vers la ligne 187 :

   ```js
   SHEET_ID: "1a2B3c4D5e6F7g8H9i0J...",
   ```

   Enregistrez et renvoyez le fichier sur GitHub. Une minute plus tard, c'est actif :
   en bas du site s'affiche « Données : Google Sheets » avec un lien vers le tableur.

**Si Google est injoignable**, le site repasse automatiquement sur `data_traduction.xlsx` et affiche
un bandeau orange. Il ne tombe jamais en panne. Pensez donc de temps en temps à
retélécharger le Google Sheet (Fichier → Télécharger → *Microsoft Excel*) et à remplacer
`data_traduction.xlsx` dans le dépôt, pour garder la copie de secours à jour.

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
