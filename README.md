# TazeMarket - İnteraktif E-Ticaret Hata (Bug) Simülasyon Platformu

TazeMarket, yazılım geliştirmede sık karşılaşılan iş mantığı kusurlarını (bug) göstermek için tasarlanmış eğitici bir e-ticaret platformudur. Standart bir e-ticaret uygulamasından farklı olarak, bu proje, geliştiricilerin ve QA test uzmanlarının belirli sistem hatalarını gerçek zamanlı olarak kasıtlı şekilde aktif edip kapatmalarına ve bu hataların mağaza üzerindeki yıkıcı etkilerini gözlemlemelerine olanak tanıyan interaktif bir Yönetici Paneline (Admin Panel) sahiptir.

## Özellikler

- **Gerçek Zamanlı Hata Yönetimi:** 6 farklı iş mantığı hatasını anında aktif edin veya kapatın. Kullanıcı arayüzü yaptığınız yapılandırmalara anında yanıt verir.
- **Görsel Hata Gösterimi:** Bir hatanın alışveriş sepetini, toplam fiyat hesaplamalarını ve ürün stoklarını nasıl etkilediğini, backend loglarını okumaya gerek kalmadan tam olarak görün.
- **Dinamik Stok Yönetimi:** Uç durumları (örneğin negatif stok zafiyetleri) test etmek için ürün stoklarını manuel olarak ayarlayabileceğiniz yerleşik bir yönetici aracı.
- **Modern Mimari:** ASP.NET Core Web API arka ucu ve modern Glassmorphism arayüzü kullanan hafif, hızlı, Vanilla JavaScript ön ucu ile oluşturulmuştur.
- **Durum Senkronizasyonu:** Hata konfigürasyonları yerel bir JSON dosyasına (`ecommerce_bugsettings.json`) kaydedilerek, arayüz ve C# sınıfları arasında tutarlı bir durum sağlanır.

## Simüle Edilen Hatalar

Bu proje şu anda aşağıdaki kritik iş mantığı zafiyetlerini simüle etmektedir:

1. **CartRemoveAllBug (Sepet Ürün Silme Hatası):** Bir kullanıcı sepetinden tek bir adet ürünü çıkarmaya çalıştığında, sistem miktara bakılmaksızın ürünü sepetten tamamen siler.
2. **CartSkipFirstItemBug (Fiyat Hesaplama Hatası):** Toplam fiyat hesaplama döngüsü sepete eklenen ilk ürünü atlar, yani müşteriye ilk ürünü bedavaya verir.
3. **Cart50PercentDiscountBug (Gizli İndirim Hatası):** Eğer sepette tam olarak 3 ürün varsa, hiçbir bildirim veya geçerli iş kuralı olmaksızın toplam fiyata %50 indirim uygulanır.
4. **OrderServiceDoubleStockDecreaseBug (Çifte Stok Düşme Hatası):** Satın alınan her bir ürün için sistem envanterden 2 adet düşer, bu da stok takibinin bozulmasına ve stokların iki kat hızlı tükenmesine neden olur.
5. **ProductNegativeStockBug (Negatif Stok İzni Hatası):** Sistem bir satın alma işlemini tamamlamadan önce stok seviyelerini doğrulamada başarısız olur ve envanterin negatif sayılara düşmesine izin verir.
6. **OrderServiceIncorrectBalanceCheckBug (Kusurlu Bakiye Kontrolü):** Ödeme sistemi ters bir mantık kullanır, müşterinin bakiyesi sepet toplamından *az* olduğunda işlemleri onaylar.

## Başlarken

### Gereksinimler
- .NET 10.0 SDK (veya uyumlu bir sürüm)

### Uygulamayı Çalıştırma
1. Repoyu yerel makinenize klonlayın.
2. Terminalinizde proje dizinine gidin.
3. Sunucuyu başlatmak için aşağıdaki komutu çalıştırın:
   ```bash
   dotnet run
   ```
4. Web tarayıcınızı açın ve yerel sunucu adresine gidin (genellikle `http://localhost:5000` veya `https://localhost:5001`).

## Proje Yapısı

- **Core/**: C# iş mantığı sınıflarını (`Cart.cs`, `Product.cs`, `OrderService.cs`) ve yapılandırma yöneticisini (`BugSettings.cs`) içerir.
- **Tests/**: Simüle edilen hataların nasıl yakalanacağını gösteren NUnit entegrasyon, black-box, gray-box ve white-box test setlerini içerir.
- **wwwroot/**: Vanilla JS ön uç uygulamasını, stillendirmeyi (`style.css`), HTML yapısını ve ürün görsel dosyalarını içerir.

## Kullanım Rehberi

1. Ekranın sol tarafındaki "Sepete Ekle" butonlarını kullanarak sepetinize ürünler ekleyin.
2. Sepetin ve stok düşümlerinin normal, hatasız davranışını gözlemleyin.
3. Belirli hataları açıp kapatmak için sağ taraftaki **Sistem Hataları Yönetimi** panelini kullanın.
4. Etkinleştirdiğiniz hataya bağlı olarak sistemin nasıl bozulduğunu görmek için sepetle tekrar etkileşime geçin.
5. Hatanın C# kod tabanında nerede bulunduğu ve sonuçları hakkında teknik detaylar için sayfanın altındaki **Sistem Hata Açıklamaları** bölümünü inceleyin.
