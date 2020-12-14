# cv-fitness



**Contents:**
- [1. Algorithm](#1-algorithm)
- [2. Install Dependency (OpenPose)](#2-install-dependency--openpose-)
- [3. Program structure](#3-program-structure)
  * [Diagram](#diagram)
  * [Main scripts](#main-scripts)
- [4. How to run: Inference](#4-how-to-run--inference)
  * [Introduction](#introduction)
  * [Test on video file](#test-on-video-file)
  * [Test on a folder of images](#test-on-a-folder-of-images)
  * [Test on web camera](#test-on-web-camera)
- [5. Training data](#5-training-data)
  * [Download my data](#download-my-data)
  * [Data format](#data-format)
  * [Classes](#classes)
- [6. How to run: Training](#6-how-to-run--training)




# 1. Algorithm


Otto tipi di azioni: `['stand', 'jump', 'run', 'jumpingJack', 'lungesBack', 'lungesFront', 'squat', 'plank', 'wave']`. 



# 2. Install Dependency (OpenPose)

First, Python >= 3.6.

Ho usato OpenPose da qui: [tf-pose-estimation](https://github.com/ildoonet/tf-pose-estimation). Prima di scaricarlo:

```
export MyRoot=$PWD
cd src/githubs  
git clone https://github.com/ildoonet/tf-pose-estimation  
```

Segui il tutorial [here](https://github.com/ildoonet/tf-pose-estimation#install-1) per scaricare il modello cmu
```
$ cd tf-pose-estimation/models/graph/cmu  
$ bash download.sh  
```

Installa le dipendenze
```
conda create -n tf tensorflow-gpu
conda activate tf

cd $MyRoot
pip install -r requirements.txt
pip install jupyter tqdm
pip install tensorflow-gpu==1.13.1
sudo apt install swig
pip install "git+https://github.com/philferriere/cocoapi.git#egg=pycocotools&subdirectory=PythonAPI"

cd $MyRoot/src/githubs/tf-pose-estimation/tf_pose/pafprocess
swig -python -c++ pafprocess.i && python3 setup.py build_ext --inplace
```


## Scripts
I 5 script principali sono sotto src, sono denominati in base all'ordine di esecuzione:
```
src/s1_get_skeletons_from_training_imgs.py    
src/s2_put_skeleton_txts_to_a_single_txt.py
src/s3_preprocess_features.py
src/s4_train.py 
src/s5_test.py
```


# 4. Come eseguire

## Introduzione
Lo script src / s5_test.py serve per il riconoscimento delle azioni in tempo reale.


## Prova su file video
``` bash
python src/s5_test.py \
    --model_path model/trained_classifier.pickle \
    --data_type video \
    --data_path data_test/exercise.avi \
    --output_folder output
```

## Prova su una cartella di immagini
``` bash
python src/s5_test.py \
    --model_path model/trained_classifier.pickle \
    --data_type folder \
    --data_path data_test/apple/ \
    --output_folder output
```

## Prova sulla webcam
``` bash
python src/s5_test.py \
    --model_path model/trained_classifier.pickle \
    --data_type webcam \
    --data_path 0 \
    --output_folder output
```

# 5. Dati di training

link:https://drive.google.com/drive/folders/1H_QRDvY6zxpMtQVsOSVuRtWILG_N2Wdd?usp=sharing


# 6. How to run: Training

* Infine, esegui i seguenti script uno per uno:
    ``` bash
    python src/s1_get_skeletons_from_training_imgs.py
    python src/s2_put_skeleton_txts_to_a_single_txt.py 
    python src/s3_preprocess_features.py
    python src/s4_train.py 
    ```

Al termine dell'addestramento, e' possibile eseguire lo script src/s5_test.py


