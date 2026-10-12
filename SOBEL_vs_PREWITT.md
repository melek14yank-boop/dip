# Kenar Belirleme Operatörlerinin Karşılaştırmalı Analizi: Sobel vs. Prewitt

## 1. Giriş ve Matematiksel Temeller
Sayısal görüntü işlemede kenar tespiti, pikseller arasındaki ani yoğunluk (gradyan) değişimlerini saptamaya dayanır. Birinci mertebeden türev tabanlı operatörler olan **Prewitt** ve **Sobel**, görüntünün yatay ($G_x$) ve dikey ($G_y$) yönlerdeki yoğunluk değişimlerini hesaplamak için 3x3 konvolüsyon çekirdekleri (kernel) kullanır.

### Prewitt Çekirdekleri:
$$G_x = \begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix}, \quad G_y = \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

### Sobel Çekirdekleri:
$$G_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix}, \quad G_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

Her iki operatörde de kenar büyüklüğü ve yönü gradyan büyüklüğü formülüyle hesaplanır:
$$M(x, y) = \sqrt{G_x^2 + G_y^2} \quad \text{veya yaklaşık olarak} \quad M(x, y) \approx |G_x| + |G_y|$$

---

## 2. Temel Farklar ve Karşılaştırma Kriterleri

| Kriter | Prewitt Operatörü | Sobel Operatörü |
| :--- | :--- | :--- |
| **Merkez Katsayı Ağırlığı** | Eşit ağırlıklı ($[-1, 0, 1]$) | Merkez piksele ağırlık verilir ($[-2, 0, 2]$) |
| **Dahili Yumuşatma (Smoothing)** | Basit ortalama alma (düz kutu filtresi etkisi) | Gauss benzeri ağırlıklı ortalama alma |
| **Gürültüye Karşı Hassasiyet** | Yüksek frekanslı gürültüye daha hassastır | Gürültüyü bastırma kabiliyeti daha yüksektir |
| **Kenar Kalınlığı ve Belirginliği** | Kenarlar daha ince ve homojendir | Kenarlar daha kalın, parlak ve belirgindir |
| **Hesaplama Karmaşıklığı** | Düşük (katsayılar sadece $\pm 1$ ve $0$) | Düşük-Orta (2 ile çarpım bit kaydırma ile yapılır) |

---

## 3. Görsel ve Niteliksel Değerlendirme

1. **Gürültü Karakteristiği ve Ön Filtreleme:**
   - **Prewitt**, komşu pikselleri eşit ağırlıkla işlediği için homojen olmayan veya grenli/dokulu (örneğin tuğla duvarı gibi pürüzlü) yüzeylerde mikro dokuları ve gürültüyü de kenar gibi algılama eğilimindedir.
   - **Sobel**, merkez satır/sütuna uyguladığı $2$ katsayısı sayesinde türev alırken aynı anda lokal bir Gauss yumuşatması gerçekleştirir. Bu durum, gürültünün kenar yanıtına olan olumsuz etkisini doğal olarak azaltır.

2. **Kenar Keskinliği ve Detay Seviyesi:**
   - Prewitt operatörü, arka plan ile nesne kontrastının net ve gürültünün düşük olduğu ortamlarda ince ve net sınır çizgileri üretir.
   - Sobel operatörü ise kenar geçişlerinde daha yüksek genlikli (parlak) yanıt verir; zayıf kenarları öne çıkarmada Prewitt'e kıyasla daha başarılıdır.

---

## 4. Sonuç ve Öneriler

* **Prewitt Operatörü:** Hesaplama kaynağının çok kısıtlı olduğu gömülü sistemlerde veya gürültü seviyesi düşük, önceden güçlü bir yumuşatma filtresi (Gauss blur) uygulanmış görüntülerde hızlı ve doğrusal kenar tespiti için uygundur.
* **Sobel Operatörü:** Gerçek dünya uygulamalarında, hafif gürültülü ortamlarda ve genel amaçlı kenar tespit boru hatlarında (örneğin Canny algoritmasının gradyan hesaplama adımında) daha dengeli ve kararlı sonuçlar verdiği için endüstri standardı olarak tercih edilir.
