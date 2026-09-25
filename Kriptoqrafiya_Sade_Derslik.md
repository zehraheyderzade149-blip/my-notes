# KRİPTOQRAFİYA VƏ ŞƏBƏKƏ TƏHLÜKƏSİZLİYİ — SADƏ VƏ AYDIN DƏRSLİK

> ⚠️ **Etik xəbərdarlıq:** Bu sənəddəki bütün alət və hücum üsulları YALNIZ öz laboratoriya mühitində və ya sənə yazılı icazə verilmiş sistemlərdə istifadə edilməlidir. İcazəsiz sistemə hücum Azərbaycan Cinayət Məcəlləsinin 272-ci maddəsinə görə cinayətdir.

**Bu sənəd nədir?** Əvvəlki versiya çox qəliz, dağınıq və bəzi yerlərdə hətta qırıq (natamam cümlə/başlıq) idi. Burada eyni məzmun saxlanılıb, amma hər mövzu əvvəlcə **sadə dillə, real həyat analogiyası ilə** izah olunur, sonra düstur/kod gəlir. Elmi dəqiqlik itirilməyib — sadəcə "əvvəl anla, sonra əzbərlə" məntiqi ilə yazılıb.

---

# HİSSƏ I — TƏMƏL ANLAYIŞLAR VƏ KLASSİK ŞİFRƏLƏR

## 1. Kriptoqrafiyanın əsas qaydaları

### 1.1. Şifrə əslində nədir?

Fikirləş ki, şifrə üç hissədən ibarət bir "qutu"dur:

- **Gen** — açar (parol kimi bir şey) yaradan hissə.
- **Enc** — açıq mesajı (`m`) açarla (`k`) şifrəli mesaja (`c`) çevirən hissə.
- **Dec** — şifrəli mesajı geri açıq mesaja çevirən hissə.

Tək şərt budur: kim düzgün açarla `Dec(k, Enc(k, m))` etsə, mütləq orijinal `m`-i almalıdır. Yəni qıfılı hansı açarla bağlamısansa, elə həmin açarla açılmalıdır.

**Kerckhoffs prinsipi (1883):** Fərz et ki, hücumçu sənin istifadə etdiyin ALQORİTMİ tam bilir — kitabdan oxuyub. Onun bilmədiyi TƏKCƏ AÇARDIR. Əgər şifrən yalnız "heç kim üsulu bilmir" deyə təhlükəsizdirsə, bu real təhlükəsizlik deyil — bu sadəcə **gizlətmə (obfuscation)** oyunudur. Məsələn WhatsApp-ın şifrələmə alqoritmi (Signal Protocol) hamıya açıqdır, amma sənin şəxsi açarını heç kim bilmir — buna görə təhlükəsizdir.

### 1.2. Shannon-un iki qızıl qaydası (1949)

Claude Shannon deyib ki, yaxşı şifrə iki şeyi etməlidir:

1. **Difuziya** — açıq mətndə BİR hərfi dəyişsən, şifrəli mətnin demək olar YARISI dəyişməlidir. Analogiya: gölə bir daş atırsan, dalğalar bütün gölə yayılır — bir nöqtədə qalmır. AES-də bunu **MixColumns** əməliyyatı edir.
2. **Konfüzyon** — açarla şifrəli mətn arasındakı əlaqə o qədər qarışıq olmalıdır ki, heç kim "açarın filan biti dəyişəndə şifrənin filan yeri belə dəyişir" deyə qanunauyğunluq tapa bilməsin. AES-də bunu **SubBytes (S-box)** edir.

### 1.3. Hücumçu nə qədər bilir? (Hücum modelləri)

| Model | Hücumçunun əlində nə var | Real həyatda nə qədər mümkündür |
|---|---|---|
| Yalnız-şifrəli-mətn | Sadəcə şifrəli mesajları görür | Ən zəif hücumçu, amma şifrə buna belə davam gətirməlidir |
| Bilinən-açıq-mətn | Bəzi mesajların həm açıq, həm şifrəli formasını bilir | Realdır — email başlıqları həmişə eynidir, məsələn "Salam" |
| Seçilmiş-açıq-mətn | İstədiyi mesajı sistemə şifrələtdirə bilir | Açıq açarlı sistemlərdə normal haldır (hamı açıq açarla şifrələyə bilər) |
| Seçilmiş-şifrəli-mətn | İstədiyi şifrəli mesajı sistemə deşifrələtdirə bilir | Padding oracle kimi hücumların əsasıdır |

**Qısası:** şifrə dizayn edərkən ən pis ssenarini düşün — hücumçunun əlində nə qədər çox məlumat ola bilər, onu fərz et.

---

## 2. Sezar şifrəsi — ən sadə nümunə

### 2.1. Necə işləyir?

Fikirləş ki, əlifbanı bir dairə kimi düzürsən və hər hərfi neçə addım "sağa" sürüşdürürsən. Bu addım sayı = açar (`k`).

`E(x) = (x + k) mod 26` — şifrələmə
`D(y) = (y − k) mod 26` — deşifrələmə

(x hərfin əlifbadakı nömrəsidir: A=0, B=1, ... Z=25)

### 2.2. Əl ilə misal

```
Açıq:   S   A   L   A   M
Nömrə: 18   0  11   0  12
+k=3:  21   3  14   3  15
Şifrə:  V   D   O   D   P
```

### 2.3. Niyə bu şifrə heç nəyə yaramır?

Cəmi **25 fərqli açar** var (26-nı çıxırıq, çünki k=0 heç nəyi dəyişmir). Kompüter demə, insan özü də 25 variantı 5 dəqiqəyə yoxlaya bilər. Bundan əlavə, hər dildə bəzi hərflər digərlərindən çox işlədilir (İngiliscədə E, T, A ən çox; Azərbaycancada A, Ə, İ, L, R, N ən çox). Şifrəli mətndə ən çox təkrarlanan hərfi tapıb "bu, ən yayılmış hərfə uyğun gəlir" deyə təxmin etməklə şifrə asanlıqla qırılır — buna **tezlik analizi** deyilir.

### 2.4. Affine şifrəsi — Sezarın bir az təkmilləşmiş forması

`E(x) = (ax + b) mod 26`

Burada iki açar var: `a` və `b`. Amma `a`-nın `gcd(a, 26) = 1` şərtini ödəməsi lazımdır (yəni 26 ilə ortaq böləni olmamalıdır), əks halda deşifrələmə mümkün olmur.

Ümumi açar sayı: 12 (a üçün) × 26 (b üçün) = **312**. Sezardan güclü görünür, amma kompüter üçün 312 variant da göz qırpımında yoxlanılır — hələ də praktik olaraq təhlükəsiz deyil.

---

## 3. Vigenère şifrəsi — 300 il "qırılmaz" sayılan şifrə

### 3.1. Fikir

Sezar şifrəsinin problemi budur: hər hərf HƏMİŞƏ eyni miqdarda sürüşür, ona görə tezlik analizi işləyir. Vigenère-də bir söz (açar söz) götürürsən və hər hərfi FƏRQLİ miqdarda sürüşdürürsən — açar sözün hərfləri təkrarlana-təkrarlana.

```
Açar:   K  E  Y  K  E  Y
Açıq:   A  T  T  A  C  K
Sürüşmə:10  4 24 10  4 24
Şifrə:  K  X  R  K  G  I
```

Bu, əslində eyni anda işləyən BİR NEÇƏ Sezar şifrəsidir. Açar sözün uzunluğuna **period** deyilir.

### 3.2. Niyə çətin qırılır?

Çünki eyni hərf (məsələn "A") mətndə hər yerdə fərqli simvola çevrilə bilər — sürüşmə mövqedən asılı olaraq dəyişir. Sadə tezlik analizi artıq işləmir.

### 3.3. Kasiski üsulu — bu şifrəni necə sındırdılar (1863)

Fikir sadədir: əgər açar söz təkrarlanırsa, mətndə eyni 3+ hərflik hissə bir neçə yerdə eyni şifrəyə düşə bilər (təsadüfən açar sözlə eyni mövqeyə düşəndə). Addımlar:

1. Şifrəli mətndə təkrarlanan 3 hərflik parçaları tap.
2. Bu parçaların bir-birindən neçə hərf aralı olduğunu ölç.
3. Bu məsafələrin **ƏBOB**-unu (ən böyük ortaq bölənini) tap — bu, çox güman ki, açar sözün uzunluğudur (period).
4. İndi mətni "hər m-ci hərf" olaraq bölüb, hər qrupu ayrıca Sezar şifrəsi kimi tezlik analizi ilə qır.

### 3.4. Friedman testi (Index of Coincidence) — periodu riyazi tapmaq

Bu, "mətn nə qədər təsadüfi görünür" sualına rəqəmlə cavab verir:

`IC = Σ fᵢ(fᵢ−1) / (N(N−1))`

- Tam təsadüfi mətndə: IC ≈ 0.0385
- Normal İngilis mətnində: IC ≈ 0.0667

Şifrəli mətni müxtəlif period fərziyələri ilə bölüb hər dəfə IC hesablayırsan — hansı bölgüdə IC "normal dil" ədədinə (0.0667) yaxınlaşırsa, doğru period odur.

---

## 4. Playfair şifrəsi

### 4.1. 5×5 cədvəl qurmaq

Açar söz seçilir, təkrarlanan hərflər atılır, qalan yer əlifbanın qalan hərfləri ilə doldurulur (İngilis versiyasında I və J bir xanaya yığılır). Məsələn açar "KEYWORD":

```
K E Y W O
R D A B C
F G H I L
M N P Q S
T U V X Z
```

### 4.2. Şifrələmə məntiqi

Mətn ikilik cütlərə bölünür (eyni hərf yan-yana gələrsə, arasına X qoyulur: "BALLOON" → BA LX LO ON). Sonra hər cüt üçün 3 qayda var:

1. **Eyni sətirdə** olan cütlər — hər hərf öz sağındakı hərflə əvəzlənir (sona çatanda başa qayıdır).
2. **Eyni sütunda** olan cütlər — hər hərf öz altındakı hərflə əvəzlənir.
3. **Fərqli sətir/sütunda** olan cütlər — düzbucaqlı təsəvvür et, hər hərf öz sətrində qalıb digərinin sütunundakı küncə keçir.

Misal: "HE" cütü — H (3-cü sətir, 3-cü sütun), E (1-ci sətir, 2-ci sütun). Düzbucaqlı qaydası ilə: H → G, E → Y, yəni "GY" alınır.

### 4.3. Zəifliyi

Tək hərf yox, HƏRF CÜTLƏRİNİN statistikası qorunur (TH, ER, ON kimi cütlər İngiliscədə tez-tez rast gəlinir). Bu, tezlik analizini bir az çətinləşdirir, amma yenə də mümkün edir. Nəticə: klassik şifrələr arasında güclüdür, amma bugünkü standartlarla müqayisədə çox zəifdir.

---

## 5. Hill şifrəsi — riyaziyyatla şifrələmə

Fikir: mətni ədədlərə çevirib, bir **matrisə** vurursan.

`C = K·P mod 26` (K — açar matrisi)
`P = K⁻¹·C mod 26` (deşifrələmə üçün əks matris lazımdır)

Əks matris (K⁻¹) yalnız `det(K)`-nin 26 ilə ortaq böləni olmadıqda mövcud olur. Zəifliyi: əgər hücumçu bir neçə (açıq mətn, şifrəli mətn) cütü əldə edərsə, xətti tənliklər sistemi qurub açar matrisini birbaşa hesablaya bilər.

---

## 6. One-Time Pad (OTP) — riyazi olaraq qırılmaz olan yeganə şifrə

`C = P ⊕ K` (XOR əməliyyatı)

Şərtlər (HAMISI eyni vaxtda ödənməlidir):

- Açar tam təsadüfi olmalıdır.
- Açar ən azı mesaj qədər uzun olmalıdır.
- Açar YALNIZ BİR DƏFƏ istifadə olunmalıdır.

Bu şərtlər ödənəndə Shannon riyazi olaraq sübut edib ki, şifrəli mətn hücumçuya HEÇ BİR məlumat vermir — istənilən açıq mətn, müəyyən açarla, elə həmin şifrəli mətni verə bilər.

**Amma açarı iki dəfə işlətsən, fəlakət olur:** iki şifrəli mətni XOR etsən, açar yoxa çıxır və qalan `P₁ ⊕ P₂` sadəcə iki açıq mətnin XOR-udur — bu, tezlik analizi ilə açıla bilər. (Sovet kəşfiyyatının Venona şifrələri məhz bu səhvə görə qırılıb.)

**Praktik problem:** mesaj qədər uzun, təkrarsız açarı necə hər iki tərəfə TƏHLÜKƏSİZ çatdırasan? Bu problemə görə real həyatda OTP əvəzinə **axın şifrələri** (stream cipher) istifadə olunur — onlar tam təsadüfi deyil, amma kriptoqrafik cəhətdən təsadüfi GÖRÜNƏN açar ardıcıllığı yaradır.

---

## 7. Şifrələrin dörd əsas ailəsi

| Ailə | Əsas hərəkət | Nümunə | Güclü tərəfi | Zəif tərəfi |
|---|---|---|---|---|
| Yerdəyişmə | Hərflərin yerini dəyişir | Sütun şifrəsi | Sadə | Tezlik analizinə açıqdır |
| Əvəzetmə | Hər hərfi başqası ilə əvəzləyir | Sezar, Playfair | Sürətli | Dilin statistikasına həssasdır |
| XOR (qamma) | Açıq mətni açarla XOR-layır | OTP | OTP-də tam qırılmaz | Açarın paylanması çətindir |
| Blok şifrələri | Bloklar üzərində mürəkkəb çevrilmə | AES, DES | Standart, sürətli | Təhlükəsizlik seçilən iş rejimindən asılıdır |

---

# HİSSƏ II — SİMMETRİK (GİZLİ AÇARLI) ŞİFRƏLƏR

Simmetrik şifrələrdə şifrələmə və deşifrələmə ÜÇÜN EYNİ AÇAR işlədilir — sanki eyni açarla həm qıfılı bağlayır, həm açırsan.

## 8. DES — ilk rəsmi standart (1976)

### 8.1. Qısa tarixi

IBM-in "Lucifer" adlı şifrəsi əsasında hazırlanıb, 1976-cı ildə ABŞ-ın rəsmi standartı elan olunub. 56-bit açar, 64-bit blok, 16 dövrlük **Feistel şəbəkəsi** işlədir.

### 8.2. Feistel şəbəkəsi nədir? (DES-in skeleti)

Bloku iki yarıya bölürsən: sol (L) və sağ (R). Hər dövrdə:

```
L(yeni) = R(köhnə)
R(yeni) = L(köhnə) ⊕ F(R(köhnə), açar)
```

Feistel şəbəkəsinin ən gözəl tərəfi budur: deşifrələmə eyni sxemlə aparılır, sadəcə açarlar TƏRS SIRAYLA verilir. Ayrıca "deşifrə alqoritmi" yazmağa ehtiyac yoxdur.

`F` funksiyası daxilində baş verənlər:
1. **Genişləndirmə (Expansion):** 32 bit → 48 bitə "şişirdilir" (bəzi bitlər təkrarlanır).
2. Açarla **XOR**-lanır.
3. **S-box-lar** — 8 ədəd 6-bit girişi 4-bit çıxışa çevirən qeyri-xətti cədvəl. Bu, DES-in "ürəyi" sayılır, çünki qeyri-xəttilik olmasa şifrə xətti tənliklərlə asanlıqla açılardı.
4. **Permutasiya (P):** bitlər yenidən qarışdırılır.

### 8.3. Niyə DES artıq işlənmir?

56-bit açar deməkdir 2⁵⁶ ≈ 72 kvadrilyon variant. Qulaqlıq kimi az görünür, amma:

- 1998-ci ildə EFF-in "Deep Crack" adlı xüsusi maşını (təxminən 250 min dollar dəyərində) DES-i cəmi **56 saata** qırdı.
- Nəticədə NIST 2001-ci ildə tam yeni standart (AES) elan etdi.

## 9. 3DES — DES-i "üç dəfə işlətmək" həlli

`C = E_K3(D_K2(E_K1(P)))`

DES-i üç dəfə art-arda (şifrələ-deşifrələ-şifrələ) tətbiq edərək effektiv açar uzunluğunu artırırdılar (112–168 bit). Amma blok ölçüsü hələ də 64 bit qalır — bu, **SWEET32** adlı hücuma açıq qapı saxlayır (bax bölmə 20). Ona görə 2017-dən NIST artıq 3DES-i tövsiyə etmir.

## 10. AES — bu günün standartı

### 10.1. Necə seçilib?

1997-ci ildə NIST açıq müsabiqə elan etdi, 15 namizəd arasından Belçikalı iki kriptoqrafın (Daemen və Rijmen) təklif etdiyi "Rijndael" 2000-ci ildə qalib gəldi və AES adlandırıldı.

### 10.2. Əsas parametrlər

- Blok ölçüsü həmişə 128 bit (4×4 bayt matris kimi düşünülür — buna **State** deyilir).
- Açar ölçüsündən asılı olaraq dövr sayı: 128-bit açar → 10 dövr, 192-bit → 12, 256-bit → 14.

### 10.3. Hər dövrdə baş verən 4 addım

1. **SubBytes** — hər bayt xüsusi cədvəl (S-box) ilə əvəzlənir. Bu, konfüzyonu yaradır.
2. **ShiftRows** — matrisdəki sətirlər sola sürüşdürülür (2-ci sətir 1 addım, 3-cü 2 addım, 4-cü 3 addım).
3. **MixColumns** — hər sütun riyazi olaraq qarışdırılır. Bu, difuziyanı yaradır (bir baytın dəyişməsi bütün sütuna yayılır).
4. **AddRoundKey** — açarla sadə XOR.

(Son dövrdə MixColumns adımı atlanılır.)

### 10.4. AES niyə hələ də qırılmayıb?

Ən yaxşı bilinən nəzəri hücum (biclique adlanır) AES-128-i 2¹²⁶.1 əməliyyata endirir — bu, tam brute-force-dan (2¹²⁸) cəmi 4 qat sürətlidir, yəni PRAKTİKİ HEÇ BİR FƏRQ YARATMIR. Bugünkü hücumlar artıq AES alqoritminin özünə yox, onun PROQRAM TƏMİNATINDA necə YAZILDIĞINA (vaxt ölçmə, enerji istehlakı kimi yan-kanal hücumları) yönəlib.

### 10.5. "Pinqvin problemi" — niyə rejim seçimi vacibdir?

Eyni AES alqoritmi, fərqli "iş rejimi" ilə tamam fərqli təhlükəsizlik verə bilər. Ən məşhur nümunə: şəkli ECB rejimi ilə şifrələsən, eyni rəngli piksel blokları eyni şifrəli blok verir — nəticədə şəklin konturu (məsələn pinqvin siluetı) şifrəli halda belə görünür qalır! CBC, CTR və GCM rejimlərində bu problem yoxdur.

## 11. Blok şifrələrin iş rejimləri

Bir şifrə alqoritmi (məsələn AES) tək bir 128-bit bloku şifrələyə bilir. Amma real mesajlar çox bloklu olur — bu blokları necə birləşdirəcəyini "iş rejimi" müəyyən edir.

### 11.1. ECB (Electronic Codebook) — işlətmə!

Hər blok tamamilə müstəqil şifrələnir: `Cᵢ = E(K, Pᵢ)`. Sadədir, amma structuru gizlətmir — yuxarıdakı "pinqvin problemi" məhz bundan qaynaqlanır.

### 11.2. CBC (Cipher Block Chaining)

Hər blok, özündən əvvəlki şifrəli blokla XOR-lanaraq şifrələnir: `Cᵢ = E(K, Pᵢ ⊕ Cᵢ₋₁)`. İlk blok üçün təsadüfi bir başlanğıc dəyər (IV) işlədilir.

- IV təsadüfi olmalı və hər dəfə fərqli olmalıdır (amma gizli saxlanmasına ehtiyac yoxdur).
- Zəifliyi: "padding oracle" adlı hücumlara açıqdır (POODLE bunun bir növüdür, bax bölmə 20).

### 11.3. CTR (Counter)

Bu rejim AES-i əslində bir axın şifrəsi kimi işlədir: sadəcə sayğac (counter) dəyərini şifrələyib, nəticəni açıq mətnlə XOR-layırsan: `Cᵢ = Pᵢ ⊕ E(K, sayğacᵢ)`.

- Şifrələmə VƏ deşifrələmə paralel edilə bilər (CBC-də mümkün deyildi).
- Kritik qayda: **eyni sayğac/nonce dəyəri iki dəfə İSTİFADƏ OLUNMAMALIDIR** — əks halda hücumçu iki şifrəli mesajı XOR-layıb məzmunu bərpa edə bilər.

### 11.4. GCM — bugünkü qızıl standart

`AES-GCM = CTR rejimi + GHASH (bütövlük yoxlaması)`. Yəni GCM eyni anda HƏM məxfiliyi (heç kim oxuya bilməsin), HƏM bütövlüyü (heç kim dəyişə bilməsin) təmin edir. Bu birləşməyə **AEAD** (Authenticated Encryption with Associated Data) deyilir və bugün TLS-də əsas seçimdir.

### 11.5. Padding — blokun sonunu doldurmaq

Mesajın son bloku tam dolmadıqda, boş yerə nə yazılacağını müəyyən edən qayda lazımdır. Ən çox işlədilən: **PKCS#7** — əgər n bayt çatışmırsa, o boşluğa n dəyərini n dəfə yazırsan (məsələn 3 bayt çatmırsa: 03 03 03). Düzgün doldurulmamış padding görəndə sistemin necə cavab verdiyi hücumçuya məlumat verə bilər (padding oracle).

## 12. Axın şifrələri

Blok şifrələr mesajı bloklara bölür, axın şifrələri isə bayt-bayt (və ya bit-bit) davamlı bir "açar axını" yaradıb açıq mətnlə XOR-layır.

### 12.1. RC4 — niyə artıq işlədilmir?

RC4 iki mərhələdən ibarətdir:

- **KSA (açarı cədvələ qarışdırmaq):** 256 elementlik cədvəli açarla qarışdırır.
- **PRGA (açar axını yaratmaq):** cədvəldən davamlı olaraq bayt çıxarır.

**Ölümcül zəiflikləri:**
1. WEP protokolunda açarla 24-bit IV birləşdirilirdi — bu, 2001-ci ildə Fluhrer-Mantin-Shamir hücumu ilə tam sındırıldı.
2. RC4-ün ürətdiyi ilk baytlarda statistik "meyillər" var — tam təsadüfi deyil.
3. 2015-ci ildə TLS-də RC4 istifadəsi rəsmən qadağan edildi (RFC 7465).

### 12.2. ChaCha20 — RC4-ün müasir, təhlükəsiz alternativi

Daniel J. Bernstein tərəfindən hazırlanıb, 2014-cü ildə Google TLS üçün təklif etdi. Cədvəl əvəzinə sadəcə toplama, dövr etdirmə (rotate) və XOR əməliyyatlarından ibarətdir (buna **ARX** deyilir) — cədvəl olmadığı üçün "cache-timing" adlı yan-kanal hücumlarına qarşı təbii şəkildə davamlıdır.

- 256-bit açar, 96-bit nonce, 20 dövr.
- ARM prosessorlu telefonlarda (AES-ə xüsusi sürətləndirici çip olmayan yerlərdə) AES-dən daha SÜRƏTLİdir.

### 12.3. ChaCha20-Poly1305 — AEAD kombinasiyası

Poly1305 bütövlük təsdiqi (MAC) yaradır. Bu ikisi birləşərək TLS-də `TLS_CHACHA20_POLY1305_SHA256` şifr dəstini formalaşdırır — WireGuard VPN də bunu əsas kimi işlədir.

### 12.4. Blok vs Axın — qısa müqayisə

| Meyar | Blok (AES) | Axın (ChaCha20) |
|---|---|---|
| İşlədiyi vahid | 128-bitlik bloklar | Bayt-bayt |
| Sürət (aparat dəstəyi ilə) | Ən sürətli (AES-NI çipi ilə) | Orta |
| Sürət (yalnız proqram) | Orta | Çox sürətli |
| Tipik istifadə yeri | Disk şifrələmə, server-server TLS | Mobil TLS, WireGuard |


---

# HİSSƏ III — ASİMMETRİK (AÇIQ AÇARLI) KRİPTOQRAFİYA

Simmetrik şifrələrdə problem budur: iki nəfər əvvəlcədən GİZLİ bir açarı NECƏ paylaşsın, halbuki kanal açıqdır? Asimmetrik kriptoqrafiya bu problemi həll edir: hər kəsin İKİ açarı olur — biri AÇIQ (hamı bilə bilər), biri GİZLİ (yalnız sahibində qalır).

## 13. Lazım olan riyaziyyat (RSA-nı anlamaq üçün)

### 13.1. Modul hesabı — sadəcə "saat hesabıdır"

`a ≡ b (mod n)` o deməkdir ki, a və b-ni n-ə böləndə qalıq eynidir. Analogiya: saat 14:00-a 5 saat əlavə etsən 19:00 olur, amma 12 saatlıq saatda bu "7"-yə bərabərdir (19 mod 12 = 7).

### 13.2. Bir neçə anlayış

- **Qarşılıqlı sadəlik:** iki ədədin ortaq böləni 1-dən başqa yoxdursa.
- **Euler φ funksiyası:** n-dən kiçik, n ilə qarşılıqlı sadə olan ədədlərin sayı. Əgər p sadə ədəddirsə: φ(p) = p−1. Əgər n iki sadə ədədin hasilidirsə (n=p·q): φ(n) = (p−1)(q−1).
- **Euler teoremi:** əgər gcd(a,n)=1 olarsa, `a^φ(n) ≡ 1 (mod n)`. RSA-nın bütün riyaziyyatı bu teoremə əsaslanır.

### 13.3. Genişləndirilmiş Evklid alqoritmi — "əks ədəd" tapmaq

Adi bölmədə əksi rahat tapırıq (məsələn 1/5). Amma modul dünyasında "əks ədəd" fərqli hesablanır. Genişləndirilmiş Evklid alqoritmi bunu tapmağa kömək edir. Misal: 7-nin mod 26-da əksini tapaq:

```
26 = 3·7 + 5
7  = 1·5 + 2
5  = 2·2 + 1
Geriyə gedərək: 1 = 3·26 − 11·7  →  7⁻¹ ≡ −11 ≡ 15 (mod 26)
Yoxlama: 7 × 15 = 105 = 4×26 + 1 ✅
```

### 13.4. Sürətli qüvvətə yüksəltmə (square-and-multiply)

RSA-də `aᵉ mod n` kimi böyük qüvvətlər hesablanır. Adi üsulla e dəfə vurmaq çox yavaş olardı. Bunun əvəzinə e-nin ikilik təsvirinə əsasən "kvadrata al, lazım gələndə vur" üsulu işlədilir — bu, əməliyyat sayını yüzlərlə dəfə azaldır.

## 14. RSA — ən məşhur açıq açarlı şifrə

### 14.1. Açar necə yaradılır?

1. İki böyük sadə ədəd seç: `p`, `q` (real sistemlərdə hər biri ≥1024 bit).
2. `n = p·q` — bu, "modul" adlanır və şifrələmənin əsasıdır.
3. `φ(n) = (p−1)(q−1)`.
4. `e` seç (adətən 65537) elə ki, gcd(e, φ(n)) = 1.
5. `d = e⁻¹ mod φ(n)` — genişləndirilmiş Evklid ilə tapılır.

**Açıq açar:** (n, e) — hər kəsə göndərilə bilər.
**Gizli açar:** (n, d) — heç kimə deyilmir.

### 14.2. Şifrələmə/deşifrələmə düsturu

`c = mᵉ mod n` (hər kəs açıq açarla şifrələyə bilər)
`m = cᵈ mod n` (yalnız gizli açarın sahibi deşifrələyə bilər)

**Kiçik rəqəmlərlə misal:** p=5, q=11 → n=55, φ=40. e=3 seçildikdə d=27 alınır (3×27=81=2×40+1). Mesaj m=7-ni şifrələyək: c = 7³ mod 55 = 343 mod 55 = **13**. Geri deşifrə etsək: 13²⁷ mod 55 = **7** ✅ (yenidən m-i alırıq).

### 14.3. Bu niyə işləyir?

Çünki `e` və `d` elə seçilib ki, `m^(ed) ≡ m (mod n)` — bu, birbaşa Euler teoreminin nəticəsidir.

### 14.4. Niyə "sadə RSA" (padding-siz) təhlükəlidir?

1. **Determinizm:** eyni mesaj həmişə eyni şifrəli mətn verir — hücumçu "bəlkə bu, hə/yox mesajıdır" deyə cavabları qabaqcadan hesablayıb müqayisə edə bilər.
2. **Malleability (dəyişdirilə bilmə):** şifrəli mətni riyazi olaraq dəyişib, deşifrə olunanda nəticəni "proqnozlaşdırıla bilən şəkildə" dəyişdirmək mümkündür.
3. Kiçik mesaj və kiçik `e` (məsələn 3) seçiləndə, riyazi qüvvətləndirmə modulu keçmir, sadəcə kub kök almaqla açıla bilir.

**Həll:** **OAEP** adlı doldurma (padding) sxemi — mesajı şifrələmədən əvvəl təsadüfi məlumatla qarışdırır ki, yuxarıdakı problemlər aradan qalxsın.

### 14.5. RSA necə "sındırılır"?

- **Tam faktorizasiya:** n = p·q ədədini yenidən p və q-ya bölmək. Kiçik açarlar (RSA-768) 2009-cu ildə yüzlərlə kompüterlə 2 ilə sındırılıb. RSA-2048 hələ heç kimə uzun-uzadı çıxmır.
- **Zəif təsadüfi ədəd generasiyası:** bəzi cihazlar p və q seçərkən yetərincə təsadüfi olmayan üsullar işlədib — 2012-ci ildə tədqiqatçılar milyonlarla real açarı yoxlayıb, bir hissəsinin ortaq faktor paylaşdığını aşkarlayıb.
- **Vaxt ölçmə hücumu (timing attack):** şifrələmə/deşifrələmə nə qədər vaxt aparır ölçülərək açar bərpa edilə bilər.

### 14.6. Diffie-Hellman — açar razılaşması

**Problem:** iki nəfər açıq (dinlənilən) kanaldan danışaraq, üçüncü şəxsin bilmədiyi ortaq bir gizli ədəd üzərində necə razılaşa bilər?

**Həlli:**

- Hamı ictimai `p` (sadə ədəd) və `g` (generator) razılaşdırır.
- Alice təsadüfi `a` seçir, `A = gᵃ mod p` göndərir.
- Bob təsadüfi `b` seçir, `B = gᵇ mod p` göndərir.
- Hər ikisi eyni ortaq açarı hesablaya bilir: `K = Bᵃ mod p = Aᵇ mod p = g^(ab) mod p`.

Hücumçu `p`, `g`, `A`, `B`-ni görür, amma `a` ya da `b`-ni tapmaq **diskret loqarifm problemi** adlanır və böyük ədədlərlə praktik olaraq həll edilməzdir.

## 15. Elliptik əyri kriptoqrafiyası (ECC)

### 15.1. Fikir

Adi RSA çox böyük ədədlərlə işləyir (2048+ bit). Elliptik əyri riyaziyyatı isə eyni təhlükəsizlik səviyyəsinə ÇOX QISA açarlarla nail olur, çünki üzərində qurulan riyazi problem (ECDLP) daha çətindir.

`y² = x³ + ax + b (mod p)` — bu tənliyi ödəyən nöqtələr üzərində "toplama" əməliyyatı təyin olunur (iki nöqtəni birləşdirən xətt əyrini üçüncü nöqtədə kəsir, onun simmetriyini götürürsən).

### 15.2. Niyə RSA-dan üstündür?

| Təhlükəsizlik səviyyəsi | RSA açar uzunluğu | ECC açar uzunluğu |
|---|---|---|
| 128 bit | 3072 bit | 256 bit |
| 256 bit | 15360 bit | 512 bit |

Daha qısa açar = daha az yaddaş, daha az hesablama, daha sürətli TLS bağlantısı. Buna görə mobil cihazlar və müasir vebsaytlar ECC-yə üstünlük verir.

### 15.3. Forward Secrecy (irəli məxfilik)

TLS 1.3-də hər sessiya üçün müvəqqəti (efemer) Diffie-Hellman açarları yaradılır (ECDHE). Bunun mənası: sabah serverin daimi gizli açarı oğurlansa belə, DÜNƏNKİ trafiki heç kim geri deşifrə edə bilməz, çünki o sessiyanın açarı artıq unudulub. TLS 1.3-də sadə RSA şifrələmə tamamilə ləğv olunub, yalnız bu üsul qalıb.


---

# HİSSƏ IV — HASH FUNKSİYALARI, MAC VƏ PAROL TƏHLÜKƏSİZLİYİ

## 16. Hash funksiyaları

Hash funksiyası istənilən ölçüdə girişi götürüb SABİT ölçüdə çıxış (barmaq izi) verir. Üç şərti ödəməlidir:

1. **Preimage müqaviməti:** çıxışı (h(x)) görüb girişi (x) tapmaq praktik olaraq mümkün olmamalıdır.
2. **İkinci-preimage müqaviməti:** verilmiş x-ə uyğun eyni hash-i verən BAŞQA bir x' tapmaq çətin olmalıdır.
3. **Toqquşma müqaviməti:** İSTƏNİLƏN iki fərqli giriş tapıb, onların eyni hash verməsini təmin etmək çətin olmalıdır (bu ən zəif tələbdir — "ad günü paradoksu" səbəbindən nəzəriyyədə gözlənilən çətinlik yalnız 2^(n/2)-dir, tam 2ⁿ deyil).

> **Ad günü paradoksu:** otaqda 23 nəfər olsa, ikisinin ad günü eyni gündə düşmə ehtimalı 50%-dən çoxdur — çünki cütlərin sayı çox sürətlə artır. Eyni məntiqlə, n-bit hash-də toqquşma tapmaq üçün orta hesabla 2^(n/2) cəhd kifayətdir, 2ⁿ yox.

### 16.1. MD5 — köhnəlmiş, artıq işlədilməməlidir

1992-ci ildə yaradılıb, 128-bit çıxış verir. 2004-cü ildə tədqiqatçılar real toqquşma nümunələri tapdılar. 2012-ci ildə "Flame" adlı kiber-silah, saxta Microsoft sertifikatı yaratmaq üçün MD5-in bu zəifliyindən istifadə etdi. Bugün MD5 YALNIZ təsadüfi xəta yoxlaması (checksum) üçün işlədilə bilər — TƏHLÜKƏSİZLİK məqsədilə İSTİFADƏSİ QADAĞANDIR.

### 16.2. SHA-1 — MD5-dən yaxşı, amma o da köhnəlib

160-bit çıxış verir. 2017-ci ildə Google və CWI institutları "SHAttered" adlı hücumla real toqquşma nümunəsi göstərdilər. Bugün artıq TLS sertifikatlarında qadağandır.

### 16.3. SHA-2 ailəsi — bugünkü əsas seçim

SHA-256 (32-bit sözlərlə işləyir, 64 dövr, 256-bit çıxış) hazırda ən geniş yayılmış, praktik olaraq sındırılmamış hash funksiyasıdır.

### 16.4. SHA-3 — ehtiyat plan

SHA-2-dən tamamilə fərqli daxili quruluşa (sponge konstruksiyası) malikdir. Məqsədi: əgər gələcəkdə SHA-2 ailəsində zəiflik tapılsa, tamam fərqli riyaziyyata əsaslanan bir alternativ hazır olsun.

### 16.5. HMAC — hash-dən "imza" düzəltmək

Sadəcə `H(açar || mesaj)` hesablamaq kifayət deyil, çünki bəzi hash konstruksiyaları "uzantı hücumu"na açıqdır (kimsə mesajın sonuna əlavə edib hash-i yeniləyə bilir, açarı bilmədən). HMAC bunu iki qatlı hash ilə həll edir:

`HMAC(K, m) = H( (K⊕opad) || H( (K⊕ipad) || m ) )`

Nəticə: göndərilən mesajın HƏM DƏYİŞMƏDİYİNİ, HƏM DƏ doğru açarın sahibindən gəldiyini yoxlaya bilirsən. API imza sistemlərində geniş işlədilir.

## 17. Parol təhlükəsizliyi

### 17.1. Niyə "sadəcə hash-ləmək" kifayət etmir?

Adi insan parolu təxminən 40 bitdən az entropiyaya (təsadüfilik dərəcəsinə) malikdir. Güclü bir qrafik kart (GPU) MD5-i saniyədə ~150 milyard dəfə hesablaya bilir. Bu o deməkdir ki, 8 simvollu (hərf+rəqəm) bir parolu MD5 ilə hash-ləmisənsə, GPU onu təxminən **25 dəqiqəyə** tapa bilər.

### 17.2. Rainbow table — qabaqcadan hazırlanmış "kod kitabı"

Hücumçular milyonlarla parol üçün hash dəyərlərini əvvəlcədən hesablayıb sıxılmış cədvəl şəklində saxlayır. Sənin hash-ini görəndə birbaşa cədvəldən axtarırlar.

**Salt həlli:** hər parola unikal, təsadüfi bir dəyər (salt) əlavə edib sonra hash-ləyirsən: `h = H(parol || salt)`. Salt gizli saxlanmır (hər kəs görə bilər) — məqsədi cədvəlin işə yaramamasını təmin etməkdir, çünki indi hər parol üçün ayrıca cədvəl lazım olardı.

### 17.3. Parol üçün xüsusi hazırlanmış "yavaş" alqoritmlər

Adi SHA-256 ÇOX SÜRƏTLİDİR — bu, parol saxlamaq üçün əslində PİS xüsusiyyətdir, çünki hücumçu da sürətlə cəhd edə bilir. Ona görə xüsusi, QƏSDƏN yavaş alqoritmlər işlədilir:

| Alqoritm | Əsas fikir | Niyə güclüdür |
|---|---|---|
| PBKDF2 | Hash-i minlərlə dəfə təkrar hesablayır | Hər cəhd bahalaşır |
| bcrypt | Xüsusi ləng şifrələmə əsasında | Tənzimlənə bilən "cost" parametri |
| scrypt | Həm CPU, həm YADDAŞ tələb edir | GPU-ların üstünlüyünü azaldır |
| **Argon2id** (2015 qalibi) | Yaddaş + zaman + paralellik birlikdə | Bugünkü ən güclü seçim |

**Tövsiyə edilən Argon2id parametrləri:** 64MB yaddaş, 3 təkrar, 4 paralel proses.

### 17.4. Müdafiə üçün praktiki tövsiyə

14+ simvollu, mənalı cümlə tərzində parol (passphrase) işlət, parol meneceri istifadə et, hər yerdə çoxfaktorlu doğrulama (MFA) aktiv et.

## 18. Rəqəmsal imza

### 18.1. RSA ilə imzalama

`imza = H(mesaj)ᵈ mod n` (yalnız gizli açarın sahibi yarada bilər)
`yoxlama: imzaᵉ mod n = H(mesaj)?` (açıq açarla hər kəs yoxlaya bilər)

### 18.2. ECDSA — elliptik əyri üzərində imza

Eyni fikir, amma elliptik əyri riyaziyyatı ilə. **Çox kritik qayda:** imzalayarkən işlədilən təsadüfi `k` ədədi HEÇ VAXT təkrarlanmamalıdır. Əgər eyni `k` iki fərqli mesaj üçün işlədilsə, riyazi olaraq gizli açarı geriyə hesablamaq mümkün olur. Bu səhv 2010-cu ildə PlayStation 3-ün proqram təhlükəsizliyini poza bilib.

**EdDSA (Ed25519):** bu problemi tamamilə aradan qaldırır, çünki `k` təsadüfi seçilmir, mesajdan DETERMİNİST şəkildə hesablanır — sürətli və qısa imza verir.

### 18.3. MAC ilə İmza fərqi

| | MAC (HMAC) | Rəqəmsal imza |
|---|---|---|
| Açar | Hər iki tərəf eyni gizli açarı bilir | Bir açıq, bir gizli açar |
| İnkar edilə bilməzlik | Yoxdur — hər iki tərəf yarada bilər | Var — yalnız gizli açarın sahibi yarada bilər |

## 19. X.509 sertifikatları və PKI (Açar İnfrastrukturu)

### 19.1. Sertifikatda nə var?

Sertifikat, sadə dillə desək, "bu açıq açar filan saytınkıdır" deyən, etibarlı bir tərəf (CA) tərəfindən imzalanmış rəsmi sənəddir. İçində: sahibi (subject), etibar edilən qurum (issuer), etibarlılıq müddəti, açıq açar və CA-nın imzası olur.

### 19.2. Etibar zənciri

Brauzerin quraşdırılmış "kök" sertifikatlarından başlayaraq, hər sertifikat özündən yuxarıdakı tərəfindən imzalandığını yoxlamaqla server sertifikatına qədər gedir. Bu zəncirin nə vaxtsa qırılması (hər hansı bir imzanın etibarsız olması) bütün sistemi şübhəli edir.

### 19.3. Real hadisə: DigiNotar (2011)

Niderland-ın sertifikat qurumu hakerlər tərəfindən sındırılıb, google.com üçün saxta sertifikatlar yaradılmışdı. Nəticədə bütün brauzerlər bu CA-ya etibarı dayandırdı və şirkət müflis oldu — bu, PKI sisteminin nə qədər kövrək ola biləcəyini göstərən klassik nümunədir.


---

# HİSSƏ V — TLS/SSL VƏ ŞƏBƏKƏ HÜCUMLARI

## 20. TLS handshake — sayt açanda arxada nə baş verir?

### 20.1. Sadələşdirilmiş axın (TLS 1.2)

1. **Client:** "Salam, mən bu versiyaları və şifr dəstlərini dəstəkləyirəm" (ClientHello).
2. **Server:** "Yaxşı, bunu seçirik" + öz sertifikatını göndərir (ServerHello + Certificate).
3. İki tərəf Diffie-Hellman (və ya RSA) vasitəsilə ortaq bir sirr (pre-master secret) yaradır.
4. Bu sirrdən "master secret" və oradan da real şifrələmə açarları törədilir.
5. "Finished" mesajları ilə hər iki tərəf handshake-in dəyişdirilmədiyini təsdiqləyir.
6. Bundan sonra bütün trafik AES-GCM (və ya bənzəri) ilə şifrələnir.

### 20.2. TLS 1.3 — nə dəyişdi?

- Köhnə, təhlükəli alqoritmlərin (RC4, 3DES, SHA-1, sadə RSA şifrələmə, CBC) HAMISI silindi.
- Handshake sürətləndirildi (1 gediş-gəliş kifayət edir, əvvəl 2 lazım idi).
- Yalnız 3 şifr dəsti qalıb — sadələşdirilib.
- Forward secrecy MƏCBURİDİR (yalnız efemer DH işlədilir).

### 20.3. Şifr dəstinin adı nə deməkdir?

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` desəm:

- `ECDHE` — açar necə razılaşdırılır (forward secrecy ilə)
- `RSA` — server öz sertifikatını necə imzalayıb
- `AES_128_GCM` — məlumat necə şifrələnir
- `SHA256` — hash funksiyası

## 21. Tarixi TLS zəiflikləri (nümunə üzərindən öyrənmək üçün)

### 21.1. POODLE (2014)

Köhnə SSLv3 protokolunda padding düzgün yoxlanılmır. Hücumçu ictimai wifi-də sənin brauzerini zorla köhnə SSLv3-ə "endirib", sonra bayt-bayt sınaqla sessiya cookie-lərini oğurlaya bilir. **Həlli:** SSLv3-ü tamamilə söndürmək.

### 21.2. SWEET32 (2016)

3DES kimi 64-bitlik bloklu şifrələrdə, çox böyük miqdarda trafik (~32GB) ötürüləndə "ad günü paradoksu" səbəbindən iki blok təsadüfən eyni şifrəli mətni verə bilir. Bu, hücumçuya mətn haqqında məlumat sızdırır. **Həlli:** 3DES-i serverdən silmək.

### 21.3. Digər tanınmış hücumlar (qısa)

- **Heartbleed (2014):** OpenSSL-də proqramlaşdırma xətası — yaddaşdan həssas məlumat (o cümlədən açarlar) sızırdı. Bu, şifrənin özünün deyil, PROQRAM KODUNUN zəifliyi idi.
- **Logjam:** zəif Diffie-Hellman parametrləri ilə əlaqəli hücum.
- **ROBOT:** RSA PKCS#1 v1.5 doldurma sxemində "oracle" zəifliyi.

## 22. IPsec VPN və IKE protokolu

### 22.1. IPsec-in iki üsulu

- **AH (Authentication Header):** yalnız bütövlük və kimlik doğrulama verir, ŞİFRƏLƏMİR.
- **ESP (Encapsulating Security Payload):** həm şifrələyir, həm bütövlüyü təsdiqləyir — bugünkü standart budur.

### 22.2. IKE — açar razılaşdırma protokolu

VPN-lər açar razılaşdırmaq üçün IKE protokolundan istifadə edir (UDP 500 portu). İki əsas rejim var:

**Main Mode (6 mesaj):** kimlik məlumatı YALNIZ Diffie-Hellman açarı qurulduqdan SONRA, artıq şifrələnmiş şəkildə göndərilir. Şəbəkədə dinləyən hücumçu kim olduğunu görmür.

**Aggressive Mode (3 mesaj):** sürəti üçün kimlik məlumatı VƏ doğrulama hash-i AÇIQ şəkildə göndərilir. Bu, ciddi bir zəiflikdir:

> Əgər sistem PSK (paylaşılan parol) ilə işləyirsə, Aggressive Mode-da bu doğrulama hash-i şəbəkədə açıq gedir. Hücumçu bu hash-i tutub, "offline" rejimdə (internetə bağlı olmadan, öz kompüterində) minlərlə parol kandidatını sınaya bilər — server bundan xəbərsiz belə qalır. Main Mode-da bu mümkün DEYİL, çünki həmin məlumat artıq şifrəlidir.

**Tövsiyə:** mümkünsə Aggressive Mode-u tamamilə söndür, IKEv2 istifadə et (bu rejim anlayışı IKEv2-də ümumiyyətlə yoxdur).

## 23. Praktiki alətlər (yalnız öz laboratoriyanda!)

- **sslscan** — bir serverin dəstəklədiyi TLS versiyalarını, şifr dəstlərini və sertifikat detallarını göstərir. Köhnə protokol (SSLv3) və ya zəif şifr (RC4, 3DES) görsən — bu, serverin zəif konfiqurasiya edildiyinin əlamətidir.
- **openssl s_client** — əl ilə TLS bağlantısı sınamaq üçün.
- **Shodan** — internetə bağlı cihazları (server, kamera, VPN cihazı) axtaran axtarış sistemi. Google veb səhifələri indeksləyir, Shodan isə açıq PORT-ları və onların "banner" məlumatlarını indeksləyir.
- **ike-scan** — VPN serverlərinin IKE konfiqurasiyasını yoxlayır, Aggressive Mode açıqsa PSK hash-ini tuta bilər.
- **Wireshark** — şəbəkə trafikini "canlı" izləmək üçün. Şifrələnmiş trafikdə IP ünvanları, portlar və (ilk mərhələdə) sayt adı (SNI) görünür, amma məzmun görünmür.


---

# HİSSƏ VI — PYTHON İLƏ PRAKTİKİ NÜMUNƏLƏR

## 24. Sezar şifrəsini qırmaq (brute-force)

```python
def sezar_desifrele(c, k):
    return "".join(
        chr((ord(ch)-ord('A')-k)%26+ord('A')) if ch.isupper()
        else chr((ord(ch)-ord('a')-k)%26+ord('a')) if ch.islower()
        else ch
        for ch in c
    )

sifre = "WKH HDJOH KDV ODQGHG"   # "the eagle has landed", açar k=3
for k in range(26):
    print(k, sezar_desifrele(sifre, k))
# 26 variantdan yalnız biri normal İngilis cümləsi kimi oxunacaq
```

Məntiq sadədir: açar sayı cəmi 26 olduğu üçün, bütün variantları sınayıb hansının məna verdiyinə baxırıq.

## 25. AES — ECB vs GCM müqayisəsi (kodla)

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
import os

açar = os.urandom(16)
iv = os.urandom(16)

# ECB — TÖVSİYƏ OLUNMUR: eyni blok həmişə eyni şifrə verir
ecb = Cipher(algorithms.AES(açar), modes.ECB()).encryptor()

# GCM — həm şifrələyir, həm bütövlüyü təsdiqləyir (AEAD)
gcm = Cipher(algorithms.AES(açar), modes.GCM(iv)).encryptor()
gcm.authenticate_additional_data(b"header")
şifrəli = gcm.update(b"Gizli melumat") + gcm.finalize()
tag = gcm.tag   # bunu MÜTLƏQ saxlamaq lazımdır — bütövlük yoxlaması üçün
```

## 26. RSA ilə tam dövrə (şifrələmə + imza)

```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes

gizli_açar = rsa.generate_private_key(public_exponent=65537, key_size=2048)
açıq_açar = gizli_açar.public_key()

# Şifrələmə — OAEP padding ilə (təhlükəsiz üsul)
şifrəli = açıq_açar.encrypt(
    b"sirli mesaj",
    padding.OAEP(mgf=padding.MGF1(algorithm=hashes.SHA256()),
                 algorithm=hashes.SHA256(), label=None))

açıq_mətn = gizli_açar.decrypt(
    şifrəli,
    padding.OAEP(mgf=padding.MGF1(hashes.SHA256()),
                 algorithm=hashes.SHA256(), label=None))

# Rəqəmsal imza
imza = gizli_açar.sign(
    b"sənəd",
    padding.PSS(mgf=padding.MGF1(hashes.SHA256()), salt_length=padding.PSS.MAX_LENGTH),
    hashes.SHA256())

açıq_açar.verify(
    imza, b"sənəd",
    padding.PSS(mgf=padding.MGF1(hashes.SHA256()), salt_length=padding.PSS.MAX_LENGTH),
    hashes.SHA256())
```

## 27. Diffie-Hellman simulyasiyası

```python
from cryptography.hazmat.primitives.asymmetric import dh

parametrlər = dh.generate_parameters(generator=2, key_size=2048)

alice_gizli = parametrlər.generate_private_key()
bob_gizli   = parametrlər.generate_private_key()

alice_açıq = alice_gizli.public_key()
bob_açıq   = bob_gizli.public_key()

alice_ortaq = alice_gizli.exchange(bob_açıq)
bob_ortaq   = bob_gizli.exchange(alice_açıq)

assert alice_ortaq == bob_ortaq   # MITM yoxdursa, hər ikisi eyni ortaq açarı alır
```

> **Tapşırıq fikri:** İndi bu simulyasiyaya "Mallory" adlı üçüncü tərəf əlavə et — Mallory Alice və Bob ilə AYRI-AYRI DH razılaşması qursun, ortadan mesajları oxuyub ötürsün. Bunun qarşısını necə TLS sertifikatlı kimlik doğrulaması alır — çünki Mallory sertifikatı imzalaya bilmir.

---

# HİSSƏ VII — MÜASİR İSTİQAMƏTLƏR

## 28. Blockchain və kriptoqrafiya

- Hər blok özündən ƏVVƏLKİ blokun hash-ini özündə saxlayır — zəncir kimi. Bir blokda dəyişiklik etsən, ondan sonrakı BÜTÜN blokların hash-i pozulur — bu, dəyişikliyi dərhal aşkar edilə bilən edir.
- **Merkle ağacı:** minlərlə əməliyyatı bir tək 256-bit "kök" hash-də toplayır. Bu sayədə telefon kimi "yüngül" cihazlar bütün əməliyyatları yükləmədən, sadəcə bir əməliyyatın doğruluğunu yoxlaya bilir.
- İmzalama üçün ECDSA (Bitcoin) işlədilir — imza yaradanda təsadüfi ədədi (k) TƏKRARLAMAQ, gizli açarın oğurlanmasına səbəb ola bilər.

## 29. Kvant kompüterlər kriptoqrafiyanı necə təhdid edir?

### 29.1. İki fərqli təhlükə

| Bugünkü alqoritm | Kvant hücumu | Nəticə |
|---|---|---|
| RSA, Diffie-Hellman, ECC | Shor alqoritmi | TAM sındırılır (kifayət qədər güclü kvant kompüter olsa) |
| AES-256, SHA-256 | Grover alqoritmi | Yalnız 2 dəfə "sürətlənmə" verir — AES-256 hələ də təhlükəsiz qalır |

**Fərq niyə bu qədər böyükdür?** Shor alqoritmi RSA/ECC-nin əsaslandığı riyazi problemi (faktorizasiya, diskret loqarifm) TAMAMILƏ fərqli üsulla həll edir. Grover isə sadəcə brute-force axtarışını sürətləndirir — buna görə açar uzunluğunu ikiqat artırmaq (128→256 bit) kifayət edir.

### 29.2. Bugün nə qədər real təhlükədir?

Hələ heç bir kvant kompüter RSA-2048-i sındıra biləcək qədər güclü deyil (minlərlə stabil kubit lazımdır, indi yüzlərlədir). AMMA "indi yığ, sonra aç" (harvest now, decrypt later) təhlükəsi realdır — hücumçular bugün şifrəli trafiki yığıb saxlaya, sonra kvant kompüter gələndə açmağa çalışa bilər. Bu yüzdən şu anki HƏSSAS məlumatlar üçün artıq ehtiyat tədbir görülür.

### 29.3. Kvanta davamlı yeni standartlar (NIST, 2024)

| Standart | Nəyə əsaslanır | Vəzifəsi |
|---|---|---|
| ML-KEM (Kyber) | Lattice riyaziyyatı | Açar mübadiləsi |
| ML-DSA (Dilithium) | Lattice riyaziyyatı | Rəqəmsal imza |
| SLH-DSA (SPHINCS+) | Hash funksiyaları | Ehtiyat imza sistemi |

## 30. Steqanoqrafiya — mesajın VARLIĞINI gizlətmək

**Fərq kriptoqrafiya ilə:** kriptoqrafiya mesajın MƏZMUNUNU gizlədir (mesaj olduğu bəllidir, oxunmur), steqanoqrafiya isə mesajın ÜMUMİYYƏTLƏ OLDUĞUNU gizlədir.

### 30.1. LSB üsulu (ən yayılmış)

Şəkil piksellərinin rəng dəyərlərinin son bitini (ən az əhəmiyyətli bit) dəyişməklə mesaj gizlədirsən. Rəng çox az dəyişdiyi üçün insan gözü fərqi görmür.

```python
from PIL import Image
img = Image.open("sekil.bmp")
pixels = img.load()
mesaj = "Gizli mesaj".encode()
bitler = "".join(f"{b:08b}" for b in mesaj)
i = 0
for y in range(img.height):
    for x in range(img.width):
        r, g, b = pixels[x, y]
        if i < len(bitler):
            b = (b & 0xFE) | int(bitler[i])   # son biti dəyiş
            pixels[x, y] = (r, g, b)
            i += 1
img.save("stego.bmp")
```

**Aşkarlanması:** aşkarlanma alətləri (məsələn stegdetect) təbii şəkillərin LSB-lərinin təsadüfi paylandığını, gizli mesajı olan şəkillərdə isə bu paylanmanın "qeyri-təbii" göründüyünü statistik olaraq yoxlayır.

## 31. QR kodlar və təhlükəsizlik

- Wi-Fi QR kodunda parol (`WIFI:T:WPA;S:...;P:parol;;`) AÇIQ MƏTNLƏ yazılır — QR-i skan edən istənilən proqram parolu oxuya bilər.
- **"Quishing"** — sahtə QR kodları vasitəsilə fişinq. Müdafiə: linki açmadan öncə URL-ə baxmaq, yalnız etibarlı mənbədən gələn QR-ları skan etmək.

## 32. Öyrənmə üçün platformalar

| Platforma | Nə üçün yaxşıdır |
|---|---|
| cryptohack.org | İnteraktiv kriptoqrafiya tapşırıqları (XOR, RSA, ECC, AES) |
| picoCTF | Yeni başlayanlar üçün addım-addım |
| CTFtime | Yarış təqvimi |
| HackTheBox / RootMe | Praktiki laboratoriya mühiti |

**Kiçik təcrübə planı:** Sezar+Vigenère zənciri qır → RSA-da zəif açar (e=3) tapşırığı → ECB rejimində şəkil sındırma tapşırığı → PNG şəklindən LSB mesaj çıxar → picoCTF-dən 3 kripto tapşırığı.


---

# HİSSƏ VIII — YADDA SAXLANMALI 25 FAKT (SADƏLƏŞDİRİLMİŞ)

1. Təhlükəsizlik AÇARDA olmalıdır, alqoritmi gizlətməkdə deyil (Kerckhoffs prinsipi).
2. Yaxşı şifrə iki şey edir: difuziya (bir dəyişiklik hər yerə yayılır) + konfüzyon (açar-şifrə əlaqəsi qarışıqdır).
3. OTP riyazi olaraq QIRILMAZ yeganə sistemdir — amma şərti: açar mətn qədər uzun, tam təsadüfi, YALNIZ BİR DƏFƏ işlədilməlidir.
4. Sezar cəmi 25 açara malikdir; Vigenère isə Kasiski/Friedman üsulları ilə açılır.
5. DES-in 56-bit açarı 1998-ci ildə 56 saata sındırılıb — buna görə artıq işlədilmir.
6. Feistel şəbəkəsi: eyni sxem HƏM şifrələmə, HƏM deşifrələmə üçün işlədilir, sadəcə açarlar tərs sırayla verilir.
7. AES-in 4 addımı: SubBytes, ShiftRows, MixColumns, AddRoundKey — 10/12/14 dövr (açar ölçüsündən asılı).
8. ECB rejimi eyni bloku eyni şəkildə şifrələyir → strukturu (məsələn şəklin konturunu) gizlətmir → İŞLƏTMƏ.
9. CBC rejimi padding oracle hücumuna açıqdır; CTR-də nonce təkrarı fəlakətdir; GCM bugünkü qızıl standartdır (məxfilik + bütövlük birlikdə).
10. RC4 artıq tam köhnəlib — istifadə etmə.
11. ChaCha20 sadəcə toplama+dövr+XOR əməliyyatları edir, cache-timing hücumlarına davamlıdır, mobil cihazlarda AES-dən sürətlidir.
12. RSA-da: n=p×q; e×d≡1 (mod φ(n)); real istifadədə HƏMİŞƏ OAEP padding lazımdır; minimum 2048 bit açar.
13. Diffie-Hellman/ECC-nin təhlükəsizliyi "diskret loqarifm problemi"nin çətinliyinə əsaslanır.
14. Forward secrecy = hər sessiyada YENİ, müvəqqəti açar (TLS 1.3-də məcburidir).
15. MD5 və SHA-1 artıq sındırılıb, təhlükəsizlik məqsədilə İŞLƏDİLMƏMƏLİDİR; SHA-256/SHA-3 hələ sağlamdır.
16. HMAC — daxili+xarici iki qatlı hash, uzantı hücumlarına qarşı qoruyur.
17. Parol saxlamaq üçün: bcrypt/scrypt/Argon2id işlət, SADƏ SHA yox — mütləq salt əlavə et.
18. ECDSA-da açar imzalayan `k` təsadüfi ədədinin TƏKRARLANMASI = gizli açarın tam açılması deməkdir.
19. X.509 sertifikatında SAN sahəsinə diqqət et (real hostname budur); CA imzasını yoxla; OCSP sertifikatın ləğv edilib-edilmədiyini yoxlayır.
20. TLS 1.3 daha sürətlidir (1 gediş-gəliş), YALNIZ AEAD şifrələri və məcburi forward secrecy işlədir.
21. POODLE — SSLv3-ün padding zəifliyi; SWEET32 — 64-bit blokların (3DES) böyük trafikdə açıla bilməsi. Hər ikisi köhnə protokolları/şifrələri söndürməklə həll olunur.
22. IKE Aggressive Mode-da doğrulama məlumatı AÇIQ gedir → offline lüğət hücumuna açıqdır.
23. IKEv2, IKEv1-dən daha təhlükəsizdir; WireGuard (ChaCha20 əsaslı) bugünkü ən müasir VPN seçimidir.
24. Kvant kompüterlər Shor alqoritmi ilə RSA/DH/ECC-ni TAM sındıra bilər, amma AES-256 Grover alqoritminə qarşı hələ təhlükəsizdir; Kyber/Dilithium yeni standartlardır.
25. Steqanoqrafiya mesajın VARLIĞINI, kriptoqrafiya isə MƏZMUNUNU gizlədir; LSB üsulu statistik testlərlə aşkarlana bilər.

---

# HİSSƏ IX — İMTAHAN ÜÇÜN 35 SUAL

Bu suallar orijinal dərslikdən saxlanılıb — cavabları yuxarıdakı bölmələrdə tapa bilərsən.

**A. Nəzəri suallar**

1. Kerckhoffs prinsipini izah et və "təhlükəsizlik gizlətməyə əsaslanmamalıdır" ifadəsinin mənasını göstər.
2. Shannon-un difuziya və konfüzyon prinsiplərini izah et, AES-də hansı əməliyyat hansına uyğun gəlir?
3. Ciphertext-only hücum modelində hücumçu nə bilir? OTP niyə buna qarşı davamlıdır?
4. Sezar şifrəsinin açar fəzası nə qədərdir? Affine şifrəsinin 312 açarı olmasına baxmayaraq niyə hələ zəifdir?
5. Kasiski üsulunda periodun ƏBOB ilə tapılmasının məntiqini izah et.
6. Vigenère-ə qarşı Friedman testi (IC) necə işləyir? IC dəyərlərini yaz.
7. Hill şifrəsində açar matrisinə hansı şərt qoyulur və niyə?
8. OTP-nin üç şərtini yaz və açarın təkrarlanmasının niyə fəlakət olduğunu göstər.
9. Feistel şəbəkəsində deşifrələmənin niyə eyni sxemlə mümkün olduğunu izah et.
10. DES-də genişləndirmə, S-box və permutasiyanın rolu nədir? S-box niyə qeyri-xətti olmalıdır?
11. AES-in bir dövrünü addım-addım yaz.
12. AES-ə qarşı biclique hücumunun nəticəsini və niyə praktik olmadığını izah et.
13. CBC, CTR və GCM rejimlərini müqayisə et: hər birinin tipik zəifliyi nədir?
14. PKCS#7 padding nədir? Padding oracle hücumu necə işləyir?
15. RC4-ün KSA və PRGA fazalarını izah et. WEP-in problemini göstər.
16. ChaCha20-nin niyə cache-timing hücumlarına davamlı olduğunu izah et.
17. RSA açar generasiyasını addım-addım yaz.
18. Textbook RSA-nın 3 zəifliyini (determinizm, malleability, kiçik mesaj) və OAEP-in hər birini necə həll etdiyini izah et.
19. Diffie-Hellman protokolunu addım-addım yaz və ona qarşı MITM hücumunun necə işlədiyini göstər.
20. Niyə ECC 256 bit ≈ RSA 3072 bit təhlükəsizlik verir?
21. MD5-in Merkle-Damgård strukturunu izah et, zəifliyinin nəticəsini göstər.
22. HMAC-ın iki qatlı (ipad/opad) quruluşunun niyə vacib olduğunu izah et.
23. Rainbow table necə işləyir? Salt onu niyə "öldürür"?
24. ECDSA-da `k` sızmasının gizli açarı necə ifşa etdiyini izah et.
25. X.509 sertifikatında SAN, KeyUsage, BasicConstraints sahələrinin funksiyasını izah et.
26. TLS 1.2 və 1.3 handshake-lərini müqayisə et: RTT sayı, alqoritm dəsti, 0-RTT riski.
27. POODLE hücumunun addımlarını yaz.
28. SWEET32-də ad günü paradoksunun rolu nədir?
29. IKE Main və Aggressive Mode arasındakı fərqi mesaj axını üzərindən izah et.
30. Shor və Grover alqoritmlərinin fərqli təsirini izah et.

**B. Praktiki tapşırıqlar**

31. `sezar_desifrele` funksiyası ilə bu şifrəni qır: `ESP BFTNVMCZHY` (İpucu: İngiliscədir).
32. picoCTF-də "Mod 26" və "Easy1" tapşırıqlarını həll et, həll yolunu yaz.
33. Öz kompüterində `openssl s_server` işə sal, `openssl s_client` ilə TLS 1.3 bağlantısı qur.
34. `hashcat -m 0` ilə verilmiş MD5 hash-ini rockyou lüğəti ilə qırmağa çalış.
35. Diffie-Hellman simulyasiyasına Mallory (MITM) əlavə et, TLS-in bunu necə əngəllədiyini bir cümlə ilə izah et.

---

# ƏLAVƏ: Daha dərin oxumaq üçün

- Bruce Schneier — *Applied Cryptography* (köhnədir, amma intuisiya üçün əladır)
- Jean-Philippe Aumasson — *Serious Cryptography* (müasir, tövsiyə #1)
- Dan Boneh & Victor Shoup — *A Graduate Course in Applied Cryptography* (pulsuz PDF)
- NIST FIPS 197 (AES), FIPS 203/204/205 (post-kvant standartları)
- RFC 8446 (TLS 1.3), RFC 7296 (IKEv2)
- cryptohack.org — praktiki tapşırıqlar

*Bu sənəd sənin əvvəlki dərsliyinin sadələşdirilmiş və yenidən strukturlaşdırılmış versiyasıdır. Uğurlar!*
