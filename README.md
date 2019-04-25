## 1- Install Anaconda

## 2- Kumpulkan gambar sebagai sumber dataset
Kumpulkan gambar yang akan digunakan sebagai sumber untuk membuat dataset yang diinginkan.

## 3- Ubah ukuran gambar menjadi lebih kecil
Gunakan resizer.py untuk mengubah ukuran gambar menjadi lebih kecil.

## 4- Buat label pada gambar
Download labelImg pada https://github.com/tzutalin/labelImg, kemudian unzip labelImg-master
Buka cmd dan menuju direktori labelImg.

```bash
conda install pyqt=5 
pyrcc5 -o resources.py resources.qrc
python labelImg.py
```

## 5- Pisahkan gambar setelah diberikan label
80 % pada folder train dan 20% pada folder test

## 6- Buat environment variable pada Anaconda

```bash
conda create -n tensorflow pip python=3.5 
activate tensorflow 
```
NB : tensorflow saat ini masih mendukung python versi 3.5 atau 3.6.

## 7- Install Tensorflow-GPU
```bash
pip install --ignore-installed --upgrade tensorflow-gpu
```

## 8- Install *library* atau *package* yang dibutuhkan

```bash
(tensorflow) C:\> conda install -c anaconda protobuf 
(tensorflow) C:\> pip install pillow 
(tensorflow) C:\> pip install lxml 
(tensorflow) C:\> pip install Cython 
(tensorflow) C:\> pip install jupyter 
(tensorflow) C:\> pip install matplotlib 
(tensorflow) C:\> pip install pandas 
(tensorflow) C:\> pip install opencv-python 
```

## 9- Download Tensorflow *Repository*
https://github.com/tensorflow/models.git kemudian download *repository* ini https://github.com/miqdadmq/Pelatihan-Dataset-Faster-RCNN

## 10- Download Model untuk dataset yang diinginkan
Karena yang saya gunakan adalah faster rcnn maka saya download faster_rcnn_inception_v2_coco pada :
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md
Kemudian buat folder `images`, `training`, dan `inference_graph` pada `C:\tensorflow\models\research\object_detection`
kemudian pindahkan folder `train` dan `test` pada folder `images`

## 11- Konfigurasi PYTHONPATH pada *environment variable*

```
set PYTHONPATH=C:\tensorflow\models;C:\tensorflow\models\research;C:\tensorflow\models\research\slim
echo %PYTHONPATH%
set PATH=%PATH%;PYTHONPATH
echo %PATH%
```
NB : *Step* ini harus dilakukan setiap kali memulai ulang Anaconda

## 12- *Compile Protobuf*
*Library protobuf* harus di-*compile*, karena digunakan untuk konfigurasi model dan *training* parameter dataset
pada Anconda masuk ke direktori `C:\tensorflow\models\research`, kemudian `copy-paste` berikut ini :

```
protoc --python_out=. .\object_detection\protos\anchor_generator.proto .\object_detection\protos\argmax_matcher.proto .\object_detection\protos\bipartite_matcher.proto .\object_detection\protos\box_coder.proto .\object_detection\protos\box_predictor.proto .\object_detection\protos\eval.proto .\object_detection\protos\faster_rcnn.proto .\object_detection\protos\faster_rcnn_box_coder.proto .\object_detection\protos\grid_anchor_generator.proto .\object_detection\protos\hyperparams.proto .\object_detection\protos\image_resizer.proto .\object_detection\protos\input_reader.proto .\object_detection\protos\losses.proto .\object_detection\protos\matcher.proto .\object_detection\protos\mean_stddev_box_coder.proto .\object_detection\protos\model.proto .\object_detection\protos\optimizer.proto .\object_detection\protos\pipeline.proto .\object_detection\protos\post_processing.proto .\object_detection\protos\preprocessor.proto .\object_detection\protos\region_similarity_calculator.proto .\object_detection\protos\square_box_coder.proto .\object_detection\protos\ssd.proto .\object_detection\protos\ssd_anchor_generator.proto .\object_detection\protos\string_int_label_map.proto .\object_detection\protos\train.proto .\object_detection\protos\keypoint_box_coder.proto .\object_detection\protos\multiscale_anchor_generator.proto .\object_detection\protos\graph_rewriter.proto .\object_detection\protos\calibration.proto
```
```
(tensorflow) C:\tensorflow\models\research> python setup.py build
(tensorflow) C:\tensorflow\models\research> python setup.py install
```

## 13- Tes Tensorflow
tes apakah tensorflow sudah bekerja dengan jupyter notebook : `(tensorflow) C:\tensorflow\models\research\object_detection> jupyter notebook object_detection_tutorial.ipynb`

## 14- Edit generate_tfrecord.py
edit file `generate_tfrecord.py` ubah *class name* sesuai dengan jenis object yang akan di-*training*

![alt text](https://github.com/miqdadmq/Pelatihan-Dataset-Faster-RCNN/blob/master/Pic/tfrecord.PNG)
 

## 15- Buat labelmap
Buat labelmap baru pada folder `/object_detection/training` dengan nama `labelmap.pbtxt`

![alt text](https://github.com/miqdadmq/Pelatihan-Dataset-Faster-RCNN/blob/master/Pic/labelmap.PNG)

## 16- Membuat data untuk *training*
Konversi data .xml ke .csv untuk menghasilkan `train_labels.csv` dan `test_labels.csv` pada folder `\object_detection\images`

```
cd C:\tensorflow\models\research\object_detection
python xml_to_csv.py
```

Buat data `train.record` dan `test.record`

```
python generate_tfrecord.py --csv_input=images\train_labels.csv --image_dir=images\train --output_path=train.record
python generate_tfrecord.py --csv_input=images\test_labels.csv --image_dir=images\test --output_path=test.record
```

## 17- Konfigurasi *pipeline* untuk training dataset
Masuk ke `C:\tensorflow\models\research\object_detection\samples\configs` dan salin `faster_rcnn_inception_v2_pets.config` ke folder `training` kemudian edit seperti berikut ini :

### -
Pada bagian `model` ubah `num_classes` sesuai dengan jumlah kelas yang dibuat

### - 
Pada bagian `fine_tune_checkpoint`:
`C:/tensorflow/models/research/object_detection/faster_rcnn_inception_v2_coco_2018_01_28/model.ckpt`

### -
Pada bagian `train_input_reader` ubah `input_path` dan `label_map_path` menjadi :
Input_path : `C:/tensorflow/models/research/object_detection/train.record`
Label_map_path: `C:/tensorflow/models/research/object_detection/training/labelmap.pbtxt`

### -
Pada bagian `eval_config` ubah `num_examples` sesuai dengan jumlah gambar pada direktori `\object_detection\images\test`

### -
Pada bagian `eval_input_reader` ubah `input_path` dan `label_map_path` menjadi :
Input_path : `C:/tensorflow/models/research/object_detection/test.record`
Label_map_path: `C:/tensorflow/models/research/object_detection/training/labelmap.pbtxt`

## 18- *Run Training*
Jalankan *training* dengan perintah
```
python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_inception_v2_pets.config
```

## 19- Tensorboard
Untuk melihat grafik proses pelatihan atau *training* dataset buka `anaconda cmd` baru dan ketikkan :
```
(tensorflow) C:\tensorflow\models\research\object_detection>tensorboard --logdir=training
```

### 20- *Export* Inference_graph
Jika training selesai, *export* inference_graph sesuai dengan jumlah `model-ckpt.XXXX` pada folder `\object_detection\training`, misalnya `model-ckpt.5642` 
```
python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/faster_rcnn_inception_v2_pets.config --trained_checkpoint_prefix training/model.ckpt-XXXX --output_directory inference_graph
```

