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
a-XXX künye kayıtlarını gerçek fiziksel/Excel envanter numaralarıyla eşleştiren, mükerrer kayıtları tespit eden çalışmalar. Bölüm bölüm ilerliyor (her bölüm bir kitap alt-başlığına karşılık geliyor).
- `bolum-01-astronomi-rasathaneler.json` — Astronomi: Rasathaneler ve Gözlem Aletleri (kitap s. 13-21, diğer aletler s. 104-108)
- `bolum-02-usturlaplar.json` — Astronomi: Usturlaplar — Standart, Küresel, Evrensel, Kadran (kitap s. 23-32, diğer aletler s. 110-118, 39 kayıt)
- `bolum-03-ekvatoryumlar-mekanik.json` — Astronomi: Ekvatoryumlar, Muhelif Aletler ve Mekanik Aletler (kitap s. 33-42, diğer aletler s. 120-128, 34 kayıt)
- **Bölüm 1-3 tamam, kesintisiz.** Devamı (Bölüm 4+) bekleniyor.
