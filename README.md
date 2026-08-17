# Google-Play-Store-Measurement-Analysis-Rating-Scoring-Product-Sorting-NLP

> Google Play Store uygulama ve oyunlarını; kullanıcı puanları, yorum sayısı, indirme sayısı, yorum faydalılığı ve güncellik gibi farklı kriterleri birlikte değerlendirerek daha adil ve güvenilir şekilde sıralamayı amaçlayan bir veri bilimi projesidir.

---

## 📌 Proje Hakkında

Uygulama mağazalarında ürünlerin değerlendirilmesinde genellikle yalnızca **ortalama puan** gibi tek bir metrik kullanılmaktadır.

Ancak yalnızca ortalama puana bakmak yanıltıcı sonuçlara yol açabilir.

Örneğin:

- 50 kullanıcıdan 4.9 puan alan bir uygulama,
- milyonlarca kullanıcıdan 4.6 puan alan bir uygulamadan

daha yüksek sırada görünebilir.

Bu nedenle bu projede yalnızca ortalama puana bağlı kalmak yerine; **puan güvenilirliği, yorum hacmi, indirme sayısı, yorum faydalılığı ve yorum güncelliği** gibi farklı sinyaller birlikte değerlendirilmiştir.

Projenin temel amacı, kullanıcıya yalnızca "en yüksek puanlı" ürünleri değil, **kalitesi ve kullanıcı ilgisi daha güvenilir şekilde kanıtlanmış ürünleri** sunabilecek bir sıralama yaklaşımı geliştirmektir.

---

## 🎯 Projenin Amaçları

- Ortalama puanın tek başına oluşturabileceği yanlılığı incelemek
- Düşük yorum sayısına sahip ürünlerin avantajını azaltmak
- Bayesian Average Rating ile daha güvenilir puanlar oluşturmak
- IMDB Weighted Rating ile oy hacmini puanlamaya dahil etmek
- İndirme ve yorum sayılarını sıralama sistemine dahil etmek
- Yorumların faydalılık ve güncellik değerlerini analiz etmek
- Ürünleri çok faktörlü bir sıralama skoru ile değerlendirmek
- Yorumları yalnızca faydalılık sayısına göre değil, birden fazla kriterle sıralamak
- Kullanıcı yorumlarından anlamlı içgörüler elde etmek

---

## 📊 Veri Seti

Projede Google Play Store uygulama ve oyunlarına ait bilgiler ile kullanıcı yorumları kullanılmıştır.

Veri setinde:

- **217 uygulama**
- **335 oyun**
- **1 milyondan fazla kullanıcı yorumu**
- Uygulama ve oyun bilgileri
- Kullanıcı puanları
- Yorum tarihleri
- Faydalı oyları
- İndirme sayıları
- Toplam puan sayıları

bulunmaktadır.

### Veri Dosyaları

```text
apps_info.csv
apps_reviews.csv
games_info.csv
games_reviews.csv

Ürün Bilgileri

Ürün veri setlerinde aşağıdaki bilgiler bulunmaktadır:

id
product_name
description
score
ratings_count
downloads
content_rating
section
categories
Kullanıcı Yorumları

Yorum veri setlerinde:

id
review_text
review_score
review_date
helpful_count

gibi değişkenler bulunmaktadır.
```
## Veri Ön İşleme

Uygulama ve oyun verileri başlangıçta ayrı veri setleri halinde tutulmuştur.

Analiz sürecinde bu veri setleri ortak bir yapıya dönüştürülerek birleştirilmiştir.

Uygulama Bilgileri
        +
Oyun Bilgileri
        ↓
Birleştirilmiş Ürün Verisi

Uygulama Yorumları
        +
Oyun Yorumları
        ↓
Birleştirilmiş Yorum Verisi


Ürün ve yorum tabloları: id + product_type

alanları üzerinden birleştirilmiştir.

Ayrıca yorumların:

- Uzunluğu
- Güncelliği
- Faydalılık değeri

gibi özellikleri analiz için kullanılmıştır.

---
## ⭐ Ürün Puanlama Analizi
1. Ortalama Puan

İlk olarak klasik ortalama puan yaklaşımı incelenmiştir.

Ancak sonuçlar, yalnızca ortalama puana göre sıralama yapmanın yanıltıcı olabileceğini göstermiştir.

Örneğin:

| Ürün | Ortalama Puan | Yorum Sayısı |
| :--- | :---: | :---: |
| Word Search: Word Puzzle Games | 4.75 | 157 |
| Word Bend | 4.72 | 85 |
| Thimbleweed Park | 4.60 | 1.226 |
| Zombie Hunt: Apocalypse Games | 4.55 | 1.241 |

Word Search ve Word Bend yüksek ortalama puanlara sahip olsa da yorum sayıları oldukça düşüktür.

Bu nedenle puanların güvenilirliği sorgulanabilir.

2. Zaman Ağırlıklı Puanlama

Uygulamaların zaman içerisinde değişebileceği düşünülerek yorumların güncelliği puanlamaya dahil edilmiştir.

Yorumlar yaşlarına göre farklı ağırlıklarla değerlendirilmiştir:

```text
0–30 gün     -> 1.0
30–90 gün    -> 0.9
90–180 gün   -> 0.8
180+ gün     -> 0.7
```

Bu yaklaşım ile daha güncel yorumların ürünün mevcut durumunu daha iyi temsil etmesi amaçlanmıştır.

3. Faydalılık Ağırlıklı Puanlama

Kullanıcıların faydalı bulduğu yorumları belirlemek için helpful_count değişkeni kullanılmıştır.

Bu sayede yalnızca yorum sayısı değil, kullanıcılar tarafından ne kadar faydalı bulunduğu da değerlendirmeye dahil edilmiştir.

Elde edilen faydalılık ağırlıklı skor:

```text
2.9217
```
olarak hesaplanmıştır.

4. Hibrit Puanlama

Zaman ağırlıklı puan ile faydalılık ağırlıklı puan birleştirilerek hibrit bir puanlama sistemi oluşturulmuştur.

Kullanılan ağırlıklar:

```text
Zaman Ağırlığı       → %50
Faydalılık Ağırlığı  → %50
```
Elde edilen hibrit skor:

```text
3.0650
```

olmuştur.

Bu yaklaşım hem yorumların güncelliğini hem de kullanıcılar tarafından faydalı bulunmasını dikkate almaktadır.

### 🧮 Gelişmiş Ürün Sıralama Sistemi
5. Bayesian Average Rating (BAR)

Ürünlerin 1–5 yıldız arasındaki puan dağılımları kullanılarak Bayesian Average Rating (BAR) hesaplanmıştır.

Amaç, az sayıda yorum alan ürünlerin yalnızca yüksek ortalama puanları nedeniyle üst sıralara çıkmasını engellemektir.

Örneğin:

```text
Word Search
Ortalama Puan → 4.75
Yorum Sayısı  → 157
BAR Skoru     → 4.57
```
Buna karşılık daha yüksek yorum hacmine sahip ürünler daha güvenilir bir puan elde edebilmektedir.

BAR yöntemi sayesinde düşük yorum sayısına sahip ürünlerin oluşturabileceği istatistiksel avantaj azaltılmıştır.

6. IMDB Weighted Rating

İkinci gelişmiş puanlama yaklaşımı olarak IMDB Weighted Rating / Shrinkage yöntemi uygulanmıştır.

Bu yöntemde:

```text
Ürün puanı
Oy sayısı
Platform ortalaması
Minimum oy eşiği
```
birlikte değerlendirilmiştir.

Amaç, düşük oy sayısına sahip ürünlerin puanlarını genel ortalamaya doğru dengelemek ve yüksek oy hacmine sahip ürünlerin güvenilirliğini korumaktır.

Örneğin:

```text
Words of Wonders: Crossword


Puan          → 4.9
Oy Sayısı     → 5.52 Milyon
IMDB Skoru    → 4.842
```
Bu sonuç, hem yüksek puana hem de milyonlarca kullanıcı tarafından doğrulanmış oy hacmine sahip ürünlerin sıralamada güçlü şekilde öne çıkmasını sağlamıştır.

7. Çok Faktörlü Hibrit Sıralama

Projenin en önemli aşamalarından biri, farklı metrikleri tek bir sıralama skorunda birleştirmektir.

Final sıralama skoru aşağıdaki ağırlıklarla oluşturulmuştur:

```text
Metrik	Ağırlık
Bayesian Average Rating	%40
IMDB Weighted Rating	%25
İndirme Sayısı	%20
Yorum Sayısı	%15
```

Final skor:

```text
Final Score =
    0.40 × BAR Score
  + 0.25 × IMDB Score
  + 0.20 × Downloads
  + 0.15 × Review Count
```
İndirme sayısı, yorum sayısı ve IMDB skorunun farklı ölçeklerde bulunması nedeniyle bu değişkenler MinMaxScaler kullanılarak 1–5 aralığına normalize edilmiştir.

## 🥇 Final Sıralama Sonuçları

Çok faktörlü sıralama sonucunda öne çıkan bazı ürünler:

| Ürün | Tür | Final Skoru |
| :--- | :---: | :---: |
| Google Calendar | App | 3.814 |
| Google Play Games | App | 3.199 |
| Scanner Radio - Police Scanner | App | 3.195 |
| Western Union Send Money Now | App | 3.163 |
| My Earthquake Alerts - Map | App | 3.146 |
| Thimbleweed Park | Game | 3.137 |
| Zombie Hunt: Apocalypse Games | Game | 3.136 |
| purple | Game | 3.132 |
| Simon's Cat Match! | Game | 3.128 |
| Dice Dreams™️ | Game | 3.124 |

Bu sıralama, yalnızca ortalama puana göre oluşturulan sıralamadan önemli ölçüde farklılaşmıştır.

Örneğin ham ortalama puanına göre listenin üst sıralarında yer alan:

```text
Word Search
Word Bend
```
gibi ürünler, düşük yorum hacimleri nedeniyle final sıralamada aynı avantajı koruyamamıştır.

Buna karşılık Google Calendar ve Google Play Games gibi yüksek kullanıcı etkileşimine sahip ürünler, yüksek indirme ve yorum hacimleri sayesinde üst sıralarda yer almıştır.
---
Yorum Sıralama Sistemi

Projenin bir diğer önemli bölümü kullanıcı yorumlarının sıralanmasıdır.

Yorumları yalnızca helpful_count değerine göre sıralamak yerine üç farklı kriter birlikte değerlendirilmiştir:

-  Faydalılık
- Bilgi değeri / yorum uzunluğu
-  Güncellik

Oluşturulan yorum sıralama skoru:

```text
%60 → Faydalılık
%40 → Bilgi Değeri
       ×
     Güncellik Faktörü
```
şeklinde oluşturulmuştur.

Bu yaklaşım sayesinde yalnızca çok fazla faydalı oy alan yorumlar değil, aynı zamanda detaylı ve güncel yorumlar da öne çıkarılabilmektedir.

📈 Kullanılan Yöntemler

Projede farklı istatistiksel ve veri bilimi yaklaşımları kullanılmıştır:

Ürün Puanlama:
- Ortalama Rating
- Time-Based Weighted Rating
- Helpfulness-Based Rating
- Hybrid Rating
- Bayesian Average Rating
- IMDB Weighted Rating
- Wilson Lower Bound
- Multi-Factor Hybrid Ranking

Yorum Analizi:
- Review Helpfulness Analysis
- Review Length Analysis
- Time Decay
- Review Ranking
- Sentiment Analysis
- N-Gram / Bigram Analysis

Veri İşleme ve Görselleştirme:
- Pandas
- NumPy
- Scikit-learn
- SciPy
- Matplotlib
- Seaborn

## 🔑 Temel Bulgular
1. Ortalama puan tek başına yeterli değildir.

Çok az sayıda kullanıcı tarafından verilen yüksek puanlar, ürünleri gerçek performanslarının üzerinde gösterebilir.

2. Oy hacmi güvenilirliği artırır.

Binlerce veya milyonlarca kullanıcıdan gelen puanlar, birkaç düzine kullanıcıdan gelen puanlara göre daha güçlü bir kanıt oluşturur.

3. Güncellik önemlidir.

Eski yorumlar uygulamanın güncel durumunu yansıtmayabilir.

4. Faydalılık yorum kalitesi hakkında önemli bir sinyal sağlar.

Kullanıcılar tarafından faydalı bulunan yorumlar, ürün hakkında daha değerli bilgiler içerebilir.

5. Çok faktörlü sıralama daha dengeli sonuçlar üretmektedir.

BAR, IMDB Shrinkage, indirme sayısı ve yorum sayısının birlikte değerlendirilmesi, yalnızca ortalama puana dayalı sıralamaya göre daha dengeli bir ürün listesi oluşturmuştur.

## 🛠️ Kullanılan Teknolojiler
Programlama Dili
- Python

Veri Analizi
- Pandas
- NumPy

Veri Görselleştirme
- Matplotlib
- Seaborn

İstatistik
- SciPy
- Bayesian Average Rating
- IMDB Weighted Rating
- Wilson Lower Bound

Veri Ön İşleme
- Scikit-learn
- MinMaxScaler,

NLP
- VADER Sentiment Analysis
- N-Gram / Bigram Analysis

## 📂 Proje Yapısı
google-play-rating-ranking/
│
├── data/
│   ├── apps_info.csv
│   ├── apps_reviews.csv
│   ├── games_info.csv
│   └── games_reviews.csv
│
├── notebooks/
│   └── google-play-rating-ranking.ipynb
│
├── README.md
│
└── requirements.txt

## Projeyi Çalıştırma

Repoyu klonlayın:

```bash
git clone [https://github.com/kullanici-adi/google-play-rating-ranking.git](https://github.com/kullanici-adi/google-play-rating-ranking.git)
```

Proje klasörüne gidin:

```bash
cd google-play-rating-ranking
```

Gerekli kütüphaneleri yükleyin:

```bash
pip install -r requirements.txt
```

Notebook'u çalıştırın:

```bash
jupyter notebook
```

Proje ayrıca Google Colab veya Kaggle Notebook ortamında da çalıştırılabilir.

##  Gelecekte Yapılabilecek Geliştirmeler
- Transformer tabanlı gelişmiş sentiment analizi
- Topic Modeling
- Otomatik rating-sentiment mismatch tespiti
- Kategori bazlı sıralama modelleri
- Spam yorum tespiti
- Zaman serisi tabanlı puan değişimi analizi
- Farklı sıralama ağırlıkları için A/B testleri
- İnteraktif dashboard geliştirilmesi
- Kişiselleştirilmiş uygulama öneri sistemi

## 📌 Sonuç

Bu proje, bir uygulamanın yalnızca yıldız puanına bakılarak değerlendirilmesinin her zaman güvenilir sonuç vermediğini göstermektedir.

**Bayesian Rating + IMDB Shrinkage + Yorum Güncelliği + Faydalılık + İndirme Sayısı + Yorum Hacmi**

gibi farklı sinyallerin birlikte değerlendirilmesiyle daha dengeli ve güvenilir bir sıralama sistemi oluşturulmuştur.

Projenin temel yaklaşımı yalnızca:

> "En yüksek puanlı uygulama hangisi?"

sorusuna cevap vermek yerine:

> **"Kullanıcılar tarafından kalitesi, güvenilirliği ve popülerliği daha güçlü şekilde kanıtlanmış uygulamalar hangileri?"**

sorusuna cevap verebilecek bir sıralama sistemi geliştirmektir.

---

👩‍💻 Geliştirici

Sueda Kazan

Computer Engineer - Jr. Data Scientist
Data Science | Machine Learning | Artificial Intelligence

⭐ Projeyi beğendiyseniz repository'ye yıldız vermeyi unutmayın!
