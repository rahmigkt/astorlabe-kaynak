# Kaynak Deposu

Bu repo bir **canlı site deposu değildir** — hetega.com / mukantara.com gibi hiçbir yere deploy edilmez. Sadece HETEGA envanter çalışması için ham kaynakları saklamak amacıyla kullanılır, ihtiyaç oldukça buradan referans alınır.

## İçindekiler

### `islamda-bilim-ve-teknik/cilt-txt/`
Fuat Sezgin — "İslâm'da Bilim ve Teknik" serisinin I-V ciltlerinin djvu OCR metinleri (tam metin).
- `cilt-I.txt` ... `cilt-V.txt`

### `hetega-envanter/eserler-kunye/`
Kitaptaki tüm eserlerin künye bilgisi — a-001'den a-505'e kadar sıralı kayıtlar (cilt, sayfa, envanterNo, açıklama vb.).
- `a-001-105.json`, `a-106-205.json`, `a-206-305.json`, `a-306-405.json`, `a-406-505.json`

### `hetega-envanter/eserler-kunye-v2-acikamali/`
`eserler-kunye/`'nin **açıklamalı/zenginleştirilmiş hâli** — **a-001'den a-505'e kadar TAMAMLANDI** (505/505, boşluk yok, tekrar yok — doğrulandı). Aynı ID'ler ama artık `curatorialText`, `keywords`, `subCategory`, `creator`, `period` gibi alanlar dolu. v1 silinmedi, iki versiyon karşılaştırılabilir durumda duruyor.
- `a-001-025.json` ... `a-476-505.json` (20 dosya, tam seri)
- Kullanıcının ayrıca ekleyeceğini belirttiği "konuyla ilgisiz" bölüm henüz gelmedi (505 rakamının dışında, ayrı bir ekleme olacak)

### `hetega-envanter/excel-dogrulama/`
Excel fiziksel envanter listesi ile 5 ciltlik a-XXX künye kayıtları arasındaki **tam eşleştirme** — 251 satır, format: `excel_no | İngilizce başlık | a-XXX ID | Türkçe başlık | cilt-sayfa`. 251 satırdan 250'si eşleşmiş, 1 tanesi açıkça "BULUNAMADI" işaretli (20202, "Quadrant Another", II-202). Kullanıcı 3 istisna olduğunu belirtti, 2.si henüz netleşmedi.
- `excel-5cilt-eslesme-tam.txt`

### `hetega-envanter/eslestirme/`
a-XXX künye kayıtlarını gerçek fiziksel/Excel envanter numaralarıyla eşleştiren, mükerrer kayıtları tespit eden çalışmalar. Kullanıcının verdiği resmî bölümleme planı (kitap "Altın Çağın Teknolojik Mirası" 2019 kataloğunun kendi sayfa numaralandırmasına göre):

| # | Bölüm | Kitap Sayfaları | Diğer Aletler Sayfaları | Durum |
|---|---|---|---|---|
| 1 | Astronomi: Rasathaneler ve Gözlem Aletleri | s.13-21 | s.104-108 | ✅ `bolum-01-astronomi-rasathaneler.json` |
| 2 | Astronomi: Usturlaplar | s.23-32 | s.110-118 | ✅ `bolum-02-usturlaplar.json` |
| 3 | Astronomi: Ekvatoryumlar + Muhelif Aletler | s.33-42 | s.120-128 | ✅ `bolum-03-ekvatoryumlar-mekanik.json` |
| 4 | Coğrafya ve Seyrüsefer | s.43-46 | s.130-134 | ✅ `bolum-04-cografya-seyrusefer.json` |
| 5 | Saatler | s.47-54 | s.136-138 | ✅ `bolum-05-saatler.json` |
| 6 | Geometri ve Tesviye | s.55-57 | s.140-144 | ✅ `bolum-06-geometri-tesviye.json` |
| 7 | Optik | s.59-62 | s.146 | ✅ `bolum-07-optik.json` |
| 8 | Fizik ve Teknik | s.63-71 | s.148-152 | ✅ `bolum-08-fizik-teknik.json` |
| 9 | Askeri Teknoloji | s.73-77 | s.154-158 | ✅ `bolum-09-askeri-teknoloji.json` |
| 10 | Kimya | s.79-83 | s.160-164 | ❌ **henüz gönderilmedi** |
| 11 | Tıp | s.85-93 | s.166-167 | ✅ `bolum-11-tip.json` (bkz. not) |

**Düzeltme notu:** Tıp dosyası ilk başta yanlışlıkla "bolum-09" olarak kaydedilmişti (kaynak belge görsellerden derlenmiş markdown olduğu için bölüm numarası belirsizdi). Kullanıcının bu tabloyu paylaşmasıyla doğru yerine (Bölüm 11) taşındı. O dosyanın içindeki kayıt bazlı `kitap_sayfa`/`kitap_cilt` alanları katalog sayfası değil, Sezgin'in 5 ciltlik "İslam'da Bilim ve Teknik" eserindeki asıl kaynak sayfasını gösteriyor (çoğunlukla Cilt IV, s.37-90) — iki ayrı sayfalama sistemi olduğu dosya içinde ayrıca not edildi.

**Bölüm 10 (Kimya) eksik — sıradaki boşluk budur.**
