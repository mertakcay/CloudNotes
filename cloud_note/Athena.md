# Athena
Veri tabanı gibi sorgulama işlemlerini yapmaya veriyi taramayı sağlar.S3 üzerinden avro json xml gibi farklı formatları okuma olanağına sahiptir.
- Athena S3 deki tüm veriyi üzerine almaz S3 üzerinden okuma işlemi sağlar.
- Athena sadece aynı regiondaki verileri okuyabilir fakat bunu kırmak için Glue Crawler ı aynı region a atayıp bunun üzerinden bir okuma sağlayabiliriz.
- Athena Glue metastore üzerinden ilgili infoyu alır. 
- Takım query sayılarını bireysel query sayılarını sınırlayabilir veya görebilirsin. Bunun sebei fazla yükü belirlemek için kullanabiliriz.
- TB başına 5 dolares
- TB başına işlem yaptığı için yükleme maliyeti üzerinden fiyatlanır Buda bizi spesifik queryler için maliyeti düşürmüş olur.

## Security
- IAM,ACL,S3 Policy
- Encypt result
- Cross acc KMS
- TLS

## Anti patterns
Genellikle küçük sorgular için kullanılır.
- Yüksek yoğunluklu raporlamada maliyetten dolayı sıkıntılı QuickSight kullanılır burada
- ETL de kullanılmaz onun yerine Glue kuulan

## Tavsiyeler
- Performans için columnar data kullan(ORC,Parquet)
- Küçük verileri load et
- Partitionlar üzerinden işleme git (/datdiri/datdat/dat tarzında)
- Son sürümünde ACID trans özellikleri garantilendi