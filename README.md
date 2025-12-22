# Kat Analizi ve İndirgeme Programı (v2.6)

Bu proje, **tek HTML dosyası** ile çalışan, tarayıcı üzerinde (telefon/PC) kullanılan bir kat/alan analiz aracıdır.

- **Kat Analizi**: İstenilen fiyat + pazarlık payı ile yuvarlanmış fiyatı hesaplar; kat bazında alan/indirgeme oranı girilerek **toplam indirgenmiş alan** ve **birim değer (TL/m²)** üretir. Kayıtlar tarayıcıda saklanır.
- **Dubleks Analiz**: Yasal Alan ve Mevcut Alanı ayrı sekmelerde tutar. Normal/Çatı/Teras değerlerini hesaplar; canlı özet gösterir ve kayıtları tarayıcıda saklar.

> Veri saklama: `localStorage` kullanılır. Sunucu gerekmez.

---

## 1) Dosyalar

- `index.html` (tek dosya)  
  İçinde: HTML + CSS + JavaScript

---

## 2) Kurulum / Çalıştırma

### PC
1. Dosyayı `index.html` adıyla kaydet.
2. Çift tıkla aç (Chrome/Edge önerilir).

### Telefon
- Dosyayı telefona kopyala ve tarayıcıyla aç.
- Daha stabil kullanım için dosyayı bir statik host’a koyabilirsin (GitHub Pages gibi).

---

## 3) Özellikler

### A) Kat Analizi Sekmesi
**Girdiler**
- İstenilen Fiyat (TL)
- Pazarlık Payı (%) (opsiyonel)
- Bodrum Kat Sayısı
- Normal Kat Sayısı
- Kat ekleme listesi
- Her kat için:
  - Alan (m²)
  - İndirgeme Oranı

**Hesaplama Mantığı**
- Pazarlık varsa: `indirimli = fiyat * (1 - pazarlık/100)`
- Yuvarlama: `100.000 TL` basamağına yuvarlama
- İndirgenmiş alan: `Alan / Oran`
- Toplam indirgenmiş alan: tüm katların indirgenmiş alanları toplamı
- Birim Değer: `Yuvarlanmış Fiyat / Toplam İndirgenmiş Alan`

**Canlı Hesap**
- Fiyat ve kat satırlarındaki alan/oran değiştikçe otomatik güncellenir.
- “Kaydet ve Listeye Ekle” ile kayıt oluşturur.
- Kayıtlar “Düzenle / Sil” ile yönetilir.

**Kayıt Depolama**
- `localStorage` anahtarı: `katAnaliziRecordsV2`

---

### B) Dubleks Analiz Sekmesi
Dubleks Analiz, **iki alt sekme** içerir:
- **Yasal Alan**
- **Mevcut Alan**

Her iki tarafta da aynı girişler vardır:
- Normal Kat Alan (m²)
- Birim Değer (TL/m²)
- Çatı Kat Alan (m²)
- Çatı Kat Oran (%)
- Teras Alan (m²)
- Teras Oran (%)

**Hesaplama Mantığı**
- Normal Kat Değer: `NormalAlan * BirimDeğer`
- Çatı Kat Etkin Birim: `BirimDeğer * (ÇatıOran/100)`
- Çatı Kat Değer: `ÇatıAlan * ÇatıEtkinBirim`
- Teras Etkin Birim: `BirimDeğer * (TerasOran/100)`
- Teras Değer: `TerasAlan * TerasEtkinBirim`
- Toplam: `NormalDeğer + ÇatıDeğer + TerasDeğer`

**Canlı Özet**
- Yasal ve Mevcut için ayrı ayrı:
  - Normal / Çatı / Teras değerleri
  - Toplam değer
  - Toplam Alan: `Normal Alan + Çatı Alan` (Teras dahil edilmez)
  - Ayrıca **Birim Değer (TL/m²)** bilgisi canlı hesaplanır:
    - Normal: `NormalKatDeğer / NormalKatAlan`
    - Çatı: `ÇatıKatDeğer / ÇatıKatAlan`
    - Teras: `TerasDeğer / TerasAlan`
- Not: Canlı özet içinde **Yasal + Mevcut birleşik toplam gösterilmez** (istenen davranış).

**Kayıt Depolama**
- `localStorage` anahtarı: `dubleksAnalizRecordsV1`
- Kayıtlar bozulmasın diye eski formatlar için `normalizeDubleksRecord()` ile geriye dönük uyum korunur.

---

## 4) Kullanım Notları

- Tüm veriler tarayıcıda saklanır. Tarayıcı verilerini temizlersen kayıtlar silinir.
- Aynı cihaz/aynı tarayıcıda kalıcıdır; farklı cihazlarda otomatik taşınmaz.
- Telefon klavyesinde sayısal giriş için `inputmode` ayarlanmıştır.

---

## 5) Sürüm / Değişiklik Özeti (v2.6)

- Üst sekmeler: **Kat Analizi** / **Dubleks Analiz**
- Kat Analizi’ne **Canlı Hesap** eklendi.
- Dubleks Analiz’e:
  - Yasal & Mevcut alt sekmeleri
  - Teras alan/oran
  - Canlı özet: değerler + toplam alan
  - Canlı özet: Normal/Çatı/Teras için **Birim Değer (TL/m²)** gösterimi eklendi.
- Dubleks canlı özetinde **Yasal+Mevcut birleşik toplam** gösterimi yoktur.

---

## 6) Sorun Giderme

- “Kayıtlar yok oldu”:
  - Tarayıcı geçmişi/veri temizleme yapılmış olabilir.
  - Farklı tarayıcı/cihaz kullanılıyor olabilir.
- “Canlı hesap güncellenmiyor”:
  - Alan/Oran girişlerinde boş/0 değer varsa ilgili satır 0 kabul edilir.
  - Oran yüzde olarak girilmelidir (örn: 60).

---

## 7) Lisans
Kişisel kullanım / proje içi kullanım için uygundur. (İstersen buraya lisans ekleyebilirsin.)
