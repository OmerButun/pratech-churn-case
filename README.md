# 🛒 Pratech E-Ticaret Müşteri Kayıp (Churn) Tahmini

## 🎯 Proje Özeti
E-ticaret platformlarında yeni müşteri kazanma maliyeti, mevcut müşteriyi elde tutma maliyetinden çok daha yüksektir. Bu projenin amacı; müşterilerin demografik bilgileri, platform içi davranışları ve alışveriş geçmişlerini analiz ederek platformu terk etme (churn) potansiyeli yüksek olan kitleyi istatistiksel doğrulukla önceden tespit etmektir.

## 📂 Proje Yapısı
Sürdürülebilirlik ve tekrarlanabilirlik (reproducibility) ilkeleri gereği proje standart veri bilimi klasör mimarisine uygun tasarlanmıştır:
* `data/` : Modelin beslendiği ham CSV veri setini içerir. Notebook içerisindeki kod, veriyi bu klasörden uzaktan (raw url) çekecek şekilde yapılandırılmıştır.
* `pratech-churn.ipynb` : Veri ön işleme, özellik mühendisliği, modelleme ve görselleştirme adımlarını içeren ana Jupyter Notebook dosyasıdır.
* `requirements.txt` : Projenin yerel ortamlarda tek tıkla çalıştırılabilmesi için gerekli Python kütüphanelerini barındırır.

## 🛠️ Yöntemler ve Teknolojiler
Proje boyunca istatistiksel ve analitik yaklaşımlar merkeze alınarak aşağıdaki adımlar uygulanmıştır:

1. **Veri Ön İşleme (Missing Value Imputation):** Eksik veriler basit atamalar yerine, varyansı ve örüntüyü korumak amacıyla makine öğrenmesi tabanlı **KNN (K-Nearest Neighbors) Imputer** ile doldurulmuştur. Kategorik verilerde Kukla Değişken Tuzağından (Dummy Variable Trap) kaçınılarak One-Hot Encoding uygulanmıştır.
2. **Özellik Mühendisliği (Feature Engineering):** Ham verilerden e-ticaret dinamiklerine uygun yeni ve anlamlı metrikler üretilmiştir:
   * `Loyalty_Score`: Sistemde kalma süresi x Memnuniyet Puanı.
   * `Cashback_per_Tenure`: Ortalama zaman başına düşen iade kazancı.
3. **Dengesiz Veri Yönetimi:** Churn verilerindeki asimetrik dağılımı çözmek ve modelin çoğunluk sınıfına ezber yapmasını (Accuracy paradoksu) engellemek adına sadece eğitim setine **SMOTE** algoritması uygulanmıştır.
4. **Modelleme:** Random Forest Sınıflandırıcısı kullanılmıştır.

## 📊 İstatistiksel Bulgular ve Performans
Sınıflandırma probleminde sadece "Doğruluk (Accuracy)" oranına bakmak yanıltıcıdır. Bizi terk edecek müşterileri kaçırmamak asıl hedefimiz olduğu için model, **Recall (Duyarlılık)** metriği hedeflenerek optimize edilmiştir.
* **Accuracy:** %91
* **F1-Score:** %75
* **Recall:** %79 *(Model, platformu gerçekten terk edecek riskli grubun %79'unu başarıyla tespit etmiştir.)*

## 💡 Yönetici Özeti ve Aksiyon Planı
Modelimizin istatistiksel çıktıları doğrultusunda pazarlama bütçesini optimize etmek için şu adımlar önerilmektedir:
1. **Mikro-Hedefleme:** Modelin `predict_proba` çıktılarına göre sistemi terk etme ihtimali en yüksek (Risk Skoru > %80) olan VIP risk grubuna acil müdahale edilmelidir.
2. **Kişiselleştirilmiş İndirim:** Riskli gruba genel reklamlar çıkmak yerine, veri setinde tutunmaya güçlü bir etkisi olduğu tespit edilen "Cashback (Para İadesi)" kurgusu üzerinden anlık hediye bakiyeler tanımlanmalıdır.
3. **Proaktif Şikayet Yönetimi:** Sistemde şikayeti bulunan riskli müşterilerin geçmiş destek talepleri önceliklendirilerek hızlıca çözüme kavuşturulmalıdır.
