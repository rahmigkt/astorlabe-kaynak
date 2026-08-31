# HETEGA PROJESİ — DEVİR METNİ

Bu metin, önceki bir Claude oturumundan devralınıyor. Önceki oturum **çok sayıda ciddi hata yaptı** ve kullanıcı bu hatalardan dolayı büyük hayal kırıklığı yaşadı. Bu metni okuyan Claude, aşağıdaki hataları **tekrarlamamak zorunda**. Kullanıcının sözünü dinlemek, acele etmemek, doğrulamadan işlem yapmamak esastır.

---

## 0. EN ÖNEMLİ KURAL — ASLA İHLAL ETME

**Kullanıcı bir "örnek/deneme" istediğinde, ASLA canlı/aktif bir sayfanın üzerine yazma.** Önceki oturum, kullanıcı "bir örnek kart ve sayfa tasarla, onaylayayım sonra devam edelim" dediğinde, gerçek canlı ürün sayfasının (`p120-1`) üzerine doğrudan yazdı. Bu **geri dönüşü olmayan bir hataydı** — kullanıcı bunu açıkça belirtti ve bu, güveni tamamen kıran bir andı.

**Doğrusu:** Her deneme/örnek/prototip için **ayrı bir test yolu** kullan (örn. `/deneme/ornek-kart/`, `/deneme/ornek-sayfa/` gibi hiç indexlenmeyen, canlı veriye dokunmayan bir sayfa). Kullanıcı onayladıktan SONRA gerçek sayfalara uygula.

**Ayrıca:** Kullanıcı hem **kart tasarımı** hem **sayfa tasarımı** istemişti. Önceki oturum sadece sayfayı yaptı, kartı hiç göstermedi. İstenen HER ŞEYİ eksiksiz yap, yarım bırakma.

---

## 1. KİMLİK VE ROLLER

- **Global Science Advancement** = koleksiyonun hukuki sahibi
- **Hüseyin Rahmi Göktaş (kullanıcı)** = danışman/tasarımcı/üretici, projenin sahibi. Türkçe konuşuyor, doğrudan ve net, kararları kendi vermek istiyor, güçlü estetik standartları var. **Çok sabırlı davrandı ama artık ciddi güven sorunu var — her adımda gerçekten doğrulama bekliyor, "yaptım" demeden önce GERÇEKTEN test et.**
- **MUKANTARA** = tasarım/üretim işbirlikçisi
- **HETEGA** = koleksiyonun markası, site: **hetega.com**

## 2. ERİŞİM BİLGİLERİ

- **GitHub PAT:** *(güvenlik nedeniyle bu public repoya yazılmadı — kullanıcıdan isteyin, önceki sohbet geçmişinde mevcut)*
- **hetega-website repo:** `rahmigkt/hetega-website` (canlı site kaynağı, FTP secrets tanımlı, GitHub Actions ile Natro'ya otomatik deploy)
- **astorlabe-kaynak repo:** `rahmigkt/astorlabe-kaynak` (public, ham kaynak verileri — a-XXX künye havuzu, bölüm eşleştirme dosyaları, bu devir metni de burada)
- **Natro:** `94.73.150.132`, cPanel: `u2768844`
- **Yerel klon konumları (bu oturumda):** `/home/claude/hetega-site2` (site), `/home/claude/kaynak-repo` (kaynak veri) — yeni oturumda yeniden `git clone` gerekir

## 3. SİTE MİMARİSİ — KRİTİK TEKNİK GERÇEKLER

### 3a. Astro statik site, kaynak kod YOK
Site Astro ile üretilmiş ama **orijinal kaynak proje kayıp** — sadece derlenmiş `dist` çıktısı (statik HTML + birkaç JS/CSS bundle) repoda. Bu yüzden her değişiklik ya statik HTML dosyalarına doğrudan ya da minified JS bundle'larına regex/string düzenlemeyle yapılıyor. **Bu çok kırılgan bir yöntem, dikkatli ol.**

### 3b. React Hydration GÜVENİLİR DEĞİL
Sitede birkaç yerde (`astro-island` ile) React bileşenleri var ama **hydration'ın çalışıp çalışmadığı garanti değil**. Bu yüzden:
- Envanter sayfasındaki kategori filtre butonları uzun süre **hiç çalışmıyordu** (düz `<button onClick>` idi, `href` yoktu) — **çözüm: tüm kategori filtrelemesi artık tamamen STATİK sayfalarla yapılıyor** (`/koleksiyonlar/{kategori}/tumu/` — her biri o kategorinin TÜM objelerini JS'siz, hardcoded gösteriyor). Bunu bozma.
- Menü çubuğunun "aşağı kaydırınca gizlen" özelliği React'e bağlıydı, elle oluşturulmuş statik sayfalarda (22 kategori sayfası + Envanter) hiç çalışmıyordu çünkü SiteHeader orada React bileşeni olarak hiç yüklenmiyor. **Çözüm: düz vanilla JavaScript ile `<script>` etiketi olarak 513 sayfanın `<head>`'ine eklendi, hydration'a hiç bağımlı değil.**
- **GENEL KURAL: Yeni bir interaktif özellik eklerken, önce "bu sayfa gerçekten React hydration alıyor mu?" diye sor. Emin değilsen, vanilla JS ile çöz, React'e güvenme.**

### 3c. Cache-busting disiplini — MUTLAKA UYGULA
Her JS/CSS dosyası değiştiğinde, **dosya adını değiştirip** (örn. `products.DUVDXmdd17.js` → `products.DUVDXmdd18.js`), TÜM referans veren dosyaları (diğer JS importları + ilgili HTML dosyaları) güncellemek gerekiyor. Aksi halde Cloudflare/tarayıcı önbelleği eski içeriği göstermeye devam ediyor — **bu defalarca yaşandı ve kullanıcıyı çok yordu.** Her değişiklikten sonra:
1. Yeni ada kopyala, eskisini sil
2. `grep -rl "eski-ad" . --include="*.html" --include="*.js"` ile TÜM referansları bul
3. Hepsini güncelle
4. `grep` ile eski adın hiçbir yerde kalmadığını doğrula
5. `node -e` ile sözdizimini gerçekten çalıştırarak doğrula (sadece görsel kontrol yetmez)

### 3d. Cloudflare/tarayıcı önbelleği — kendi araçların da güvenilmez
`web_fetch` aracı bu domain için **defalarca eski/önbelleklenmiş içerik döndürdü**, hatta bazen `web_search` bu siteyi hiç indexlemiyor (root sayfa "yapım aşamasında" olduğu ve bazı sayfalar `noindex` olduğu için). **Kendi doğrulamanın da yanlış olabileceğini unutma** — mümkünse kullanıcıdan gizli pencerede kontrol etmesini iste, ya da Cloudflare panelinden "Purge Everything" yapmasını rica et.

### 3e. Statik sayfa "dondurulmuş an" sorunu
`/koleksiyonlar/{kategori}/index.html` (vitrin, 6 öne çıkan obje) ve `/koleksiyonlar/{kategori}/tumu/` (tüm objeler) sayfaları **Python script ile veri anlık görüntüsünden (snapshot) üretiliyor**. Canlı veri (`products.DUVDXmdd*.js`) her güncellendiğinde, **bu 22 statik sayfa + Envanter'in statik HTML'i de MUTLAKA yeniden üretilmeli**, yoksa eski/stale içerik göstermeye devam ederler. Bu defalarca unutuldu ve kullanıcı fark etti. **Her veri güncellemesinden sonra bu adımı unutma.**

## 4. GEÇMİŞTE YAŞANAN CİDDİ VERİ HATALARI — TEKRARLAMA

### 4a. Aynı soyadlı farklı tarihi kişilerin karıştırılması
"En uzun/en zengin küratöryel metni seç" gibi otomatik/toplu birleştirme mantıkları, **aynı soyadı taşıyan ama FARKLI yüzyıl/şehirde yaşamış farklı tarihi kişileri** birbirine karıştırdı. Örnek: 11. yüzyıl Toledo'lu "Muhammed b. eş-Şaffar" ile 15. yüzyıl Kahireli "Şemseddîn Muhammed eş-Şaffâr" — isim benzerliği yüzünden bir kişinin metni yanlışlıkla diğerine atandı. **42 grupta, 135 objede bu tür çapraz bulaşma tespit edildi ve düzeltildi.**
**DERS: İsim/soyadı benzerliğine göre otomatik eşleştirme YAPMA. Her atamayı tarih+yer+bağlam üzerinden doğrula, sadece isim yetmez.**

### 4b. Kısa açıklama ile uzun küratöryel metin çelişkisi
Ürün sayfalarında hem kısa `description` hem uzun `curatorialText` gösteriliyordu, bazen ikisi ÇELİŞKİLİ bilgi içeriyordu (yukarıdaki 4a'nın bir belirtisi). **Çözüm: artık sadece TEK bir metin gösteriliyor** (`curatorialText` varsa o, yoksa `description`). İkisini asla yan yana gösterme.

### 4c. "medium" (malzeme) ve "dimensions" (boyutlar) alanları unutuldu
Kullanıcının gönderdiği JSON dosyalarında bu alanlar vardı ama uzun süre canlıya hiç işlenmedi. **DERS: Kullanıcının verdiği JSON dosyalarındaki HER ALANI kontrol et, sadece isim/açıklama/yapımcı ile yetinme.**

### 4d. Özel karakter sadeleştirmesi sırasında yanlış "düzeltme"
ā, ī, ū, ḥ, ṣ gibi transliterasyon işaretleri Türkçe karakterler (ç,ğ,ı,ö,ş,ü) korunarak sadeleştirildi — ama bu işlem defalarca yapılırken bazı kayıtlarda geri alma/yeniden uygulama karmaşası oldu. **Kaynak veride zaten var olan bazı eski yazım hataları da ("cAlâ°uddevle" gibi, ayn işareti yerine "c" harfi kullanılmış) ayrıca düzeltildi.**

### 4e. Regex tabanlı toplu düzenlemeler bazen kayıtları atladı/bozdu
Birden fazla kez, regex ile JS dosyası düzenlerken bazı kayıtlar (33-50 arası) atlandı çünkü regex pattern'i tüm varyasyonları yakalamıyordu. **DERS: Büyük toplu güncellemelerde regex yerine gerçek JS/Node ile diziyi `eval` edip programatik olarak işleyip yeniden serileştir — çok daha güvenilir.**

### 4f. Yanlış dosyayı uygulama
Bir noktada, kullanıcının onaylamadığı **riskli/doğrulanmamış 50 isim eşleştirmesi** yanlışlıkla canlıya uygulanmak üzereyken fark edilip geri alındı. **DERS: Hangi dosyanın "onaylı/güvenli" hangisinin "taslak/riskli" olduğunu KARIŞTIRMA, her uygulamadan önce dosya adını ve içeriğini tekrar kontrol et.**

## 5. VERİ YAPISI

Canlı katalog verisi `_astro/products.DUVDXmdd*.js` içinde (dosya adı her değişiklikte artıyor, en güncelini `grep -rl "products.DUVDXmdd" products/index.html` ile bul). Her obje şu alanları içerebilir:
```
id, slug, name, category, subcategory, images[], source, description,
descriptionSource, creator, period, envanterNo, excelNo, medium,
dimensions, curatorialText, shortDesc
```
483 obje var. Kaynak: Prof. Dr. Fuat Sezgin'in 5 ciltlik "İslam'da Bilim ve Teknik" eseri + "Altın Çağın Teknolojik Mirası" kataloğu (Global Science Advancement, 2019) + fiziksel Excel envanteri.

Kullanıcı, künye/eşleştirme verilerini **kendi hazırladığı JSON dosyaları** halinde parça parça gönderiyor (bazen `sonX.json` — 200-500 kayıtlık büyük partiler, bazen `tümbilgiler` gibi çok daha zengin biyografi içeren dosyalar — `yapimciDetay` alanı: tam ad, Latin ad, doğum-ölüm, bölge, alan, hayat/çalışmaları, önemli eserleri, geleneğe katkısı). **Son istediği yöntem: 5'erli küçük gruplar halinde gönderip her birinin TEK TEK, dikkatlice işlenmesi** — toplu/otomatik işlem istemiyor artık.

## 6. AÇIK İŞLER

1. **Kart tasarımı + sayfa tasarımı örneği** — kullanıcı bunu istedi, teslim edilmedi. Yeni oturum bunu **ayrı bir test sayfasında** (canlı veriye dokunmadan) yapmalı, onay almadan hiçbir gerçek sayfaya uygulamamalı.
2. `tümbilgiler1.json` içindeki 5 kayıt (a-001 dahil) — zengin `yapimciDetay` biyografi verisiyle künye sayfası tasarımı bekliyor.
3. Kalan yüzlerce obje, kullanıcının 5'erli gruplar halinde göndereceği veriyle işlenmeyi bekliyor.
4. Site hâlâ **kök adres "yapım aşamasında"** durumda (`hetega.com/`), gerçek çalışma `/calisma-tam/` üzerinden takip ediliyor. Kullanıcı "her şey bitince yayına alacağız" dedi — siz söylemeden yayına ALMA.
5. `.manifesto`, `.hero`, `.site-header` gibi birkaç CSS kuralı defalarca üst üste tanımlanmış (responsive breakpoint'ler + patch'ler) — işlevsel ama temiz değil, zamanla gerçek bir refactor gerekebilir.

## 7. GENEL DAVRANIŞ KURALLARI (kullanıcının doğrudan talebi)

- **"Yaptım" demeden önce GERÇEKTEN test et.** Sadece dosyada var olması yetmez, mümkünse canlıda doğrula, doğrulayamıyorsan bunu açıkça söyle.
- **Deneme/örnek işleri canlı sayfanın üzerine yazma — ayrı bir test yoluna yaz.**
- **Kullanıcı ne isterse eksiksiz teslim et** — bir parçasını atlayıp "yaptım" deme.
- **Toplu/otomatik eşleştirme mantıklarına güvenme** — özellikle isim benzerliğine dayalı olanlar tehlikeli.
- **Her veri güncellemesinden sonra ilgili TÜM statik sayfaları yeniden üret** (Envanter + 22 kategori sayfası + etkilenen ürün detay sayfaları).
- **Cache-busting'i asla atlama.**
- Kullanıcı Türkçe konuşuyor, doğrudan ve sabırsız — gereksiz uzun açıklama yerine **iş yap, sonucu göster.**
