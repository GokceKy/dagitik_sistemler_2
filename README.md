Daha detaylı anlatım ve görsel açıklama için aşağıdaki YouTube videosunu izleyebilirsiniz:  
[https://youtu.be/dNX8OuMTa_Q](https://youtu.be/dNX8OuMTa_Q)
# MPI vs. OpenMP Performans Karşılaştırması

Bu depo, 10 milyon elemanlık veri kümesi üzerinde hibrit MPI + OpenMP uygulamasını göstermektedir. Uygulama, çoklu işlemciler arasında dağıtık hesaplama için MPI'yi kullanırken, her işlem içerisinde OpenMP ile çoklu iş parçacığı (thread) paralelliğini gerçekleştirir.

## Genel Bakış

- **MPI (Message Passing Interface):**
  - **Amaç:** İş yükünü birden fazla işleme dağıtarak dağıtık bellek mimarisinde paralel hesaplama sağlar.
  - **Avantajları:** Birden fazla düğümde ölçeklenebilirlik sunar.
  - **Dezavantajları:** İşlemler arası iletişim nedeniyle ek iletişim maliyeti oluşur.

- **OpenMP (Open Multi-Processing):**
  - **Amaç:** Her MPI süreci içinde, paylaşılan bellek üzerinde çoklu iş parçacıkları kullanarak paralel hesaplama yapar.
  - **Avantajları:** Düşük iletişim gecikmesi ile verimli paralel hesaplama sağlar.
  - **Dezavantajları:** Yalnızca tek bir düğümdeki kaynaklarla sınırlıdır.

## Uygulama İşleyişi

1. **Veri Kümesi Oluşturma:**  
   Ana (master) süreç, 10 milyon rastgele tamsayı içeren bir veri kümesi oluşturur.  
   

2. **MPI ile Veri Dağıtımı:**  
Oluşturulan veri kümesi eşit parçalara bölünerek, `MPI_Scatter` komutu ile her MPI sürecine dağıtılır.

3. **OpenMP ile Paralel İşleme:**  
Her MPI süreci, aldığı veri parçası üzerinde OpenMP kullanarak yerel toplam ve yerel maksimum değeri hesaplar.  
Her iş parçacığı, kendi aktivitesini ekrana yazdırır, örneğin:  

4. **Küresel Sonuçların Hesaplanması:**  
Her sürecin hesapladığı yerel toplamlar ve maksimum değerler, `MPI_Reduce` ile birleştirilerek:
- **Küresel Toplam**
- **Küresel Maksimum** elde edilir.  
Toplam işlem süresi, `MPI_Wtime()` ile ölçülür.

## Örnek Çıktı


## Performans Analizi

- **MPI Katkısı:**  
  - Büyük veri setlerini farklı süreçlere dağıtarak ölçeklenebilirlik sağlar.
  - Birden fazla düğüm arasında çalışabilir.
  - İşlemler arası iletişimde gecikme yaşanabilir.

- **OpenMP Katkısı:**  
  - Her MPI süreci içinde, düşük maliyetli iş parçacığı paralelliği sağlar.
  - Paylaşılan bellek kullanımı sayesinde iletişim maliyeti düşüktür.

- **Hibrit Yaklaşımın Avantajları:**  
  - MPI'nin dağıtık hesaplama yetenekleri ile OpenMP'nin verimli iş parçacığı yönetimini birleştirir.
  - Bu sayede hem düğümler arası hem de düğüm içi paralel hesaplama maksimum verimlilikle gerçekleştirilir.
  - Örneğimizde toplam hesaplama süresi yaklaşık 0.13 saniyede tamamlanmıştır.

## Sonuç

Bu hibrit MPI + OpenMP uygulaması, her iki teknolojinin güçlü yönlerini kullanarak büyük veri setlerinin hızlı ve verimli bir şekilde işlenebileceğini göstermektedir:
- **MPI:** Dağıtık sistemlerde büyük ölçekli veri işleme imkanı sunar.
- **OpenMP:** Aynı düğümde hızlı ve düşük maliyetli paralel hesaplama sağlar.

Bu birleşik yaklaşım, yüksek performans gerektiren uygulamalarda etkili bir model olarak öne çıkmaktadır.
