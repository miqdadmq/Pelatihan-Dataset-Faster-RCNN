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

## Install Tensorflow-GPU
```bash
pip install --ignore-installed --upgrade tensorflow-gpu
```
