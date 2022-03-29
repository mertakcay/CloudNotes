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