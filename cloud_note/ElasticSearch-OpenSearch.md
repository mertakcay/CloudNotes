# Elastic Search
- Petabyte derecede dataları anlık olarak işlemek için kullanılır
- Sparktan daha hızlıdır
- Visul. için kibana ile aynı dashboardı kullanır.
- Google analytics yetmediği noktada opensearch dashboard çözümleri daha uygun olur.
- Doküman, Type(custom schemaya bağlı yapı oluşturma), index konspetlerine sahiptir.
- Shardlar üzerinden okuma sağlar burada her biri için 1 tane Primary shards ve 2 tane replikası bulunur. Asıl read okuma Primary üzerinden gerçekleşir diğerleri ise thorughtput için kullanılır.

## Service
- Serverless yapıya sahiptir.
- Master node genellikle veri tutma ve işleme ile ilgilenmez genellikle manage etme üzerine yoğunlaşır.
- Domain ise genellikle cluster bilgilerini tutar
- Snapshots S3 veri yedekleme ile ilgilenir
- Zone awareness performans için bölgeler arasındaki ücretlendirme ve performans miktarını ayarlar.
### Security
- IAM veya IP policty
- Signed Request
- VPC
- Coginto
- Resource based policity

## Anti Patterns
- OLTP için kullanma bunun için RDS veya Dynamo kullanman daha mantıklı
-Ad-Hoc data query için kullanma bunun yerine athena daha mantıklı *Dayanılan nokta sadece aws guide da böyle yazıyor.*

## Index Management And Design 
- *Hot Storage* genellikle EC2 gibi çalışır, hızlıdır fakat oldukça maliyetlidir.

- *UlturaWarm Storage* genellikle log dataları gibi değiştirilemez dataları saklamak için en iyi yöntemdir. Daha yavaştır.

- *Cold Storage* daha yavaştır ve daha ucuzdur ve eski old logları saklamak için kullanmaktadır.

- Tüm bunların arasında geçiş kolaydır.

### Index statemaent
- ISM otomatik olarak arttırmaya yöneliktir. Eski datalar silinirse yenilenebilir. Otomatik olarak snapshot alır.
- Otomatik olarak eski datalardaki information ı toplamak içinde kullanılabilir.
- Otomatik olarak yapılan işlemlerde agg groupping gibi işlemleride periyodik olarak tanımlayabiliriz.
- Relability ve latency için yapılan farklı region copylerinde illaki tüm datayı taşımak zorunda değilsin istenilen indexleri belirleyerek taşıma imkanına sahiptir.
- Failure durumunu handle etmek için en az 3 master node kullanılır.
- Genellikle shardları unbalanced olarak kullanma ve çok fazla shards a ayırma bunun sebebi shardları ayarlarken fazla memory allocation olmasıdır.
- Çok eski data saklanıcaksa buda index overthinker durumuna düşürebilir. Bunun için glacier ı kullan.