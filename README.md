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
https://github.com/tensorflow/models.git

## 10- Download Model untuk dataset yang diinginkan
Karena yang saya gunakan adalah faster rcnn maka saya download faster_rcnn_inception_v2_coco pada :
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md
Kemudian buat folder `images`, `training`, dan `inference_graph` pada `C:\tensorflow\models\research\object_detection`

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
```

```
(tensorflow) C:\tensorflow\models\research> python setup.py install
```

## 13- Tes Tensorflow
tes apakah tensorflow sudah bekerja dengan jupyter notebook : `(tensorflow) C:\tensorflow\models\research\object_detection> jupyter notebook object_detection_tutorial.ipynb'

## 14- Membuat data untuk *training*


