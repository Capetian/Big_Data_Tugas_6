Kelas: Big Data

Nama: Raden Bimo Rizki Prayogo

NRP: 0511740000139

# Big Data - Tugas 6

# MLlib model to PMML
## Business Understanding
Workflow ini digunakan untuk mengubah suatu model yang sudah di-training menjadi suatu file Predictive Model Markup Languange (PMML). File PMML tersebut kemudian dapat di-compile kembali menjadi suatu model dan di-deploy untuk melakukan prediksi suatu event secara cepat.

Overview Workflow:

![picture](/img/overview_1.PNG)

## Data Understanding

Data yang digunakan adalah dataset Iris Classification yang terdiri dari 3 species bunga iris, masing-masing memiliki 50 sample.


Dataset mengandung 1 kolom klasifikasi dan 4 kolom fitur-fitur suatu iris. 

![picture](/img/dataset.PNG)


## Data Preparation
### Membuat Spark Context
Pertama-tama sebuah context spark perlu disiapkan terlebih dahulu. Spark Context dibuat secara lokal dan menggunakan 2 executor (thread).

![picture](/img/spark_setup.PNG)

![picture](/img/spark_con.PNG)


### Menyiapkan Data Training
#### Mengektraksi Data File Dataset Training

![picture](/img/read_spark.PNG)

Dataset untuk training mengandung 50% data dari dataset secara keseluruhan dan setiap kelas spesies memiliki 25 sample. File dataset dibaca dengan node File Reader dan kemudian diubah menjadi sebuah Spark dataframe menggunakan node Table to Spark. 

## Modelling

![picture](/img/modelling.PNG)

Dataset yang sudah dijadikan spark dataframe dijadikan data training untuk suatu k-Means Clustering. Model yang dihasilkan diubah menjadi suatu PMML dengan node Spark MLlib to PMML.

![picture](/img/modelling_conf.PNG)

Hasil training:

![picture](/img/res_training.PNG)

PMML yang dihasilkan:

![picture](/img/res_PMML.PNG)


## Evaluasi


![picture](/img/evaluation.PNG)

Menggunakan test set dari file dataset training, lakukan penilaian akurasi model berdasarkan nilai entropy. Model yang dijadikan PMML di-compile terlebih dahulu.

Hasil Evaluasi:

![picture](/img/scorer.PNG)


## Deployment 

![picture](/img/Deployment.PNG)

Untuk mensimulasikan prediksi suatu event secara real time, dibuatkan suatu container json yang berisi data yang mengandung fitur-fitur yang diperlukan untuk melakukan prediksi. Data tersebut lalu dilakukan prediksi pada model yang sudah dicompile lalu hasil prediksinya dikirim ke pengirim event sebagai json.

Json yang diterima:

![picture](/img/input_json.PNG)

Hasil yang dikirim:

![picture](/img/output_json.PNG)



# Spark Compiled Model Predictor
## Business Understanding
Workflow ini digunakan untuk mengubah suatu model yang sudah di-training menjadi suatu file Predictive Model Markup Languange (PMML). File PMML tersebut kemudian dapat di-compile kembali menjadi suatu model dan di-deploy untuk melakukan prediksi terhadap data dalam jumlah yang banyak pada suatu cluster Hadoop.

Overview Workflow:

![picture](/img/overview_2.PNG)

## Data Understanding

Data yang digunakan adalah dataset Iris Classification yang digunakan sebelumya.


## Data Preparation

### Menyiapkan Data Training

![picture](/img/read_trn.PNG)

Dataset untuk training mengandung 50% data dari dataset secara keseluruhan dan setiap kelas spesies memiliki 25 sample. File dataset dibaca dengan node File Reader.

## Modelling

![picture](/img/modelling_2.PNG)

Dataset yang sudah dibaca training ke suatu Decision Tree dan Multi Layer Perceptron dengan optimizer RMSProp. Masing-masing model yang dihasilkan diubah menjadi suatu PMML yang kemudian disatukan untuk menjadi PMML yang melakukan ensemble training antara kedua model tersebut. Pembobotan ensemble model yang digunakan adalah majority vote.

Konfigurasi ensemble learning:

![picture](/img/modelling_conf_2.PNG)

PMML yang dihasilkan:

![picture](/img/res_PMML_2.PNG)


## Deployment 

![picture](/img/Deployment_2.PNG)

Dataset test dapat dianggap sebagai data dari suatu cluster. Data tersebut dijadikan suatu spark dataframe lalu dilakukan prediksi pada model yang sudah dicompile. Prediksi dilakukan dengan Spark Compiled Model Compiler untuk mensimulasikan prediksi data secara masal dengan Apache Spark, yang dapat melakukan paralelisasi terhadap pekerjaan prediksi tersebut.

Hasil prediksi:

![picture](/img/Deployment_pred.PNG)


## Evaluasi

![picture](/img/evaluation_2.PNG)

Hasil prediksi dijadikan tabel dan dievalusi dengan scorer.

Hasil Evaluasi:

![picture](/img/scorer_2.PNG)