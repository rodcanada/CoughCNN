Cough CNN 

AVEC QC DATASET

Modèle de détection des anomalies covid
En raison de l’absence de suffisamment d’échantillons de toux covid, et des caractéristiques précises dans ces toux qui le classent comme covid; nous ne pouvons pas utiliser une approche simple d’apprentissage supervisé. Au lieu de cela, l’approche adoptée actuellement est un non supervisé (qui se tournera vers semi-supervisé plus tard en utilisant les données avaiables pour affiner le modèle) à l’aide d’un autoencoder. L’autoencoder prend tous les échantillons de toux non covid qui ont été détectés par le modèle en spectro et les utilise comme un ensemble de données de formation, l’autoencoder apprend la représentation d’une toux « normale » et comment le recréer (résume l’échantillon en utilisant des convolutions, puis reconstruit l’échantillon en utilisant une transposition de la convolution). Lorsqu’une toux qui n’était pas semblable à celles qui se trouvaient dans l’ensemble de données (qui, avec suffisamment de données, devrait être pointée du côté de la toux covid), l’erreur que le modèle fera lors de la recréation de l’échantillon fourni (calculer à l’aide du MSE de l’image originale avec celle créée par le modèle) sera élevée et l’échantillon sera étiqueté comme une anomalie.

Actuelle
Sous le dossier covid il ya deux fichiers: model.py, contient le code pour l’autoencoder et le MSE appliqué au melspectrogram, cela ne fonctionne toujours pas malheureusement, je suis peaufiner la forme d’entrée des échantillons en augmentant la longueur de l’échantillon et le nombre de mels par échantillon, mais je reçois toujours des problèmes. processing.ipynb contient le traitement des données pour créer l’ensemble de données utilisé

short-spectro est un dossier clone de spectro, mais contient les scripts pour rendre les données appropriées pour le modèle de détection des anomalies, une fois qu’une solution stable pour la détection des anomalies sera terminée tous les échantillons seront dans le même format et utilisable dans n’importe quel modèle. A partir de maintenant, je vais garder les deux séparés.

Voici toutes les données traitées par le spectro court: https://drive.google.com/file/d/1aAMGHTFJjiv7K6DrN_1DptcCeA3Efmbk/view?usp=sharing

Log Melspectrogram
Sous le spectro dossier, vous pouvez trouver toute l’approche en utilisant les melspectrograms pour former un réseau neuronal convolutionnel

Actuelle
À l’heure actuelle, les données qui ont été utilisées proviennent d’un ensemble de données sur la toux de : https://www.karger.com/Article/FullText/504666 et l’utilisation de données étiquetées manuellement provenant de notre webapp. L’ensemble de données de test est également de nombreuses données étiquetées provenant de notre webapp, mais que obviosly le CNN n’a pas vu avant.

Ceci a été réalisé augmentant le jeu de données en mélangeant l’échantillon de toux avec un certain bruit de fond pour les rendre plus réels. Il a été utilisé une ration de mélange de 0,25 (0,25 du signal sonore ajouté) à l’aide de l’ensemble de données musan https://www.openslr.org/17/

Les échantillons sont maintenant un mel spectrogram à l’échelle grise de 0,5 s.

Toutes les données du projet se trouvent à : https://drive.google.com/drive/folders/1deqYCDye5l95RGJCeKXlcqH9Ras7lRQr?usp=sharing

En ce moment, nous avons normalisé les données et nous utilisons une nouvelle structure de modèle en utilisant GlobalAveragePooling2D qui a donné un énorme coup de pouce de performance à la version précédente. Nous sommes passés de 68-75% à 80-84% de précision sur notre ensemble de test créé à partir du webapp. En ce moment, nous sommes en cours d’optimisation hyperparameters et vous pouvez tout vérifier à: https://app.wandb.ai/mastersplinter/CoughDetect/sweeps/phdtst8z

prochain
Hyperparamètres de traitement de données
Tester différentes longueurs d’échantillon et les optimiser
Ressources qui ont inspiré ceci :
