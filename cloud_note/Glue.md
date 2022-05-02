# AWS Glue
- ETL işlevini sağlar  
- Spark gibi çalışır fakat cluster manage etmene gerek yoktur.
- Glue Crawler ise S3 deki ETL edip bunu athena, redshift, EMR gibi yapılara yollar. 
- Glue tam olarak Unstructered S3 ile RDBMS gibi düzenle databaseler arasındaki interface görevi görür.

# Glue Kullanarak Verimli Şekilde Unstructered Datayı Sorgulama
- S3 ye live veriyi yüklerken windowing yapısını kullan genel timestapten özele doğru inerek daha kolay sorgulama imkanı sun eğer daha sık sorgu için device_id gibi bir durumun var ise bunu primary window olarak kullan ki ilerideki sorgularda daha hızlı erişebilesin.

# AWS Glue + Hive
- Hive kısaca EMR cluster üzerine SQL sorgusu gibi sorgular atar. ***HiveQL***


# Glue ETL
Veriyi transform etmek için otomatik olarak scriptle veya console üzerinden kullanılır. Kısaca preprocessing için kullanılır.
- Automatik olarak datayı transform edebilir.
- SSL ve serverside şifreleme vardır.
- Event-driven mimariye sahiptir
- DPU(data işleme birimi) eklenebilir. Spark balancerları için
- Errorlar cloudwatch ile handle edilebilir.
- Buradan gelen transformed data DB veya S3 gibi yerlere yazılabilir.
- Manage etme tamamen Glue ya aittir ve bunu schedlur edip trigger ile kullanabiliriz.
- DynamicFrame içinde her bir record şeklinde tutulum sağlar. Sparktakine oldukça benzerdir.
- Preprocessing de yaptığımız bundle işlemleri(null silme/doldurma) olanağı sağlar. Klasik bir db gibi filter/join/map gibi işlem olanaklarınıda bize sunar.
- CSV,Json,Avro(Apache), parquet, XML gibi çeşitli data formatlını destekler.
- Apache Spark ML transformationlarını destekler.
- Nested Schema' yı destekler.
- ETL'de sagemaker, zeplin gibi farklı notebooklar dahil edilebilir.

# Glue DataBrew
- Grafiksel olarak bir pre-process ortamı sunar ve farklı kaynakları source, resource edebilir.

# Glue Data Lake Formation
- Şifreleme, kolay lake oluşturma, veri toplama depolama monitör etme transformation gibi durumları sağlayan bir alt başlık.
- Aslında yaptığı şey S3 Data Lake için bir interface görevi görmektir.
- ACID temel olarak garantiler.
- Otomatik data compressleme gibi olanakları vardır.


