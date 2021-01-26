CoughCNN QC DATASETS

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


All the data of the project can be found at : https://drive.google.com/drive/folders/1deqYCDye5l95RGJCeKXlcqH9Ras7lRQr?usp=sharing

Right now we have normalized the data and we use a new model structure using GlobalAveragePooling2D which gave a huge performance
boost to the previous version. We went from 68-75% to 80-84% accuracy on our test set created from the webapp.
Right now we are running hyperparameters optimization and you can check out everything at:
https://app.wandb.ai/mastersplinter/CoughDetect/sweeps/phdtst8z

### Next

- Data processing hyperparameters
- Testing different sample length and optimize them

### Resources that inspired this:

- https://arxiv.org/pdf/2004.01275.pdf (for initial model architecture)
- https://www.mi.t.u-tokyo.ac.jp/assets/publication/LEARNING_ENVIRONMENTAL_SOUNDS_WITH_END-TO-END_CONVOLUTIONAL_NEURAL_NETWORK.pdf
- https://www.cs.tut.fi/~tuomasv/papers/ijcnn_paper_valenti_extended.pdf
- https://adventuresinmachinelearning.com/global-average-pooling-convolutional-neural-networks/
- https://arxiv.org/pdf/1809.04437.pdf
- https://arxiv.org/pdf/1711.10282.pdf
