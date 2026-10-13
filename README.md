# EEM4011 Proje: Az kanallı EEG ile sol/sağ yumruk hayali sınıflaması

Bu projede, sabit 0 eşikli ve kişiye özgü parametresi olmayan basit bir kuralla, EEG'den sol ve sağ yumruk hayalinin ne kadar doğru ayırt edilebildiği ölçülmektedir. Makine öğrenmesi kullanılmamaktadır.

## Veri seti

- **Ad:** PhysioNet EEG Motor Movement/Imagery Dataset v1.0.0
- **Link:** https://physionet.org/content/eegmmidb/1.0.0/
- **Kullanılan kısım:** S001–S020 kişileri, R04, R08 ve R12 koşuları (sol/sağ yumruk hayali). T1 = sol, T2 = sağ.
- **Veri bu depoda yok.** Kod, gereken 60 EDF dosyasını PhysioNet'ten kendisi indirir.

## Yöntem (kısa)

1. C3 ve C4 kanallarına 8–13 Hz (mu bandı) Butterworth bant geçiren filtre uygulanır.
2. Her denemede görev penceresi (ipucundan sonra 0,5–4,0 s) ve dinlenme penceresi (ipucundan önce 3,0 s) için güç hesaplanır.
3. ERD_dB = 10·log10(görev gücü / dinlenme gücü), C3 ve C4 için ayrı ayrı.
4. LI = ERD_C3 − ERD_C4. Kural: LI > 0 ise "sol", değilse "sağ" (eşik sabit 0).

## Kodu çalıştırma

1. Bu depodaki `eem4011_proje.ipynb` dosyasını indir.
2. https://colab.research.google.com adresinde **Dosya → Not defteri yükle** ile dosyayı aç.
3. Hücreleri yukarıdan aşağıya sırayla çalıştır. İlk hücre Google Drive'ı bağlar ve 60 dosyayı `MyDrive/eegmmidb` klasörüne indirir.
4. Gerekenler: Google hesabı ve Drive'da yaklaşık 160 MB boş yer. Kurulan ek paket: `pyedflib`.

## Ana sonuç

- 20 kişi, 60 dosya, 900 deneme.
- Baseline doğruluk (kişi başı doğruluğun ortalaması): **0,542 ± 0,056** (ddof=1), medyan 0,544.
- Kişileri yeniden örnekleyen bootstrap (2000 tekrar, tohum 0) ile %95 güven aralığı: **[0,519, 0,566]**.
- İlk iyileştirmeden sonraki değer: ___

## Şekiller

- `sekil_en_kotu3_erd.png`: Doğruluğu en düşük 3 kişide C3 ve C4 mu ERD'si (sol ve sağ denemeler).
- `sekil_binom_guc.png`: 45 denemede kişi başı anlamlılığa ulaşma olasılığı (binom hesabı).
