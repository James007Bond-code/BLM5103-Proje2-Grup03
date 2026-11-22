# BIL545 Veri Madenciliğinde İleri Konular · Proje 2

**Konu 9.7:** Hava kalitesi ve kirlilik epizotları: Pekin PM2.5 verisinde kümeleme, aykırı gözlem ve alt grup analizi

**Dönem:** 2026–2027 Güz · **Öğretim üyesi:** Dr. Selma Güzel · Üsküdar Üniversitesi, Fen Bilimleri Enstitüsü

**Proje 1 deposu:** https://github.com/James007Bond-code/BLM5103-Proje1-Grup03

---

## 🎥 Proje sunum videoları

📁 **Bütün videolar (Google Drive klasörü):** _(bağlantı eklenecek)_

| Sıra | Üye | Anlattığı bölüm | Klasördeki dosya |
| :--- | :--- | :--- | :--- |
| 1 | Süleyman Balsak | Bölüm 1 · Kümeleme | `P2_Bolum1_Suleyman_Balsak.mp4` |
| 2 | Seda Erten | Bölüm 2 · Aykırı gözlem | `P2_Bolum2_Seda_Erten.mp4` |
| 3 | İlkin Urcan | Bölüm 3 · Alt grup analizi | `P2_Bolum3_Ilkin_Urcan.mp4` |
| 4 | Aslıhan Semiz Varlı | Ek paket 1 · Kümeler ve sınıflandırma | `P2_EkPaket1_Aslihan_Semiz_Varli.mp4` |

*(Videolar sırayla izlenmelidir. Erişim izni: "Bağlantıya sahip olan herkes görüntüleyebilir".)*

---

## 👥 Grup (4 kişi · temel kapsam + Ek paket 1)

**Grup adı:** Grup 03 · **İletişim sorumlusu:** Süleyman Balsak

| Öğrenci adı soyadı | Öğrenci no | Sorumlu olduğu alt başlıklar |
| :--- | :--- | :--- |
| **Süleyman Balsak** | 254312022 | Proje 1'den devralınanlar ve düzeltmeler · **Bölüm 1:** kümeleme eğilimi (Hopkins), ölçeklemenin etkisi, k seçimi (dirsek ve kararlılık), KMeans / Gauss karışımı / DBSCAN, küme profilleri |
| **Seda Erten** | 254312008 | **Bölüm 2:** Isolation Forest ve LOF, iki sıralamanın uyumu, en aykırı 20 kayıt, aykırıların modele etkisi |
| **İlkin Urcan** | 244329078 | **Bölüm 3:** mevsime, saat dilimine ve rüzgâr yönüne göre alt grup analizi; hataların nedenleri; model kartı |
| **Aslıhan Semiz Varlı** | 254329024 | **Ek paket 1:** kümelere göre hata analizi, küme bilgisinin sızıntısız öznitelik olarak eklenmesi, kümeye göre ayrı modeller · tez ve araştırma yönleri |

Ayrıntılı katkı beyanı defterin sonundaki **Katkı beyanı** bölümündedir.

---

## 📊 Veri kümesi

- **Ad:** Beijing PM2.5 Data (PRSA), UCI Machine Learning Repository · **Lisans:** CC BY 4.0
- **Kaynak:** Liang, X. vd. (2015). Assessing Beijing's PM2.5 pollution: severity, weather impact, APEC and winter heating. *Proceedings of the Royal Society A*, 471, 20150257.
- **İçerik:** 2010–2014, saatlik; hedefi dolu 41.757 saat. Hedef: kirlilik epizodu = PM2.5 > 75 µg/m³.
- **Veri depoda yok.** Defter veriyi doğrudan UCI'dan indirir.

---

## ▶️ Defterin çalıştırılması

1. `BIL545_Proje2_Beijing_PM25.ipynb` dosyasını Google Colab'da açın.
2. **Çalışma zamanı → Tümünü çalıştır.** Ek paket kurulumu gerekmez; Colab ücretsiz katmanında yaklaşık 1–2 dakika sürer.
3. Tohum `SEED = 42`; sonuçlar her çalıştırmada aynıdır.

---

## 🧭 Değerlendirme protokolü (Proje 1'den devralındı)

- Son %20 (14 Ocak – 31 Aralık 2014) dokunulmayan test dönemi; bütün analiz ve seçimler geliştirme verisinde (2010 – Ocak 2014).
- Geliştirmede `TimeSeriesSplit(n_splits=5, gap=24)`; ölçekleme, kümeleme ve aykırılık modelleri her katta yalnızca eğitim verisiyle kurulur.
- Ölçütler: F1 (epizot) ve dengeli doğruluk, ortalama ± std.

---

## 📌 Sonuç özeti

- **Proje 1'den düzeltme:** `year` ve `day` sütunları çıkarıldı (F1 0,726 → 0,730). Model: rastgele orman (100 ağaç, `max_depth = 10`). **Test (2014): F1 0,753, dengeli doğruluk 0,737.**
- **Kümeleme:** Hava verisi kararlı **üç hava tipine** ayrılıyor (kararlılık ARI 0,98; Hopkins 0,85, sütunlar karıştırılınca 0,64). Hedef kümelemeye girmediği hâlde epizot oranı tipler arasında **%19** (soğuk-kuru kuzeybatı havası) ile **%58** (soğuk-durgun kış havası) arasında değişiyor: kışın kirliliği soğuk değil durgunluk belirliyor. DBSCAN küme bulamıyor; hava uzayı sürekli.
- **Aykırı gözlem:** Isolation Forest ve LOF farklı aykırılık türleri buluyor (Spearman ρ = 0,36). En aykırı saatler çoğunlukla aşırı hava olayları (Ocak 2013 soğuk dalgası, uzun yağışlı fırtınalar) ve bunlarda kirlilik düşük: gerçek ama olağandışı kayıtlar. Bir saatte çiy noktası sıcaklıktan yüksek (olası ölçüm hatası). Aykırıların modele etkisi: _(eklenecek)_
- **Alt grup analizi:** Model kışın iyi (F1 0,83), yazın zayıf (F1 0,66, yanlış alarm %52). Yazın sorunu **dağılım kayması**: yaz epizot oranı geliştirmede %54, 2014'te %38. Kuzeybatı rüzgârında epizotların yalnızca %65'i yakalanıyor; durgun havada yanlış alarm %70.
- **Ek paket 1:** Küme bilgisi öznitelik olarak eklenince F1 0,730 → 0,742 ve katlar arası std 0,057 → 0,037; fark std içinde. Kümeye göre ayrı modeller katkı sağlamıyor.

---

## 📁 Dizin yapısı

```text
├── README.md                          # Grup, veri, çalıştırma, sonuç özeti
└── BIL545_Proje2_Beijing_PM25.ipynb   # Kod, çıktılar ve bütün yazılı cevaplar
```

---

## 🤖 Yapay zekâ beyanı

Claude (Anthropic) bu projede kodlama ve yazım asistanı olarak kullanıldı: defterin kodunun yazılması ve optimize edilmesi, sızıntı kontrolü, yorum ve rapor metinleri için ilk taslaklar. Konu ve model seçimi, Colab'da çalıştırma, çıktıların kontrolü, metinlerin düzeltilmesi ve sunum grup tarafından yapıldı. Ayrıntısı defterdeki **Yapay zekâ beyanı** bölümünde.
