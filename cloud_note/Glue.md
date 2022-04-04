# AWS Glue
- ETL işlevini sağlar  
- Spark gibi çalışır fakat cluster manage etmene gerek yoktur.
- Glue Crawler ise S3 deki ETL edip bunu athena, redshift, EMR gibi yapılara yollar. 
- Glue tam olarak Unstructered S3 ile RDBMS gibi düzenle databaseler arasındaki interface görevi görür.

# Glue Kullanarak Verimli Şekilde Unstructered Datayı Sorgulama
- S3 ye live veriyi yüklerken windowing yapısını kullan genel timestapten özele doğru inerek daha kolay sorgulama imkanı sun eğer daha sık sorgu için device_id gibi bir durumun var ise bunu primary window olarak kullan ki ilerideki sorgularda daha hızlı erişebilesin.

# AWS Glue + Hive
- Hive kısaca EMR cluster üzerine SQL sorgusu gibi sorgular atar. ***HiveQL***

-
