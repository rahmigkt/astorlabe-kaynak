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
`eserler-kunye/`'nin **açıklamalı/zenginleştirilmiş hâli** — şu an sadece a-001'den a-100'e kadar (devamı gelecek). Aynı ID'ler ama artık `curatorialText`, `keywords`, `subCategory`, `creator`, `period` gibi alanlar dolu. Bazı alanlarda (`category`, `year` formatı) da küçük düzeltmeler var. v1 silinmedi, iki versiyon karşılaştırılabilir durumda duruyor.
- `a-001-025.json`, `a-026-050.json`, `a-051-075.json`, `a-076-100.json`

### `hetega-envanter/eslestirme/`
a-XXX künye kayıtlarını gerçek fiziksel/Excel envanter numaralarıyla eşleştiren, mükerrer kayıtları tespit eden çalışmalar. Bölüm bölüm ilerliyor (her bölüm bir kitap alt-başlığına karşılık geliyor).
- `bolum-01-astronomi-rasathaneler.json` — Astronomi: Rasathaneler ve Gözlem Aletleri (kitap s. 13-21, diğer aletler s. 104-108)
