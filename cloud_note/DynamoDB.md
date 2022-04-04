# DynamoDB
- 400kb item başına 
- NoSQL
- Dtypes -> Klasiklerin yanında List, Map, String/Number/Binary Set var
- Hashli unique key ve partition key yapısı bulunur
- ***PK = Partition key + Sort Key*** Partition üzerinden grouping yapabilirsin
- Read Capacity Units(RCU) : Okuma birim miktarı
- Write Capaticy Units(WCU) : Yazma birim miktarı
***Scale seçeneği var***
## Write Capatiy Units Miktarı
-1 Kb kadar her bir saniyede gelen obje sayısı ile çarpılır 1 * 10 obje = 10WCU
-1 Kb dan sonra ise Kb boyutu * unit sayısı(unit->x miktarı->y --> x*y = WCU sayısı)

### Strongly Consistant Read vs Eventually Consistent Read
Eventually Consistent : Veri replicate edildirken tekrar okumak  yapma durumu oluştuğunda inconsistant veri oluşur burada hatalı veri gelebilir default olarak bu geçerlidir
Strongly Consistant Read : Veri replicate edilirken bu veri doğru olucak garantisini sağlamaya çalışır.

## Read Capaity Units Miktarı
- 4KB buffer miktarı vardır yani 4KB kadar object sayısı kadar RCU gerekir fakat üstüne bir durum olursa Object boyutu/4Kb yaparak her bir obje için kaç tane buffer miktarı olucağını belirleriz. Bunun yanında eğer strongly consistant ise * 1, eventually ise * 2 olması gerekir.

- Eğer elimizde 24 Kb lık 20 tane object okunucaksa saniyede ve hepsi eventually ise  --> 24/4 *20 = 120*2 = 240 strongly ise 120 tane RCU gerekiyor.***Küsüratlı bir yapı var ise buffer'ı bölemiyeceğimize göre üst 4kB katına yuvarlarız.

## Throttling
- Eğer RCU veya WCU yu aşarsak ProvisionedThrouhputExceededExceptions hatası fırlatır
- Bunu çözmek için;
1. Exponential back-off SDK içindeymiş
2. Distribute partitions keys 
3. DAX kullan

## DynamoDB Partitions
- Her partitions boundry e sahip ve 3000RCU/ 1000WCU max capacity, max 10GB
## DynamoDB Batch
- 25 tane ***PutItem veya DeleteItem*** yapılabilir
- 16mB kadar veri yazabilir her bir veri 400kB kadar olabilir
- Request sayısını azaltmak için aktif kullanılır paralel olarak delete ve write işlemi gerçekleştiği için tıkanma yaratmaz.
- 100 item'a kadar ve 16 mB itema kadar batch olarak ***get*** edebilir.
- Paralellik devamke
- ***Query için*** 1mB kadar sorgu atılabilir ve fazlası varsa paging kullan
- ***Scan için*** oldukça boktan bir tercih fakat gerekirse bil yinede tüm tabloya bakarak ilgili filtre var mı kontrol eder RCU nun içinden geçer 1mB kadar kullanım sağlar ve fazlası varsa paging kullan. Paralel olarak multi partition kullanabilirsin bir nebze darboğazdan kurtarır.***Hive ile ortaklaşa kullanmayı sever ***

## DynamoDB Indexes
### LSI
- Tablo yaratılırken oluşturulmak zorunda 
### GSI

## DynamoDB DAX
- DynamoDB' yi cache'lemek için kullanılır. Çok fazla read olma durumunda aktif olarak kullanılır.
- Her bir DAX cluster da 10 tane node bulunur ve 5 dakikaya kadar yaşayabilir 3 tane tavsiye edilir.
- Encrption olarak KMS, VPC, IAM, CloudTrail etc
## DynamoDB Streams
- Elimizdeki tüm tabloları live hale getirmeye yarar
- Direkt olarak kullanılmaz live hale gelirken KCL Lib veya Lambda ile Kinesis Adapter kullanılır.(Yine ve veyalara boğulmuş bir cümle KCL lib veya lambdadan birini seç ucuna Kinesis adapter tak) Kafkadaki producer-consumer mantığı tarzında işaretlenir.
- Genellikle elasticSearch ile tercih edilir.
- Production ortamında cross-region kullanmak teerrrcih sebebi olabilir. Farklı region tepki süresi etc.
- Kinesis te 24H retention süresi var 
- Batch size 1k Row 6mB kadar saklayabilir

## DynamoDB TTL
- WCU veya RCU yemez ve bunu background serviceleri düzenler ek bir maliyeti yoktur
- Genellikle 48 saatte yok olur, yok olduktan sonra serivce 24 saat içinde geri getirme özelliğine sahiptir.

## DynamoDB Security & Other Feature
- VPC ile bağlantı kurulabilir
- IAM ile yetkilendirilebilir
- KMS / SSL / TLS ile encryption var.
- ***Backup ve Restore*** Zaman kaybı olmadan direkt olarak restore edilebilir
- ***Global Tablolarla** multi-region, fully replicated ve yüksek performanslı tablo tutumu gerçekleştirilebilir.
- *** AWS Migration service ile neredeyse tüm dbleri -MySQL dahil- dynamodbye migrate edebiliyoruz.

## DynamoDB Large Object Storing

- DynamoDb de her bir dosya 400kb olduğu için daha büyük objeleri saklamak için AWS S3 ye yüklenip adresi dynamoDB ye referans gösterilir.

- Bunun yanında ***az ulaşılan*** dosyalar var ise bunu AWS S3' e saklayıp adresini dynamoDB ye refere ederiz çünkü, DynamoDB her bir okuma yazma durumunda WCU ve RCU costu ortaya çıkartıcaktır buda büyük dosyalarda oldukça fazla olduğu için maliyeti x10 arttıracaktır.