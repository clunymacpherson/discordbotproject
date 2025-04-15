```plaintext
 ('-. .-.                                 _   .-')                _  .-')     _ (`-.  ('-. .-. 
( OO )  /                                ( '.( OO )_             ( \( -O )   ( (OO  )( OO )  / 
,--. ,--..-'),-----. ,--.     .-'),-----. ,--.   ,--.).-'),-----. ,------.  _.`     \,--. ,--. 
|  | |  ( OO'  .-.  '|  |.-')( OO'  .-.  '|   `.'   |( OO'  .-.  '|   /`. '(__...--''|  | |  | 
|   .|  /   |  | |  ||  | OO )   |  | |  ||         |/   |  | |  ||  /  | | |  /  | ||   .|  | 
|       \_) |  |\|  ||  |`-' \_) |  |\|  ||  |'.'|  |\_) |  |\|  ||  |_.' | |  |_.' ||       | 
|  .-.  | \ |  | |  (|  '---.' \ |  | |  ||  |   |  |  \ |  | |  ||  .  '.' |  .___.'|  .-.  | 
|  | |  |  `'  '-'  '|      |   `'  '-'  '|  |   |  |   `'  '-'  '|  |\  \  |  |     |  | |  | 
`--' `--'    `-----' `------'     `-----' `--'   `--'     `-----' `--' '--' `--'     `--' `--' 
```

# Holomorph: Template-Based Data Extractor

Holomorph est une pipeline complète qui permet d'extraire les données d'un document en utilisant une approche basée sur l'exploitation de modèles (templates). Elle permet à l'utilisteur de définir des régions d'intérêt sur un template et d'extraire et lire les données présentes dans ces zones avec une grande précision.

## Features

- **Mask Designer** (`mask_designer.py`):
  - Création et édition de masques sur les templates.
  - Visualisation, modification des masques, modification des régions.
  - Visualisation du masque appliqué sur une image.

- **Image Processing Pipeline**:
  1. **Alignment and Cropping**:
     - Crop sur le contour extérieur de l'image pour un alignement facilité.
     - Aligne l'image d'entrée avec le template en utilisant AKAZE et BFMatcher.
     - Extraction des images sur les régions définies
  2. **Text Recognition**:
     - Lecture des régions avec post-processing
     - Plus de **93.45% d'accuracy** and **97.84% Normalized Edit Distance (mesure de similarité)**.

- **Label Editor**:
  - Ouvre une fenêtre permettant de vérifier la précision de la lecture par le modèle, vérifier les zones, le correct alignement.
  - Survoler chacune des zones de texte des régions pour les visualiser plus facilement.
  - Modification en cas d'erreur de lecture ou de cohérence.
  - Affiche sur le côté la confiance et prévision du modèle (capable d'estimer si c'est une zone vide, une fausse prédiction, une date erronnée, ...)

## Workflow

1. **Design a Mask**:
   - Utiliser `mask_designer.py` pour créer un masque pour le document modèle.
   - Enregistrer le masque.

2. **Run the Pipeline**:
   - Lancer `run.py` pour traiter la/les images:
     - Appelle `masquage_zones.py` pour aligner l'image et extraire les données en fonction du masque.
     - Appelle `read.py` pour reconnaître le texte présent dans les zones.

3. **Edit Labels**:
   - Vérifier et modifier le texte reconnu par le modèle à l'aide de la fenêtre d'édition.

## Requirements

- Python 3.x
- Required libraries: see `requirements.txt`

## Usage

1. **Create a Mask**:
```
   python mask_designer.py
   python run.py
```

2. **Train your Model**:
```
   cd tools
   python create_lmdb_dataset.py
   cd ..
   python train.py
```

2. **Train from Predictions (active learning)**:

After running run.py and having a substantially large database of verified predictions in all_predictions.json, you can start training your model on model-labeled and human-verified data instead of handwriting the labels. This allows for better accuracy, better fit of the predictions (less general, more specific to your case) 
```
   python create_lmdb_dataset.py
   python train.py
```
