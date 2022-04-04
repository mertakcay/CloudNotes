# Lambda
- Herhangi bir ek gereksinime kalmadan EC2 üzerinde kodu çalıştırıp ek hardware gereksinimlerini bize sağlanayan bir teknolojidir.
- Farklı serverlar arasındaki konuşmayı sağlayan apinin güvenli bir kullanıcı olup olmadığını vb söyleyen scripting alanı gibi düşün
### Lambda Kullanma Sebepleri
1. Hardware monitoring gibi zor işleri yapmak zorunda kalmazsın
2. Serverların ilk alımı ucuz fakat scale etme ücreti oldukça pahalı
3. Ortamı ayırmada, düzenlemede ve izole ortam oluşturmada server, cloud kadar esnek davranamaz

### Lambda Kullanımları
1. S3 veya herhangi bir depolama yerine veri geldiğince lambda triggerlanarak ilgili işlemleri yaptırır. ETL-ELT
2. Belirli saatlerde istenilen işlemler için kurabilirsin.

***Kinesis stream'de data lambdaya iletilmez tam tersine lambda kinesis streamın kapısından verileri okur ve işlemeye başlar***

***Redshift in bestpractise load data durumu COPY komutu ile gerçekleşir.***
***Lambda statless bir teknolojidir yani nerede ne kadar yaptığını aklında tutmaz***
***Eğer stateful bir yapı istersen data saklarken S3 yerine DynamıDB kullan***

### Lambda + Kinesis
- 10.000 kayda kadar batch e izin verir ve fazlası olursa timeout atar
- Tek requestte 6mB dan fazla dosya gönderemez
- Bir veriyi işlerken veri silinesiye veya başarılı olasıya kadar tekrar dener eğer error handle edemezsen loop durumu olabilir
- Asenkron olarak çalışmaz paralel işleme sıkıntı olabilir

### Lambda Cost
- 1M request ücretsiz 400kGB second ücretsiz, gerisi milyon başına 20 cent saklanan gb başına 0.00001667 dolar 

### Anti-Patterns Lambda
- Eğer EC2 gibi büyük veri saklama yerleri ile işlem yapıcaksan ve verin büyük ise multi chain olarak lamda kullan ***splitting data***
- Dynamic websiteler için uygun değildir onun yerine EC2 veya CloudFront gibi yapıları kullan
-