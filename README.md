# hand-geste-recognition-using-mediapipe
Estimez la pose de la main à l'aide de MediaPipe (version Python).<br> Ceci est un exemple
programme qui reconnaît les signes de la main et les gestes des doigts avec un simple MLP en utilisant les points clés détectés.

![mqlrf-s6x16](https://user-images.githubusercontent.com/37477845/102222442-c452cd00-3f26-11eb-93ec-c387c98231be.gif)

Ce référentiel contient le contenu suivant.
* Exemple de programme
* Modèle de reconnaissance des signes de la main (TFLite)
* Modèle de reconnaissance des gestes du doigt (TFLite)
* Données d'apprentissage pour la reconnaissance des signes de la main et cahier pour l'apprentissage
* Données d'apprentissage pour la reconnaissance des gestes du doigt et ordinateur portable pour l'apprentissage

# Conditions
* mediapipe 0.8.1
* OpenCV 3.4.2 ou version ultérieure
* Tensorflow 2.3.0 ou version ultérieure<br>tf-nightly 2.5.0.dev ou version ultérieure (uniquement lors de la création d'un TFLite pour un modèle LSTM)
* scikit-learn 0.23.2 ou version ultérieure (uniquement si vous souhaitez afficher la matrice de confusion)
* matplotlib 3.3.2 ou version ultérieure (uniquement si vous souhaitez afficher la matrice de confusion)

# Démo
Voici comment exécuter la démo à l'aide de votre webcam.
```bash
python app.py
```

Les options suivantes peuvent être spécifiées lors de l'exécution de la démo.
* --device<br>Spécifier le numéro de l'appareil photo (par défaut：0)
* --width<br>Largeur au moment de la capture de la caméra (Par défaut：960)
* --height<br>Hauteur au moment de la capture de la caméra (par défaut：540)
* --use_static_image_mode<br>S'il faut utiliser l'option static_image_mode pour l'inférence MediaPipe (Par défaut：Non spécifié)
* --min_detection_confidence<br>
Seuil de confiance de détection (Par défaut：0,5)
* --min_tracking_confidence<br>
Seuil de confiance de suivi (par défaut：0,5)

# Répertoire
<pré>
app.py
│ classification_point_clé.ipynb
│ point_history_classification.ipynb
??
modèle
├─keypoint_classifier
│ │ keypoint.csv
│ │ keypoint_classifier.hdf5
│ │ keypoint_classifier.py
│ │ keypoint_classifier.tflite
│ └─ keypoint_classifier_label.csv
│
└─point_history_classifier
│ point_history.csv
│ │ point_history_classifier.hdf5
│ point_history_classifier.py
│ point_history_classifier.tflite
└─ point_history_classifier_label.csv
??
utils
    cvfpscalc.py
</pre>
### app.py
Ceci est un exemple de programme pour l'inférence.<br>
De plus, des données d'apprentissage (points clés) pour la reconnaissance des signes de la main,<br>
Vous pouvez également collecter des données d'entraînement (historique des coordonnées de l'index) pour la reconnaissance des gestes du doigt.

### keypoint_classification.ipynb
Il s'agit d'un modèle de script d'entraînement pour la reconnaissance des signes de la main.

### point_history_classification.ipynb
Il s'agit d'un modèle de script d'entraînement pour la reconnaissance des gestes du doigt.

### model/keypoint_classifier
Ce répertoire stocke les fichiers liés à la reconnaissance des signes de la main.<br>
Les fichiers suivants sont stockés.
* Données d'entraînement (keypoint.csv)
* Modèle formé (keypoint_classifier.tflite)
* Données d'étiquette (keypoint_classifier_label.csv)
* Module d'inférence (keypoint_classifier.py)

### modèle/point_history_classifier
Ce répertoire stocke les fichiers liés à la reconnaissance des gestes du doigt.<br>
Les fichiers suivants sont stockés.
* Données d'entraînement (point_history.csv)
* Modèle formé (point_history_classifier.tflite)
* Données d'étiquette (point_history_classifier_label.csv)
* Module d'inférence(point_history_classifier.py)

### utils/cvfpscalc.py
Il s'agit d'un module de mesure FPS.

# Entraînement
La reconnaissance des signes de la main et la reconnaissance des gestes du doigt peuvent ajouter et modifier des données d'entraînement et recycler le modèle.

### Formation à la reconnaissance des signes de la main
#### 1.Collecte de données d'apprentissage
Appuyez sur « k » pour entrer dans le mode d'enregistrement des points clés (affichés sous la forme 「MODE : Logging Key Point」）)<br>
<img src="https://user-images.githubusercontent.com/37477845/102235423-aa6cb680-3f35-11eb-8ebd-5d823e211447.jpg" width="60%"><br><br>
Si vous appuyez sur "0" à "9", les points clés seront ajoutés à "model/keypoint_classifier/keypoint.csv" comme indiqué ci-dessous.<br>
1ère colonne : numéro enfoncé (utilisé comme ID de classe), 2e colonne et suivantes : coordonnées du point clé<br>
<img src="https://user-images.githubusercontent.com/37477845/102345725-28d26280-3fe1-11eb-9eeb-8c938e3f625b.png" width="80%"><br><br>
Les coordonnées des points clés sont celles qui ont subi le prétraitement suivant jusqu'à ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%"><br><br>
Dans l'état initial, trois types de données d'apprentissage sont inclus : la main ouverte (ID de classe : 0), la main fermée (ID de classe : 1) et le pointage (ID de classe : 2).<br>
Si nécessaire, ajoutez 3 ou plus tard, ou supprimez les données existantes de csv pour préparer les données d'entraînement.<br>
<img src="https://user-images.githubusercontent.com/37477845/102348846-d0519400-3fe5-11eb-8789-2e7daec65751.jpg" width="25%">　<img src="https://user -images.github