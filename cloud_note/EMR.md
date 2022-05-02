# EMR
- Spark, HBase, Presto, Flink gibi instanceları yönetir.
- Asıl amacı clusterları yönetmektedir. Master, Core ve Task nodeları bulunur.
- Core node genllikle scale, duplicate node durumlarında kullanılır.
- Scalingi yönetmek için CloudWatch ile belirli metrikleri aşıldığı taktirde otomatik olarak scale edebiliriz. Bunu sadece capacity management olarak düşün başka durumlara bağlama.
- Nodelar arası otomatik oalrak şifreli veri geçişi vardır. Fakat S3 gibi storagelar arasındaki haberleşmeyi kendimizin veya çeşitli yöntemlerle şifrelememiz gerekiyor.
- *Bence "Spark" tüm sorunları çözmek için yeterli performans kolaylıkları sağlıyor gidipte hive pig gibi farklı teknolojilere yönelme*
- *Presto* çok fazla connector(farklı big data tech.) sahip olduğu için belki merge etmede kullanılabilir.

2 Tip Cluster Bulunmaktadır. Bunlar; Transient Cluster ve Long Running Cluter
## Transient Cluster
Genellikle data işleme, depolama gibi geçici süreli olarak işler ve işi bittikten sonra kapatılır. Buradaki amaç düşük maliyettir.
## Long Running Cluster 
Büyük veriyi periyodik olarak işleme gibi işlemler bunun içinde yapılır. Sanki batch işlemleri yapıyor gibi düşünebilirsin.

# EMR Storage
2 Farklı tipi bulunuyor bunlar HDFS(Hadoop), EMRFS 
## HDFS
- Block şeklinde saklanır hadoop taki gibi.
- Birden fazla node a blocklar dağıtılarak bir node broke olursa diğeri ayağa kalkarak veriyi korur.
- Her bir block size 128mbtır.
## EMRFS
- EC2 yu tıptı HDFS gibi kullanmayı sağlar.
- Hadoopta cluster kapandığında veri gidiyordu fakat bu tipte cluster kapansa bile hala veriyi korumaya devam eder.
- Hadooptan biraz daha yavaş çalıştığı sölyeniyor.

## EMR Serverless
- Node sayısını vb belirtmeden otomatik olarak bunları belirlemek için kullanılır. Down veya up scaling işleri ile uğraşır.

## DynamoDB ve HBase Farkları
- *DynamoDB* auto scaling, daha kolay entegrasyon gibi özelliklere sahiptir.
- *Hbase* daha dağıtık datalar, daha yüksek throughput ve hadoop ekosistemine uygunluk olarak daha iyi 

## S3DistCP
- Large object copy durumlarında aktif olarak kullanılır. Farklı teknolojiler arasındaki geçişler için ideal

## EMR Security
### IAM Policy
### Kerberos : User auth tokenla giriş yaptırır
### SSH
### IAM Roles 
### Block Public Access

------
## Block/Node Size
- Eğer 50 den daha fazla cluster var ise master node *m5.xlarge tipini* tercih et. AWS baba öyle diyo 
