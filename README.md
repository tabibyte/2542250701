# Conformer mimarisi kullanarak MİDİ müzik üretimi

Bu proje, MİDİ/Sembolik müzik üretimi (Symbolic Music Generation) için **Conformer** mimarisinin performansını, geleneksel **Transformer** mimarisi ile karşılaştırmalı olarak incelemektedir. Çalışma kapsamında BACH (Chorales), POP909 ve MAESTRO veri setleri kullanılarak kapsamlı deneyler yapılmıştır.

## 1. Problemin Tanımı

Müzik üretimi, uzun süreli bağımlılıkların (long-term dependencies) ve yerel dokuların (local textures) aynı anda modellenmesini gerektiren karmaşık bir dizi problemleridir. 
* **Global Bağlam:** Müziğin genel yapısı, tonu ve tekrar eden temaları.
* **Yerel Bağlam:** Notalar arası geçişler, artikülasyon ve zamanlama.

Standart Transformer mimarileri global bağlamı yakalamakta başarılı olsa da, yerel özellikleri yakalamakta bazen yetersiz kalabilmektedir. Bu proje, onları birleştiren **Conformer** mimarisini müzik alanına uyarlayarak, doğruluk (Accuracy) ve Perplexity - PPL metrikleri üzerindeki etkisini araştırmaktadır.

## 2. Veri Seti ve Ön İşleme

Projede üç farklı karakteristiğe sahip veri seti kullanılmıştır:
1.  **BACH (JSB Chorales):** 4 sesli, homofonik ve son derece kurallı koral eserler.
2.  **POP909:** Melodi, eşlik ve ritim içeren modern pop piyano düzenlemeleri.
3.  **MAESTRO (v3.0.0):** Virtüözite ve ifade gücü yüksek klasik piyano performansları.

veri setinde örnek veriler her veri seti dosyasında "/sample_data" klasöründe bulunmaktadır. 

### Ön İşleme Adımları:
* **Format:** Tüm MIDI dosyaları `miditok` kütüphanesi kullanılarak işlenmiştir.
* **Tokenization:** Müzikal veriler **REMI (Revamped MIDI)** formatında tokenize edilmiştir (Pitch, Velocity, Duration, Position).
* **Augmentation:** Veri çeşitliliğini artırmak için bazı testlerde perde kaydırma (Pitch Augmentation) uygulanmamıştır.
* **POP909** veri seti ön işlemede maestroyla aynı tipde MİDİ formatına getirilmiştir.
* **Bach Chorales** veri seti ise önce .csv dosyalarından MİDİ formatına dönüştürülmüştür.

## 3. Model Mimarisi ve Yaklaşım

### Referans Model: Transformer
Standart "Decoder-only" GPT benzeri mimari kullanılmıştır.

### Önerilen Model: Conformer
Konuşma tanıma (ASR) için önerilen mimarinin "Lite" versiyonu müziğe uyarlanmıştır.
* **Macaron Net Yapısı:** Feed-Forward blokları Attention bloğunun öncesine ve sonrasına bölünmüştür.
* **Convolution Modülü:** Self-Attention'dan sonra gelen 1D Konvolüsyon katmanı, müzikteki yerel (local) ilişkileri yakalar.
* **Gerekçe:** Müziğin hem matematiksel hem de yapısal doğasını aynı anda modellemek, local ve global contexti daha iyi yakalamak.

## 4. Kurulum ve Çalıştırma Talimatları

Projeyi yerel ortamınızda çalıştırmak için:

### Gereksinimler
Python 3.8+ ve CUDA destekli bir ortam önerilir.
Veri setinin kısayolu kodda düzenlenip Kaggle, Jupyter Notebook veya Google Colab üzerinden sırasıyla hücreleri çalıştırarak eğitim başlatılabilir.

Kaggleda veri seti ve kodları kullanmak için:
Bach - https://www.kaggle.com/datasets/pranjalsriv/bach-chorales-2
POP909 - https://www.kaggle.com/datasets/easterharry/pop909
Maestro - https://www.kaggle.com/datasets/kritanjalijain/maestropianomidi

## 5. Deneysel Sonuçlar (Model Çıktıları)

<img width="869" height="129" alt="image" src="https://github.com/user-attachments/assets/c5fc6d1b-5279-4acd-9bcf-fe52a017318d" />

## 6. Grafikler ve Görseller
Eğitim sürecine ait Loss, Accuracy ve PPL grafikleri her modelin kendi klasöründe, modellerin karşılaştırma görselleri ise veri setinin ana klasöründe yer almaktadır. Ayrıca modellerin ürettiği örnek MIDI dosyaları her model dosyasında generated.mid olarak bulunmaktadır. 
