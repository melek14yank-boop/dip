# Kenar Belirleme Operatörlerinin Karşılaştırmalı Analizi: Sobel vs. Prewitt

## 1. Giriş ve Matematiksel Temeller
Sayısal görüntü işlemede kenar tespiti, pikseller arasındaki ani gri seviye (yoğunluk/gradyan) değişimlerinin saptanması esasına dayanır. Birinci mertebeden türev tabanlı ayrık operatörler olan Prewitt ve Sobel, görüntünün yatay (Gx) ve dikey (Gy) yönlerdeki yoğunluk değişimlerini hesaplamak için 3x3 konvolüsyon çekirdekleri (kernel) kullanır.

Prewitt Operatörü Çekirdekleri:

       Gx (Yatay Gradyan)            Gy (Dikey Gradyan)
       [-1   0   1]                   [-1  -1  -1]
       [-1   0   1]                   [ 0   0   0]
       [-1   0   1]                   [ 1   1   1]

Sobel Operatörü Çekirdekleri:

       Gx (Yatay Gradyan)            Gy (Dikey Gradyan)
       [-1   0   1]                   [-1  -2  -1]
       [-2   0   2]                   [ 0   0   0]
       [-1   0   1]                   [ 1   2   1]

Her iki operatörde de kenar büyüklüğü (Gradient Magnitude) şu formülle elde edilir:
M(x, y) = karekök(Gx^2 + Gy^2) veya yaklaşık olarak M(x, y) ≈ |Gx| + |Gy|

---

## 2. Temel Farklar ve Karşılaştırma Tablosu

| Değerlendirme Kriteri | Prewitt Operatörü | Sobel Operatörü |
| :--- | :--- | :--- |
| **Merkez Ağırlık Katsayısı** | Tüm satır/sütun katsayıları eşittir ([-1, 0, 1]) | Merkez piksele daha fazla ağırlık verilir ([-2, 0, 2]) |
| **Dahili Yumuşatma (Smoothing)** | Basit ortalama alma (düz kutu filtresi etkisi) | Gauss benzeri ağırlıklı ortalama alma |
| **Gürültü Duyarlılığı** | Yüksek frekanslı gürültülere karşı daha hassastır | Gradyana dik yönde lokal yumuşatma yaptığı için gürültüye daha dirençlidir |
| **Kenar Kalınlığı ve Parlaklığı** | Kenarlar daha ince ve homojendir | Kenar yanıtı daha kalın, parlak ve belirgindir |
| **Hesaplama Yükü** | Yalnızca toplama ve çıkarma işlemleri içerir (çok hızlı) | 2 ile çarpım içerir (donanımda tek bir bit kaydırma ile çözülür) |

---

## 3. Deneysel ve Niteliksel Değerlendirme

1. Gürültü Karakteristiği ve Ön Filtreleme:
   * Prewitt Operatörü: Komşu pikselleri eşit ağırlıkla işlediği için homojen olmayan veya grenli/dokulu (örneğin tuğla duvarı gibi pürüzlü) yüzeylerde mikro dokuları da kenar gibi algılama eğilimindedir. Bu nedenle işlem öncesinde güçlü bir Gauss yumuşatma filtresi uygulanması kritiktir.
   * Sobel Operatörü: Merkez satır/sütuna uyguladığı 2 katsayısı sayesinde türev alırken aynı anda lokal bir Gauss yumuşatması gerçekleştirir. Bu durum, gürültünün kenar haritasına olan olumsuz etkisini bastırır.

2. Kenar Keskinliği ve Detay Seviyesi:
   * Prewitt operatörü, arka plan ile nesne kontrastının net ve gürültünün düşük olduğu ortamlarda ince ve net sınır çizgileri üretir.
   * Sobel operatörü ise kenar geçişlerinde daha yüksek genlikli (parlak) yanıt verir; zayıf kenarları öne çıkarmada Prewitt'e kıyasla daha başarılıdır.

---

## 4
