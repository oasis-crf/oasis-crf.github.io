# OASIS — consignes pour Claude

Site statique (`index.html`) de phrases traduites et écoutables, pour l'accueil
de la Croix-Rouge. Il lit `data_traduction.xlsx` (onglets `Phrases` et
`Traductions`) ; `SHEET_ID` reste vide car le domaine CRF interdit le partage
public d'un Google Sheet. Répondre à Eulalie en français.

## Ajouter ou mettre à jour une langue

La ligne de la langue existe presque toujours déjà dans `Traductions` (code,
nom, drapeaux). On remplit, on ne crée pas, sauf langue vraiment nouvelle.

1. **Textes** : Eulalie colle les phrases P1 à P6. Les écrire dans les colonnes
   `P1`…`P6`, passer `StatutTrad1..6` à « Nouvelle version ». Si le texte
   collé diffère de l'existant (ex. portugais brésilien → européen), il le
   remplace : c'est elle qui fait foi.
2. **Audio** : les enregistrements arrivent dans un `.zip` du Drive déposé dans
   `Imports enregistrements/` (exclu du dépôt). Le prendre le plus récent,
   extraire **dans un dossier neuf du scratchpad**, jamais dans le projet.
   Les dossiers portent le nom de la langue en français et en majuscules
   (`TAMIL`, `PORTUGAIS`, `PULLAR` = peul `ff`). Le numéro de phrase est dans le
   nom du fichier ; certains fichiers n'ont pas d'extension (lire l'en-tête,
   `ftyp` = m4a). Si une langue a plus ou moins de six fichiers, ou une seule
   longue piste (swahili), découper et le dire.
3. **Conversion** vers `audio/CODE_pN.mp3` : mono, 48 kHz, 64 kb/s. ffmpeg n'est
   pas installé : `python -c "import imageio_ffmpeg; print(imageio_ffmpeg.get_ffmpeg_exe())"`.
4. **Annonces** : les bénévoles disent parfois « phrase N » en français en tête
   de piste (repérées au silence ≥ 0,85 s qui suit). À couper.
5. **Tableur** : remplir `AudioN` = `audio/CODE_pN.mp3` et `StatutAudioN` =
   « Enregistré ». Jamais de lien Drive (le bouton « Écouter » resterait muet).
6. **Vérifier** : chaque chemin `audio/` existe, aucun code de langue perdu,
   mise en forme conditionnelle et listes déroulantes conservées (openpyxl les
   garde), le fichier s'ouvre encore (Collabora : `C:\Program Files\Collabora
   Office\program\soffice.exe`).
7. **Commit** en français, un message par étape ou par langue
   (« Russe : … »). Ne commiter que si Eulalie le demande.

## Débruiter (sur demande, ou si Eulalie le signale)

`afftdn` ne sert à rien. Utiliser une soustraction spectrale : STFT 1024/256,
profil de bruit = moyenne des 15 % de trames les plus faibles du fichier,
gain `max(1 - 2.5*N/|X|, -22 dB)` lissé sur 3 bandes puis 3 trames, puis
renormaliser au pic d'origine. Gain attendu : 8 à 20 dB sur le plancher.
Vérifier au spectre, pas à la corrélation de forme d'onde. N'appliquer qu'aux
langues demandées. Whisper ne transcrit pas tout (pas le kurmandji) : en cas
de doute sur ce qui est dit, demander à Eulalie.

## Synchronisation avec le Sheet des bénévoles

Le Sheet en ligne est la surface d'édition ; le site sert `data_traduction.xlsx`.
Rien ne les synchronise. **Partir de l'export récent des bénévoles** (Fichier →
Télécharger → Excel) si elle en dépose un, jamais de la copie du dépôt sans
lui demander : sinon on écrase leurs corrections. À la fin, elle remonte le
fichier par Fichier → Importer → « Remplacer la feuille de calcul » (écrase ce
qui a été tapé en ligne depuis le téléchargement). Toute mise en forme
conditionnelle doit rester en dégradé de couleurs (les barres de données sont
perdues par Sheets).

## Particularités à ne pas « corriger »

- Une ligne **sans traduction écrite** mais avec audio est légitime : le site
  affiche « Traduction à ajouter » et « Écouter » (farsi, hébreu, hindi, roumain).
- L'onglet « Mode d'emploi » devient « Mode demploi » après un passage par
  Google : sans effet.
- Les dossiers `_ecoute/` et `Imports enregistrements/` sont de travail, hors dépôt.

## Poste Windows

Shell Git Bash/PowerShell. Python 3.12 : lancer avec `-I` et
`PYTHONIOENCODING=utf-8` (la console cp1252 plante sur le tamoul et le
tibétain). Fichiers temporaires dans le scratchpad, pas dans le projet.
