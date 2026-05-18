# ENM421_Veri-Bilimine-Giri-Grup-3-Repository
ENM421_Veri Bilimine Giriş Grup 3 Repository
Kullanılan Yöntemler ve Teknolojiler

**1. Veri Toplama**

	Film Seçimi: Aksiyon & Macera, Korku & Gerilim ve Dram & Biyografi türlerinden, farklı bütçeleri kapsayan homojen dağılımlı 150 adet film veri seti olarak belirlenmiştir. 
	YouTube API Entegrasyonu: Google Cloud Console üzerinden alınan YouTube Data v3 API kullanılarak, Python scriptleri ile Google Colab ortamında veriler çekilmiştir. Her film fragmanı için en popüler 500 yorum ve bu yorumların beğeni sayıları alınmıştır. 
	Finansal Veriler: Filmlerin vizyon öncesi yapım bütçeleri ve vizyon sonrası gişe hasılatı verileri TMDB (The Movie Database) üzerinden elde edilmiştir. 
	
**2. Veri Ön İşleme (Pre-processing)**

	Dil Filtrelemesi: Toplanan 66.532 yorumun %90.7'sini oluşturan İngilizce yorumlar model tutarlılığı için veri setinde tutulmuş, diğer diller analize dahil edilmemiştir. 
	Gürültü Temizliği: 3 karakterden kısa olan, sadece emojilerden oluşan, mükerrer veya boş olan anlamsız yorumlar temizlenmiştir.
	
**3. Duygu Analizi (Sentiment Analysis)**

	NLP Modeli: Cardiff NLP grubu tarafından geliştirilen, sosyal medya metinleri için optimize edilmiş "Twitter-RoBERTa-base for Sentiment Analysis" yapay zeka modeli kullanılmıştır. 
	Ağırlıklandırma: Yorumlar Pozitif, Negatif ve Nötr olarak etiketlenmiştir. Eşit ağırlıklandırma yerine, yorumların aldığı beğeni sayıları logaritmik olarak formüle dahil edilerek "Ağırlıklı Duygu Skoru" hesaplanmıştır. 
	
**4. Makine Öğrenmesi ve Modelleme**

	Veri seti %80 Eğitim (92 Film) ve %20 Test (24 Film) olarak ikiye ayrılmıştır. 
	Bağımlı değişken olarak logaritmik dönüşüm uygulanmış "Gişe Hasılatı", bağımsız değişken olarak ise "Yapım Bütçesi" ve yorum analiz özellikleri kullanılmıştır. 
	Tahminleme için Random Forest Regressor ve XGBoost Regressor algoritmaları kullanılmış; modeller GridSearchCV ile optimize edilmiştir. 
	
**Özet Sonuçlar ve Çıktılar**
  **Model Performansı**
	En İyi Model: "Bütçe + Kategori + Duygu" parametrelerini içeren "Tam Özellik Seti" ile kurulan Random Forest modeli, R^2 = 0.596 skoru ile en iyi performansı göstermiştir. 
	Bütçe ve kategori tek başına gişenin sadece %2.7'sini açıklarken, duygu analizi gişenin %42.2'sini tek başına açıklayabilmiştir. Tüm veri setinin entegre edilmesiyle açıklayıcılık %59.6'ya ulaşmıştır. 
Gişeyi Etkileyen Faktörler (Feature Importance)
Random Forest algoritmasının özellik önem (feature importance) sonuçlarına göre:
	Ortalama Beğeni (%33): Bir yorumun ortalama kaç beğeni aldığı gişeyi açıklayan en güçlü değişkendir. 
	Bütçe (%24): İkinci sıradaki en önemli değişkendir. 
	Toplam Beğeni (%16): Üçüncü sıradaki etken olmuştur. 
	Kategori Etkisi: Analiz sonucunda film türünün (kategorisinin) gişeyi açıklamada etkisinin neredeyse sıfır olduğu gözlemlenmiştir. 
Model Analizi ve Kısıtlar
	Model, düşük ve orta bütçeli filmlerde başarılı tahminler üretirken; franchise bilinirliği yüksek blockbuster yapımlarda sistematik olarak gerçek değerin altında tahmin yapmıştır. 
	GridSearchCV hiperparametre optimizasyonu sonrası yapılan Cross Validation (Çapraz Doğrulama) sonuçlarında yüksek varyans (0.179 standart sapma) gözlemlenmiştir. Bu durum, 116 filmlik sınırlı veri setinden kaynaklı bir "overfitting" (ezberleme) eğilimine işaret etmektedir. 
	Franchise bilinirliği, yönetmen ve oyuncu ünü gibi dış faktörlerin modele dahil edilmemesi mevcut modelin kısıtları arasındadır. 

