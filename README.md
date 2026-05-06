# COVID-19 ve Akciğer Hastalıkları Sınıflandırması: PCA ve SVM Optimizasyonu

Bu proje, X-Ray (Röntgen) görüntüleri üzerinden COVID-19, Viral Pnömoni ve Normal akciğer vakalarını yüksek doğrulukla sınıflandırmayı amaçlayan kapsamlı bir makine öğrenmesi ve veri madenciliği çalışmasıdır. Proje kapsamında verideki gürültüyü filtrelemek için **Temel Bileşenler Analizi (PCA)** ve sınıflandırma için hiperparametreleri optimize edilmiş **Destek Vektör Makineleri (SVM)** kullanılmıştır. Bu yöntemin bir çok parametresi denenmiştir ve en iyi olanlar kullanılmıştır.

## Proje Özeti ve Metodoloji

Geleneksel derin öğrenme yöntemlerine alternatif olarak, daha açıklanabilir ve işlem gücü açısından verimli bir model inşa edilmiştir. Modelin başarısı sadece yüksek doğruluk oranıyla değil, aynı zamanda 10-Katlı Çapraz Doğrulama ve ROC analizleri gibi akademik metriklerle de kanıtlanmıştır.

### 1. Veri Seti Hazırlığı
Medikal görüntüleme veri setlerinde sıkça karşılaşılan "dengesiz sınıf (class imbalance)" problemini aşmak için özel bir örnekleme stratejisi izlenmiştir:
* **Dengeli Dağılım:** COVID-19, Normal ve Viral Pnömoni sınıflarının her birinden 1.300 adet görüntü seçilmiştir.
* **Görüntü Boyutu:** Detay kaybını minimize etmek ve işlem yükünü optimize etmek adına görüntüler 200x200 piksel çözünürlüğe getirilmiştir.
* **Normalizasyon:** Piksel yoğunluk değerleri [0, 1] aralığına ölçeklendirilmiştir.

### 2. Özellik Mühendisliği (PCA)
40.000 pikselden oluşan orijinal veri uzayı, PCA kullanılarak bilginin %95.02'sini temsil eden en önemli 191 temel bileşene indirgenmiştir. Bu adım:
* Verideki gürültülerin elenmesini sağlamıştır.
* Modelin eğitim süresini saniyelere indirmiştir.
* Gereksiz boyutlardan kaynaklanan "boyutun laneti (curse of dimensionality)" etkisini ortadan kaldırmıştır.

### 3. Model Optimizasyonu (Grid Search)
SVM modelinin en kararlı hali için `GridSearchCV` yöntemi kullanılarak kapsamlı bir parametre taraması yapılmıştır:
* **Çekirdek (Kernel):** RBF (Radyal Tabanlı Fonksiyon)
* **C Parametresi:** 10 (Hata toleransı ve genelleme dengesi)
* **Gamma:** 0.001 (Veri noktalarının etki alanı)

## Performans Analizi

Modelin başarısı farklı metrikler üzerinden titizlikle test edilmiştir:

* **Doğruluk (Accuracy):** Test seti üzerinde %91-93 bandında başarı sağlanmıştır.
* **10-Katlı Çapraz Doğrulama:** Veri seti 10 farklı kombinasyonda test edilmiş ve ortalama %91.77 (±%1.28) başarı elde edilmiştir. Bu sonuç, başarının tesadüfi olmadığını kanıtlamıştır.
* **ROC/AUC Analizi:** Viral Pnömoni sınıfında 1.00, COVID-19 sınıfında 0.99 AUC değeri elde edilerek sınıflar arası ayrım gücü maksimuma çıkarılmıştır.
* **Hata Analizi:** Yanlış tahmin edilen görüntüler manuel olarak incelenmiş, hataların çoğunlukla düşük kontrastlı veya çok erken evredeki vakalardan kaynaklandığı saptanmıştır.

## Teknik Gereksinimler

Projeyi çalıştırmak için aşağıdaki kütüphanelerin yüklü olması gerekmektedir:

```bash
pip install numpy pandas scikit-learn opencv-python matplotlib seaborn tqdm
