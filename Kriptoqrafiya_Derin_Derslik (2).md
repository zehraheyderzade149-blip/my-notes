# KRİPTOQRAFİYA VƏ ŞƏBƏKƏ TƏHLÜKƏSİZLİYİ — DƏRİN ÖYRƏNMƏ DƏRSLİYİ

## (Qeydlər + təqdimat əsasında, ən xırdalıqlarına qədər)

> ⚠️ **Etik xəbərdarlıq:** Buradakı bütün alətlər və hücum üsulları yalnız öz laboratoriya mühitinizdə, test maşınlarınızda və ya yazılı icazəniz olan sistemlərdə tətbiq edilməlidir. İcazəsiz sistemlərə skan/hücum Azərbaycan Respublikası Cinayət Məcəlləsinin 272-ci (qanunsuz daxil olma) və əlaqəli maddələri ilə cinayətdir.

---

# HİSSƏ I — NƏZƏRİ ƏSASLAR VƏ KLASSİK ŞİFRƏLƏR

## ) — daxili quruluş

### 8.1. Tarixi

- 1973: NBS (indiki NIST) açıq müsabiqə1. Kriptoqrafiyanın riyazi təməli

NBS və NIST: NBS (National Bureau of Standards) ABŞ-ın Milli Standartlar Bürosunun keçmiş adıdır. 1988-ci ildən etibarən bu qurum NIST (National Institute of Standards and Technology - Milli Standartlar və Texnologiya İnstitutu) adlandırılmağa başlanıb.

Açıq Müsabiqə: NIST tarix boyu dövlət və ya qlobal miqyasda istifadə olunacaq şifrələmə standartlarını (məsələn, məşhur DES və AES alqoritmlərini) qapalı şəkildə deyil, bütün dünya alimləri və kriptoqrafları üçün açıq müsabiqələr elan etməklə seçir. Dünyanın hər yerindən mütəxəssislər yeni şifrələmə üsulları təklif edir, digər alimlər isə onları sındırmağa və ya zəif cəhətlərini tapmağa çalışırlar.

"Kriptoqrafiyanın riyazi təməli" nə deməkdir?
Əvvəllər şifrələmə metodları daha çox gizli və praktiki üsullara əsaslanırdısa, NBS/NIST-in başlatdığı bu açıq elmi proses kriptoqrafiyanı sırf hərbi və ya qapalı sahə olmaqdan çıxarıb ciddi riyazi təməllər üzərində qurdu.

Təklif olunan hər bir alqoritm sərt riyazi qanunlara (məsələn, saylar nəzəriyyəsi, böyük ədədlərin vuruqlara ayrılması, cəbri strukturlar) əsaslanmalıdır.

Açıq müsabiqələr sayəsində bu riyazi modellər qlobal miqyasda yoxlanılıb təsdiqlənmişdir.

Yəni ABŞ-ın bu qurumunun (əvvəlki NBS, indiki NIST) yaratdığı açıq müsabiqə ənənəsi müasir kriptoqrafiyanın elmi, güclü və etibarlı riyazi təməllər üzərində qurulmasının əsasını qoymuşdur.

### 1.1. Kriptosistem formallıqlaşdırması

Müasir kriptoqrafiya (Shannon, 1949-cu ildə "Communication Theory of Secrecy Systems" əsərilə) şifrəni formallılaşdırır:

Bir **şifrə (cipher)** üç alqoritmdən ibarətdir:

- **Gen(1ⁿ)** → açar `k` yaradır (təsadüfi, n-bit)
- **Enc(k, m)** → şifrəli mətn `c`
- **Dec(k, c)** → açıq mətn `m`

**Düzgünlük tələbi:** `Dec(k, Enc(k, m)) = m` hər m və k üçün ödənməlidir.

**Təhlükəsizlik tərifi (Kerckhoffs şərti):** Hücumçu alqoritmi bilir, yalnız açarı bilmir. Shannon bunu "düşmən sistemi tam analiz edə bilər" prinsipi ilə əsaslandırdı.

### 1.2. Shannon-un iki prinsipi

1. **Difuziya (diffusion)** — açıq mətnin bir bitinin dəyişməsi şifrəli mətnin təxminən yarısını dəyişməlidir (istatistik əlaqələri dağıdır). AES-də bunu **MixColumns** təmin edir.
2. **Konfüzyon (confusion)** — açarla şifrəli mətn arasındakı əlaqə mürəkkəb olmalıdır. AES-də bunu **SubBytes (S-box)** təmin edir.

> Bu iki prinsip Claude Shannon tərəfindən təklif edilib və bütün müasir blok şifrələrin dizayn fəlsəfəsidir.

### 1.3. Hücum modelləri (hücumçu nə qədər bilir?)

| Model | Hücumçunun əlində nə var | Nə qədər realdır |
| --- | --- | --- |
| Ciphertext-only | Yalnız şifrəli mətnlər | Ən zəif hücumçu — şifrə buna qarşı davamlı olmalıdır |
| Known-plaintext | Bəzi (açıq, şifrəli) cütlər | Real: header-lər məlumdur |
| Chosen-plaintext | İstədiyi mətni şifrələtdirə bilir | Public-key sistemlərdə standart |
| Chosen-ciphertext | İstədiyi şifrəli mətni deşifrələtdirə bilir | Padding oracle hücumlarının əsası |
| Adaptive variants | Cavablara əsasən sorğularını dəyişir | Müasir təhlil |

## 2. Əvəzetmə şifrəsi: Sezar — tam analiz

### 2.1. Düstur

`E(x) = (x + k) mod 26`, `D(y) = (y − k) mod 26`

Burada x hərfin əlifbadakı mövqəsidir (A=0, B=1, ..., Z=25).

### 2.2. Nümunə (əl ilə hesablama)

```JavaScript
Açıq:  S  A  L  A  M
mövqe:18  0 11  0 12
+k=3:  21  3 14  3 15
Şifrə: V  D  O  D  P
```

### 2.3. Niyə qırılır?

Açar fəzası: `|K| = 25` (0 və 26 eynidir). Cəmi 25 yoxlama. Hətta əllə qırıla bilər. Claude Shannon-un terminologiyası ilə: **açar entropiyası ≈ 4.7 bit** — kompüter üçün sıfır saniyə.

**Tezlik analizi (frekans analysis):** İngilis dilində ən çox hərflər: E(12.7%), T(9.1%), A(8.2%), O(7.5%), I(7.0%). Azərbaycan dilində: A, Ə, İ, L, R, N daha tez-tez rast gəlinir. Şifrəli mətdə ən çox təkrarlanan hərf → ehtimal ki, E və ya A ilə uyğunlaşdırılır və sınaq aparılır.

### 2.4. Affine şifrəsi (Sezarın ümumiləşməsi)

`E(x) = (ax + b) mod 26`, burada `gcd(a, 26) = 1` olmalıdır (əks element mövcud olsun deyə).

- a variants: φ(26) = φ(2)·φ(13) = 1·12 = 12
- b variants: 26
- Cəmi açar: 12 × 26 = **312** — hələ də trivialdır.

**Əks element:** `D(y) = a⁻¹(y − b) mod 26`. Məs: a=5 üçün a⁻¹=21, çünki 5·21=105=1 mod 26.

## 3. Vigenère şifrəsi — dərin analiz

### 3.1. Mexanizm

Açar söz `K = (k₁, k₂, ..., kₘ)`. i-ci hərf üçün sürüşmə `kᵢ mod 26`:

```JavaScript
Açar:  K  E  Y  K  E  Y
Açıq:  A  T  T  A  C  K
Sürüş:10  4 24 10  4 24
Şifrə: K  X  R  K  G  I
```

Bu, m sayda paralel Sezar şifrəsidir. Açar təkrarlanana qədər period = m.

### 3.2. Niyə 300 il qırılmadı?

- eyni hərf fərqli yerdə fərqli simvolla şifrələnir → sadə tezlik analizi işləmir
- Kuhnun sözləri ilə "le chiffre indéchiffrable" (qırılmaz şifrə) adlanırdı

### 3.3. Kasiski üsulu (1863) — addım-addım

1. Şifrəli mətdə təkrarlanan 3+ hərflik ardıcıllıqları tap (məs. "KXR" 3 yerdə)
2. Onların mövqelər arası məsafələri hesapla: d₁, d₂, ...
3. Məsafələrin **ƏBOB**-unu tap → period m
4. Hər m-ci simvolu götür, hər sətri ayrıca Sezar kimi tezlik analizi ilə qır

### 3.4. Friedman testi (index of coincidence)

`IC = Σ fᵢ(fᵢ−1) / (N(N−1))` , burada fᵢ hərfin tezliyi.

- Təsadüfi mətn: IC ≈ 0.0385
- İngilis mətni: IC ≈ 0.0667
- Şifrənin fərqli altmətnlərini (shift-lərini) ayırıb IC hesablayaraq period təxmin edilir.

## 4. Playfair şifrəsi — tam mexanizm

### 4.1. 5×5 kvadrat qurulması

Açar söz yazılır, təkrar hərflər atılır, qalan əlifba əlavə olunur (İngilis variantında I/J bir xanada). Məs. açar "KEYWORD":

```JavaScript
K E Y W O
R D A B C
F G H I L
M N P Q S
T U V X Z
```

### 4.2. Şifrələmə qaydaları

Mətn cüt-cüt bölünür (eyni hərflər cütündə araya X əlavə edilir: "BALLOON" → BA LX LO ON):

1. **Eyni sətirdə:** hər hərf sağdakı ilə əvəz olunur (sağdan son isə ən sola)
2. **Eyni sütunda:** hər hərf aşağıdakı ilə (aşağıdan son isə ən yuxarı)
3. **Düzbucaqlı:** hər hərf öz sətrində, digərinin sütunundakı küncə keçir

### 4.3. Nümunə

Cüt "HE": H(3-cü sətir, 3-cü sütun), E(1-ci sətir, 2-ci sütun) → düzbucaqlı: H→G (3-cü sətir, 2-ci sütun), E→Y (1-ci sətir, 3-cü sütun) → "GY".

### 4.4. Zəiflik

- Cüt statistikası saxlanılır: TH, ER, ON kimi cütlərin əvəzlənmələri müəyyən edilə bilər
- Teorik variant sayı böyük olsa da (25!/...), praktikada tezlik analizi ilə qırılır
- Tək hərf əvəzləməkdən güclüdür, amma 25 cüt əvəzetmə cədvəlindən çox deyil

## 5. Hill şifrəsi — analitik şifrələmə nümunəsi

Təqdimatda "analitik şifrələmə üçün vektorun matrisə vurulması" deyilir — bu Hill şifrəsidir.

`C = K·P mod 26`, burada K — n×n açar matrisi.

- Deşifrələmə: `P = K⁻¹·C mod 26`
- K⁻¹ mod 26 mövcuddur ⇔ `det(K)` 26 ilə qarşılıqlı sadədir
- Zəiflik: known-plaintext ilə xətti cəbr qurulub həll edilir (n cüt (P,C) kifayətdir)

## 6. Qamma şifrəsi / One-time pad (Vernam)

`C = P ⊕ K` (XOR), K — mətn qədər uzun, tam təsadüfi, təkrarlanmayan açar.

**Məhzəmsiz təhlükəsizlik:** hər şifrəli mətnə uyğun İSTƏNİLƏN açıq mətn mümkün açarla alına bilər → şifrəli mətn heç bir informasiya vermir (Shannon tərəfindən isbat olunub). **BUTUN ŞƏRTLƏR:** açar tam təsadüfi, mətn qədər uzun, birdəfəlik istifadə olmalıdır. Təkrarlansa → iki şifrəni XOR-layanda açar yox olur: `C₁ ⊕ C₂ = P₁ ⊕ P₂` və tezlik analizi mümkün olur (Venona layihəsi belə qırıldı).

**Praktik problem:** açar paylanması. Buna görə müasir praktikada OTP yerinə axın şifrələri istifadə olunur (qamma deterministik, amma kriptoqrafik cəhətdən təsadüfi görünən açar axını).

## 7. Şifrələrin sinifləri — ümumiləşdirmə

| Sinif | Əsas əməliyyat | Nümunə | Güclü tərəf | Zəif tərəf |
| --- | --- | --- | --- | --- |
| Yerdəyişmə | Simvolların yerini dəyişir | Sezar, sütun | Sadə | Tezlik analizi |
| Əvəzetmə | Simvol ↔ simvol | Monoalphabetik | Sürət | Dil statistikası |
| Qammalaşdırma | P ⊕ qamma | Vernam | OTP-də məhzəmsiz | Açar paylanması |
| Blokvari | Bloklar üzərində çevirmələr | AES, DES | Standart, sürətli | Rejimdən asılı |

# HİSSƏ II — SİMMETRİK (GİZLLI AÇARLI) ŞİFRƏLƏR

## 8. DES (Data Encryption Standard elan etdi

- 1974: IBM-in Lucifer şifrəsi əsasında qəbul → 1976 DES
- 56-bit açar, 64-bit blok, 16 raund Feistel şəbəkəsi

### 8.2. Feistel şəbəkəsi — DES-in skeleti

Bloq sol (L₀) və sağ (R₀) yarılara bölünür. Hər raund:

```javascript
L(i) = R(i-1)
R(i) = L(i-1) ⊕ F(R(i-1), K(i))
```

F-funksiya (raund funksiyası):

1. **Expansion E:** 32 bit → 48 bit (bitlər təkrarlanır)
2. **XOR** raund açarı K(i) ilə (48 bit)
3. **S-boxlar:** 8 ədəd 6→4 bit qeyri-xətti əvəzetmə (kriptoqrafiyanın "ürəyi")
4. **Permutation P:** 32 bit qarışdırılır

**Deşifrələmə** eyni şəbəkə ilə, açarlar tərs ardıcıllıqla (Feistel şəbəkəsinin gözəlliyi: D(L,R) üçün struktur eyni qalır).

### 8.3. İlkin və son permutasiyalar (IP, FP)

IP/FP birləşmə açarlı deyil, effekti sıfırdır (müəyyən məqsədlər üçün 1970-ci illərdə hardware-də paralel yükləmə üçün). Daha sonra təhlilçilər bunun software təcili üçün əngəl yaratdığını gördü.

### 8.4. Açar genişlənməsi (Key Schedule)

56-bit açar (hər 8-ci bit paritet) → PC-1 ilə 56 bit seçilir → C və D 28-bit hissələrə → hər raundda shift → PC-2 ilə 48 bit raund açarı çıxarılır.

### 8.5. DES-in qırılması

- 1993: Wiener theoretically $1M maşınla saatlarla qırıla biləcəyini göstərdi
- 1997: DESCHALL (minlərlə İnternet kompüteri) 96 gündə açarı tapdı → "distributed brute force"
- 1998: EFF-in "Deep Crack" maşını (~$250K) **56 saatda** qırdı
- Nəticə: NIST 2001-də AES seçdi

**Brute-force hesablaması:** 2⁵⁶ ≈ 7.2×10¹⁶ açar. 1 milyard açar/saniyə = ~835 gün; 10⁶ paralel nüvə = ~72 dəqiqə.

## 9. 3DES (Triple DES)

`C = E_K3(D_K2(E_K1(P)))` — şifrələ-DEŞİFRƏLƏ-şifrələ (EDE) quruluşu: DES hardware ilə retro-uyğunluq üçündür.

- 2-key 3DES: 112 bit effektiv; 3-key: 168 bit (lakin "meet-in-the-middle" → təhlükəsizlik ~112)
- **SWEET32** (bax bölmə 20): 64-bit blok səbəbindən köhnəldi → 2017-dən NIST rəsmi tövsiyə etmir

## 10. AES (Rijndael) — tam daxili mexanizm

### 10.1. Tarixi

1997: NIST açıq müsabiqə. 15 namizəd (MARS, RC6, Serpent, Twofish, Rijndael...). 2000: Rijndael (Belçikalı kriptoqrafikçılar Daemen və Rijmen) qalib. AES-128/192/256.

### 10.2. Parametrlər

- Blok: 128 bit = 16 bayt → 4×4 **State** matrisi
- Raundlar: 10 (128), 12 (192), 14 (256)
- Açar genişlənməsi: Rijndael Key Schedule (Rcon, SubWord, RotWord)

### 10.3. Dörd əməliyyat (hər raundda, son raundda MixColumns-suz)

1. **SubBytes:** hər bayt S-box ilə əvəz olunur (qeyri-xətti konfüzyon). S-box GF(2⁸)-də `x → x⁻¹ + b` (x=0 üçün 0).
2. **ShiftRows:** sətirlər sola sürüşür: 2-ci sətir 1, 3-cü 2, 4-cü 3 mövqə.
3. **MixColumns:** hər sütun sabit matrislə GF(2⁸)-də vurulur (difuziya!). İstinad polinomu: x⁸+x⁴+x³+x+1.
4. **AddRoundKey:** açarla XOR.

### 10.4. Niyə AES qırılmayıb?

- Ən yaxşı universal hücum: **biclique** — AES-128 üçün 2¹²⁶.1 (yalnız 4 qat sürətləndirmə, praktik deyil)
- Diferensial/lineer kriptoanaliz AES-in dizaynına görə effektiv deyil
- Kriptoanalizin hədəfi indi AES-in özü deyil, **realizasiyaları**: side-channel (vaxt, enerji, cache), səhv hücumları (fault injection)

### 10.5. AES Challenge (təqdimat mövzusu)

"Eyni mətn, müxtəlif rejimlər" — məqsəd: eyni AES alqoritmi fərqli rejimlərdə tam fərqli təhlükəsizlik verir. ECB-də "pinguin problemi" (təqdimat şəkli): eyni piksel blokları → eyni şifrə blokları → şəkil silueti şifrəli mətnə aydın görünür. CBC/CTR/GCM-də belə siluet qalmır.

## 11. İş rejimləri — dərin müqayisə (diskussiya mövzusu)

### 11.1. ECB (Electronic Codebook)

`Cᵢ = E(K, Pᵢ)`. Eyni blok → eyni şifrə. **Tamamilə təhlükəsizdir? Xeyr — strukturu gizlətmir.** Hər blok müstəqil → hərəkətlər, siluetlər, ümumi forma açıq qalır.

### 11.2. CBC (Cipher Block Chaining)

`Cᵢ = E(K, Pᵢ ⊕ Cᵢ₋₁)`, ilk blok: `C₁ = E(K, P₁ ⊕ IV)`.

- IV təsadüfi olmalı, şifrələnməməlidir, yalnız təkrarlanmamalıdır
- Deşifrələmə paralelləşir, şifrələmə yox
- **Zəiflik:** padding oracle hücumları (POODLE bunun qohumudur — bax 20.1), paralel şifrələmə yoxdur

### 11.3. CTR (Counter)

Axın kimi işləyir: `Cᵢ = Pᵢ ⊕ E(K, counterᵢ)`.

- Paralel həm şifrə, həm deşifrə
- Raund funksiyası açar axını generatoru kimi
- **nonce+counter təkrarlanmamalıdır** — əks halda iki şifrə XOR-lanıb mətn açılır (CTR ufuk problemi)
- AEAD (GCM) nədir: `AES-GCM = CTR + GHASH (Poly əmsallı hash)` → eyni vaxtda məxfilik + bütövlük (integrity tag 128 bit). Cari qızıl standart.

### 11.4. Digər rejimlər

- **CFB/OFB:** axınvari, feedback-lərlə; çox az istifadə olunur
- **XTS:** disk şifrələməsi üçün (tweak ilə sektor başına)

### 11.5. Padding

Blok sonu tamamlanır: **PKCS#7** — n bayt çatışmazlığı varsa n dəyərini n dəfə yaz. Deşifrələ bilməyən pad → hücumçu üçün oracle.

## 12. Axın şifrələri — dərin analiz

### 12.1. RC4 (Ron Rivest, 1987) — niyə öldü?

İki fazalı:

- **KSA (Key Scheduling Algorithm):** 256 baytlık S permutasiyası açarla qarışdırılır:

```javascript
for i in 0..255: S[i]=i; j=0
for i in 0..255:
    j = (j + S[i] + key[i mod keylen]) mod 256
    swap(S[i], S[j])
```

- **PRGA (Pseudo-Random Generation Algorithm):** keystream baytı:

```javascript
i = j = 0 (hər şifrə sessiyasında yenidən!)
loop: i++; j = (j+S[i]) mod 256; swap(S[i],S[j]); output S[(S[i]+S[j]) mod 256]
```

**Ölümcül zəifliklər:**

1. Açar + IV (WEP-də 24-bit IV) birləşdirilir → Fluhrer-Mantin-Shamir 2001: açar baytları keystream statistikasından bərpa olunur
2. RC4 keystream-in ilk baytları güclü yönəlmə göstərir (Mantin-Shamir bias)
3. TLS-də `TLS_RC4_*` şifrələri 2015 RFC 7465 ilə qadağan
4. WPA-TKIP (Wi-Fi) zəifliyi də RC4 əsasıdır → WPA2-də AES-CCMP-ə keçid

### 12.2. Salsa20 / ChaCha20 (Daniel J. Bernstein) — daxili quruluş

**ChaCha20** — Salsa20-nin təkmilləşdirilmiş variantı (2014 Google tərəfindən TLS üçün təklif).

- Dövlət: 16 ədəd 32-bit sözdən ibarət 4×4 matris:
- 4 sabit söz (0x61707865, 0x3320646e, 0x79622d32, 0x6b206574 — "expand 32-byte k" sözlərinin kodu)
- 8 açar sözü (256-bit açar)
- 3 nonce/dövriyyə sözü (96-bit nonce + 32-bit counter)
- 1 counter
- 20 raund = 10 dəfə **double round** (column round + diagonal round)

**Quarter Round (QR)** — sətir/sütun dörd söz üzərində:

```javascript
a += b; d ^= a; d <<<= 16;
c += d; b ^= c; b <<<= 12;
a += b; d ^= a; d <<<= 8;
c += d; b ^= c; b <<<= 7;
```

(qeyd: `<<<= n` = rotl — sola dövrəvi sürüşmə; heç bir ayrıca lookup cədvəli yoxdur — cache-timing hücumlarına davamlı!)

**Şifrələmə:** add_state = matrix; 20 raunddan sonra state ilə add_state XOR → 64 bayt keystream bloku; `C = P ⊕ keystream`.

### 12.3. ChaCha20 + Poly1305 (AEAD)

- **Poly1305:** 130-bit primo əmsallı bir-biricik (one-time) MAC: `tag = (msg_aad·r + s) mod 2¹³⁰−5`, r və s açar müqaviləsindən (ChaCha raund nəticəsindən) çıxarılır
- TLS-də şifr paketi: `TLS_CHACHA20_POLY1305_SHA256`
- **Niyə mobil cihazlarda AES-dən sürətli?** AES hardware-də (AES-NI) sürətli olsa da, ARM cihazlarda AES-NI yoxdur və AES vaxt-təhlükəsiz proqram realizasiyası ağırdır; ChaCha20 + Poly1305 proqramda çox sürətlidir. Google məlumatına görə mobil HTTPS-də ~30% sürət qazancı.

### 12.4. Axın vs Blok — yekun fərq cədvəli

| Kriteriya | Blok (AES) | Axın (ChaCha20) |
| --- | --- | --- |
| Məlumat ölçüsü | Blok qədər (128b) | Bayt-bayt |
| IV/nonce | CBC: IV, CTR/GCM: nonce(96b) | nonce (96b) + counter |
| Bütövlük | Rejimdən asılı (GCM əlavə edir) | Poly1305 ilə AEAD |
| Sürət (SW) | Orta (S-box lookups) | Çox sürətli (yalnız ARX: add-rotate-xor) |
| Sürət (HW AES-NI) | Ən sürətli | Orta |
| Tipik istifadə | Disk, fayl, AES-GCM | TLS mobil, WireGuard (ChaCha20-Poly1305) |

---

# HİSSƏ III — ASİMMETRİK (AÇIQ AÇARLI) KRİPTOQRAFİYA

## 13. Riyazi təməl — modul əritmətikası (bunları bilmədən RSA anlaşılmaz)

### 13.1. Əsas anlayışlar

- **a ≡ b (mod n):** n, (a−b)-ni bölür.  Saat hesabı: 14:00 + 5 saat = 19:00 → 7 (mod 12).
- **Qarşılıqlı sadəlik:** gcd(a, n) = 1
- **Euler φ funksiyası:** φ(n) = n-dən kiçik, n ilə qarşılıqlı sadə ədədlərin sayı
- p sadədirsə: φ(p) = p−1
- p·q (iki fərqli sadə): φ(pq) = (p−1)(q−1)
- **Euler teoremi:** gcd(a,n)=1 → a^φ(n) ≡ 1 (mod n)
- **Fermatın kiçik teoremi:** a^(p−1) ≡ 1 (mod p) — p sadə, p∤a

### 13.2. Ən böyük ortaq bölən — genişləndirilmiş Evklid alqoritmi (EEA)

EEA həm gcd verir, həm də Bézout əmsalları: gcd(a,b) = a·x + b·y.
Tətbiq: modul əritmətikasında əks element a⁻¹ (mod n) tapmaq.

**Nümunə (əl ilə):** 7⁻¹ (mod 26):
EEA: 26 = 3·7 + 5; 7 = 1·5 + 2; 5 = 2·2 + 1
Geri: 1 = 5 − 2·2 = 5 − 2·(7 − 1·5) = 3·5 − 2·7 = 3·(26 − 3·7) − 2·7 = 3·26 − 11·7
→ −11·7 ≡ 1 (mod 26) → 7⁻¹ ≡ −11 ≡ **15 (mod 26)**. Yoxla: 7·15 = 105 = 4·26+1 ✅

### 13.3. Sürətli modul qüvvətləndirmə — square-and-multiply

`aᵉ mod n` — e-in bitləri üzrə: hər bitdə kvadratla, 1 bitdə vur.
RSA-də sürətli qüvvətləndirmənin əsasıdır. Nümunə: 3^13 mod 7:
13 = 1101₂ → 3²≡2; (3²)²·3 = 2²·3 = 12 ≡ 5; ... → 3^13 ≡ 3 (mod 7).

## 14. RSA — tam mexanizm, riyazi nümunə, qırılma üsulları

### 14.1. Açar generasiyası

1. İki böyük sadə seç: p, q (indiki standart ≥ 1024 bit hər biri)
2. n = p·q (modulus, 2048 bit)
3. φ(n) = (p−1)(q−1)
4. e seç: 1 < e < φ(n), gcd(e, φ(n)) = 1 (adətən e = 65537 = 2¹⁶+1 — sadə və sürətli: iki 1 bit)
5. d = e⁻¹ (mod φ(n)) — EEA ilə

- **Açıq açar:** (n, e) — yayılır
- **Gizli açar:** (n, d) — yalnız sahibdə

### 14.2. Şifrələmə / deşifrələmə

`c = mᵉ mod n` ;  `m = cᵈ mod n`

**Nümunə (kiçik ədədlərlə, əl ilə):**
p=5, q=11 → n=55, φ=40
e=3 (gcd(3,40)=1) → d: 3d ≡ 1 mod 40 → d=27 (3·27=81=2·40+1 ✅)
Şifrələ m=7: c = 7³ mod 55 = 343 mod 55 = 343 − 6·55 = **13**
Deşifrə: 13^27 mod 55 — square-multiply ilə: 13²=169≡4; 13⁴≡16; 13⁸≡256≡36; 13^16≡36²=1296≡31
27 = 16+8+2+1: 31·36·4·13 = 31·36=1116≡16; 16·4=64≡9; 9·13=117≡**7** ✅

### 14.3. Niyə işləyir? (riyazi əsası)

`cᵈ = m^(ed) mod n`. Çünki ed ≡ 1 (mod φ(n)) → ed = kφ(n)+1 →
m^(ed) = m·m^(kφ(n)) ≡ m·1ᵏ = m (mod n) (Euler teoremi, gcd(m,n)=1 halında; ümumi hal Çin qalıq teoremi ilə isbatlanır).

### 14.4. Real həyatda niyə "textbook RSA" istifadə olunmur?

1. **Deterministikdir** — eyni m hər dəfə eyni c verir → hücumçu lüğət yoxlaya bilər
2. **Malleable:** c' = c·sᵉ mod n → deşifrədə m' = m·s (chosen-ciphertext)
3. **Həmçinin:** kiçik m, e=3 olduqda mᵉ < n → tam qüvvət alınır, kub kök ilə qırılır

**Həll — padding:** **OAEP (Optimal Asymmetric Encryption Padding)**: m + təsadüfi seed + MGF1 maska ilə "yumuşaldılır". PKCS#1 v1.5 (köhnə, PKCS#1 v1.5 padding oracle → Bleichenbacher hücumu).

### 14.5. RSA-nın qırılma üsulları

- **Tam faktorizasiya (GNFS — General Number Field Sieve):** RSA-768 (232 onluq rəqəm) 2009-cu ildə qırıldı (yüzlərlə maşın, 2 il). RSA-2048 GNFS ilə qırıla bilsə, dünya maliyyə sistemi bir gecədə dağılardır.
- **Zəif açar generasiyası:** 2012 tədqiqat (Lenstra et al.): milyonlarla real açar cihazdan toplanıb, 0.2%-də n = p·q = n' = p·q' paylaşılan p faktoru tapıldı (RNG zəifliyi!).
- **Timing attack (Kocher 1996):** d bitlərindən asılı şifrələmə vaxtını ölçüb d bərpa edir → müdafiə: constant-time kod, RSA blinding
- **Power/EM side-channel, fault attack (Bellcore):** CRT optimizasiyasında səhv → q tapılır
- **ROCA (2017):** Estoniya sertifikat çiplərində zəif açar generasiyası → 2²⁵.8 faktorizasiya

### 14.6. Diffie-Hellman (1976) — açar mübadiləsi

**Problem:** Açıq kanaldan iki tərəf gizli açar razılaşdırmaq istəyir.

**Halq variantı:**

- Ümumi: sadə p, generator g (g-in qədər qüvvətləri mod p bütün qrupları gəzir)
- Alice: təsadüfi a (gizli), A = gᵃ mod p → göndər
- Bob: təsadüfi b, B = gᵇ mod p → göndər
- **Ortaq açar:** K = Bᵃ mod p = g^(ab) mod p = Aᵇ mod p ✅ (hər ikisi eyni K alır)
- Hücumçu p, g, A, B görür; a, b tapmaq = **diskret loqarifm problemi (DLP)** — p ≥ 2048 bit olduqda praktik qeyri-mümkün (GNFS-DLP: log₂(p) ~ 112-bit təhlükəsizlik)

**ElGamal** (1985) — DH üzərində şifrələmə:

- Açıq açar: (p, g, gᵃ mod p); gizli: a
- Şifrələ m: təsadüfi k; c₁ = gᵏ, c₂ = m·(gᵃ)ᵏ → c = (c₁, c₂)
- Deşifrə: m = c₂ · (c₁ᵃ)⁻¹

**Kriptovalyuta əlaqəsi (mövzu 6 diskussiyası):** Bitcoin/Ethereum ECDSA (EC üzərində DSA) ilə imza; wallet açarları açıq/gizli açar çiftdir. "Trezor/ledger" cihazları = gizli açar heç cihazdan çıxmır.

## 15. Elliptik əyri kriptoqrafiyası (ECC) — dərin analiz

### 15.1. Elliptik əyri

`y² = x³ + ax + b (mod p)` + sonsuzluq nöqtəsi ∞.
Weierstrass forması. Bitcoin/secp256k1: y² = x³ + 7. P-256 (NIST): a = −3.

**Nöqtə toplama qaydaları** (həndəsi + modul əritmətikası):

- P + Q: P və Q-nu birləşdirən xətt əyrini üçüncü nöqtədə kəsir → simmetriği (x,-y) = P+Q
- P + P (double): təngent xətti
- Formullar: λ = (y₂−y₁)/(x₂−x₁) mod p; x₃ = λ²−x₁−x₂; y₃ = λ(x₁−x₃)−y₁

### 15.2. Niyə ECC RSA-dan güclüdür?

| Təhlükəsizlik səviyyəsi | RSA açar | ECC açar |
| --- | --- | --- |
| 128 bit | 3072 bit | 256 bit |
| 256 bit | 15360 bit | 512 bit |

ECDLP (elliptik əyri diskret loqarifm) ən yaxşı üsul **Pollard's rho** = √(q) adım. 256-bit qrup → 2¹²⁸ əməliyyat = qeyri-mümkün. GNFS ECC-yə tətbiq edilə bilmir.

Nəticə: TLS-də `ECDHE` (ephemeral ECDH) standartdır — server sertifikatı yalnız imza/istifadəçi, sessiya açarı hər dəfə yeni ECDH ilə. **Forward secrecy** təmin edir!

### 15.3. ECDH və forward secrecy

Daimi açar sızsa, köhnə sessiyalar açılmır, çünki hər sessiyada yeni efemer açarlar istifadə olunur. TLS 1.3-də yalnız efemer DH (DHE/ECDHE) qalıb — RSA şifrələməsi tam söndürülüb!

---

# HİSSƏ IV — HASH FUNKSİYALARI, MAC, PAROL TƏHLÜKƏSİZLİYİ

## 16. Hash funksiyaları — daxili quruluş

### 16.1. Tələblər

1. **Preimage resistance:** h(x) verilib → x tapmaq qeyri-mümkün (2ⁿ əməliyyat)
2. **Second preimage:** x verilib → x' tap ki, h(x')=h(x) (2ⁿ)
3. **Collision resistance:** istənilən (x, x') cütü tap: h(x)=h(x') (yalnız 2^(n/2) — birthday paradox!)

> **Birthday paradox (sənin SWEET32 mövzunun riyası):** 23 nəfərin içində eyni ad-günü ehtimalı 50%-dən çoxdur. n-bit hash-də toqquşma 2^(n/2) yoxlamada gözlənilir: 128-bit hash → 2⁶⁴.

### 16.2. MD5 (1992, Ron Rivest) — içəridən xaricə

- Merkle-Damgård konstruksiyası: mesaj 512-bit bloklara bölünür, hər blok keçmiş state ilə kompressiya funksiyasına girir
- State: 4×32-bit (A,B,C,D); 64 əməliyyat, hər biri: `F` funksiyası + sürüşmə + əlavə + Kᵢ sabiti
- F funksiyaları 4 raundda dəyişir: F, G, H, I
- **Qırılma tarixi:**
- 2004: Wang et al. — real toqquşma nümunəsi (kompressiya funksiyasındakı zəiflik)
- 2007+: chosen-prefix collision: iki İSTƏNİLƏN fərqli PDF-yə eyni MD5 vermək mümkün oldu (Flame kiber silahı Microsoft sertifikatını belə saxtalaşdırdı — 2012)
- Hal-hazırda: yalnız checksum (səhv aşkarlama) kimi məqsədlər; təhlükəsizlikdə **QADAĞANDIR**

### 16.3. SHA-1 (1995) — NIST-in MD5 cavabı

- 160-bit çıxış, 80 raund. **2017: SHAttered (Google+ CWI)** — iki fərqli PDF, eyni SHA-1 (2⁶³ ≈ 1 il GPU vaxtı, $110k). Git, old TLS sertifikatları təsirləndi → artıq qadağan.

### 16.4. SHA-2 ailəsi (2001) — hazırkı iş atlısı

- **SHA-256:** 32-bit sözlər, 64 raund, 256-bit çıxış. Funksiyalar: Ch, Maj, Σ₀, Σ₁ (rotr + xor + add)
- SHA-512, SHA-224, SHA-384 variantları
- Hələlik praktik qırılma yoxdur (əməliyyat sayı 2^128 ətrafı)

### 16.5. SHA-3 / Keccak (2015) — sponge konstruksiyası

- **Merkle-Damgård yox, sponge:** state 1600 bit; absorb fəazı (mesaj bloku state-ə xor, sonra permutation f) + squeeze fəazı (çıxış hasil edir)
- f = Keccak-f[1600], 24 raund, xətti olmayan χ mapping
- SHAKE128/SHAKE256 — **XOF** (istənilən uzunluqda çıxış)
- Niyə yeni standart? SHA-2 hələ sağlam olsa da, "ehtiyat ehtiyatın ehtiyatıdır" — struktur cəhətdən tamamilə fərqli ailə

### 16.6. HMAC — hash-dən MAC yaratmaq

`HMAC(K, m) = H((K' ⊕ opad) ‖ H((K' ⊕ ipad) ‖ m))`

- ipad = 0x36..., opad = 0x5c... təkrarlanan sabitlər
- Niyə daxili+çöl iki hash? Tək H(K‖m) uzantı hücumuna açıqdır (merkle-damgard zəncirləmə xüsusiyyətindən istifadə); HMAC bunu qapadır
- İstifadə: API imzaları (webhook), TLS session MAC (köhnə versiyalarda), IPsec

## 17. Parol təhlükəsizliyi — tam nəzəri + praktik

### 17.1. Niyə hash-lənmiş parollar qırılır?

İnsan parolları təxminən 40-bit entropiyadan azdır. GPU (RTX 4090):

- MD5: ~150 milyard h/s; SHA-256: ~50 milyard h/s
- 8 simvol parol (a-z, A-Z, 0-9): 62⁸ = 2.2×10¹⁴ → MD5-də ~təqribən 25 dəqiqə!

### 17.2. Rainbow table hücumu

Hash → parol əks cədvəl, qabaqcadan hesablanmış (zəncir texnikası ilə sıxılmış). Salt rainbow table-i öldürür.

**Salt:** hər parola unikal təsadüfi dəyər: `h = H(password ‖ salt)`; salt hamıya açıq saxlanılır (qorunması lazım deyil — məqsədi cədvəli ləğv etməkdir).

### 17.3. Adaptiv hash alqoritmləri

| Alqoritm | Nə edir | Niyə güclüdür | Parametrlər |
| --- | --- | --- | --- |
| **PBKDF2** (2000) | Şifrəni çoxsaylı iterasiya ilə hash (HMAC-SHA) | Hər parol yoxlaması bahalı olur | iterations (10⁶+) |
| **bcrypt** (1999) | Blowfish şifrəsinin Eksbey variantı, daxili salt | Parametrli bahalı funksiya | cost (4–31, 2^cost raund) |
| **scrypt** (2009) | CPU + yaddaş-geniş (memory-hard) | GPU/ASIC faydasını azaldır | N, r, p |
| **Argon2** (2015, PHC qalibi) | yaddaş-geniş + paralellik + data-dependent | üç parametrlə tam tənzimlənmə | m (yaddaş KB), t (iter), p (thread) |

**Argon2id tövsiyəsi:** `m=64MB, t=3, p=4`. Parol qırma alətlərinin qarşısında Argon2id ən güclü müdafiədir.

### 17.4. Hashcat / John the Ripper praktikası (yalnız laboratoriya!)

```bash
# MD5 hash (mode 0) sözlük ilə
hashcat -m 0 hashlar.txt /usr/share/wordlists/rockyou.txt
# bcrypt (mode 3200) — yavaş olacaq
hashcat -m 3200 bcrypt_hash.txt rockyou.txt
# maska hücumu: 8 simvol, 1 hərf + 7 rəqəm
hashcat -m 0 hash.txt -a 3 '?u?d?d?d?d?d?d?d'
# John
john --wordlist=rockyou.txt --format=raw-md5 hash.txt
```

**Müdafiə nəticəsi:** 14+ simvol, passphrase, password manager, MFA.

## 18. Rəqəmsal imza — dərin

### 18.1. RSA imza

İmzala: s = H(m)ᵈ mod n. Yoxla: sᵉ ≡ H(m) (mod n).

- RSA-nın malleability-i burada faydalıdır: yalnız d sahibi imzalayır, hamı yoxlayır
- **hash-and-sign şərti:** hamı m-dən asılı olmalıdır; PKCS#1 v1.5 / PSS padding-ləri

### 18.2. DSA / ECDSA

- **DSA:** p, q, g = h^((p−1)/q); gizli x; açıq y = gˣ mod p. İmza (r, s) = (gᵏ mod p mod q, k⁻¹(H(m)+xr) mod q)
- **ECDSA:** eyni sxem elliptik əyridə: r = (k·G).x mod n; s = k⁻¹(h + dr) mod n. **Kritik: k təkrarlanmamalıdır!** PlayStation 3 hack (2010) və Bitcoin Android bug: eyni k → x (gizli açar) birbaşa hesablanır:
k = (h₁ − h₂)/(s₁ − s₂), sonra x = (s·k − h)/r
- **EdDSA (Ed25519):** deterministik nonce (RFC 8032) — təsadüfi k generatorunun sızmasını aradan qaldırır; sürətli, qısa imza (64 bayt)

### 18.3. İmza vs MAC — fərq

|  | MAC (HMAC) | Rəqəmsal imza |
| --- | --- | --- |
| Açar | Paylaşılan sirr | Açıq/gizli cüt |
| İnkaredilməzlik | YOX (hər iki tərəf yarada bilər) | VAR |
| Etibar modeli | Simmetrik | Asimmetrik (CA ilə) |

## 19. Sertifikatlar, X.509 və PKI — dərin

### 19.1. X.509 sertifikatının strukturu (v3)

- Version, Serial Number
- **Issuer:** CA-nın DN-i (distinguished name)
- **Subject:** sahibin DN-i (CN = example.com)
- SubjectPublicKeyInfo (açıq açar + alqoritm)
- **Validity:** notBefore / notAfter
- **Extensions:** SAN (Subject Alternative Name — real hostname), KeyUsage (digitalSignature, keyEncipherment...), ExtendedKeyUsage (serverAuth, clientAuth), BasicConstraints (CA:TRUE/FALSE), CRL DP, OCSP URL
- **Signature:** CA öz gizli açarı ilə imzaladı

### 19.2. Etibar zənciri (chain of trust)

Server sertifikatı → Intermediate CA(s) → **Root CA** (self-signed, brauzerdə quraşdırılmış).
Yoxlama: hər sertifikatın imzasını yuxarıdakının açıq açarı ilə yoxla → root-a qədər.

### 19.3. Sertifikatın həyat dövrü

1. Açar cütü yarat (CSR ilə: Certificate Signing Request — subject + açıq açar, öz gizli açarı ilə imzalanır)
2. CA-ya göndər (Web: ACME protokolu — Let's Encrypt)
3. CA kimliyi yoxlayır (DV: DNS-də TXT, HTTP-də fayl; OV/EV: sənədlə)
4. Sertifikat al, veb serverə quraşdır (nginx/apache)
5. **Ləğv:** CRL (Certificate Revocation List — imzalı cədvəl) və ya **OCSP** (onlayn sorğu) → brauzer "stapling" (OCSP stapling: server öz OCSP cavabını TLS handshake-ə əlavə edir)

### 19.4. Real hadisələr (case study: HTTPS mövzusu)

- **DigiNotar (2011):** Niderland CA hack olundu, google.com saxta sertifikatlar buraxıldı → CA işindən çıxarıldı, şirkət iflas etdi
- **Let's Encrypt (2015):** pulsuz, avtomatlaşdırılmış DV sertifikatları — HTTPS-in yayılmasında inqilab (Web-də HTTPS payı %30→%90+)

## 20. TLS / SSL — dərin protokol analizi (Real həyatda kriptoqrafiya mövzusu)

### 20.1. TLS 1.2 handshake (detallı)

```javascript
Client → Server: ClientHello (versiya, təsadüfi client_random, TƏKLİF EDİLƏN ŞİFR PAKETLƏRİ, SNI, uzantılar)
Server → Client: ServerHello (seçim: versiya, server_random, cipher_suite)
              + Certificate (zəncir)
              + ServerKeyExchange (DHE: p, g, gᵇ)
              + ServerHelloDone
Client:       verifikasiya edir; pre_master = DH açarı / RSA ilə şifrələnmiş gizli rəqəm
              master_secret = PRF(pre_master, "master secret", client_random+server_random)
              (PRF: HMAC-based key derivation)
              Derive: 6 açar → client_write_key, server_write_key, MAC açarları (və IV-lər)
Client → Server: ClientKeyExchange + [ChangeCipherSpec] + Finished (verify_data = PRF bütün handshake transcript)
Server → Client: [ChangeCipherSpec] + Finished
→ Tətbiq məlumatı: AES-GCM ilə şifrəli records (sequence number + nonce AEAD)
```

**Açar mənbəyi:** master_secret → key_block → hər istiqamət üçün açar+IV.

### 20.2. TLS 1.3 (2018) — inqilabi dəyişikliklər

- **Yalnız AEAD:** CBC, RC4, SHA-1, RSA şifrələməsi, 3DES — HAMISI SİLİNDİ
- **1-RTT** handshake (1.2-də 2-RTT); **0-RTT** resumption (təkrarlanan təhlükə: replay hücumu!)
- **Forward secrecy məcburi:** yalnız DHE/ECDHE
- **Şifr paketi sadələşməsi:** `TLS_AES_256_GCM_SHA384`, `TLS_CHACHA20_POLY1305_SHA256`, `TLS_AES_128_GCM_SHA256` — cəmi 3!
- Handshake-ın böyük hissəsi şifrəlidir (hücumçu serverin sertifikatını görmür)

### 20.3. Şifr paketi sintaksisi

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`

```javascript
KEX:        ECDHE  (açar mübadiləsi — forward secrecy)
AUTH:       RSA    (sertifikat imzası)
ENCRYPTION: AES_128_GCM
HASH:       SHA256 (PRF / HMAC)
```

### 20.4. Tarixi zəifliklər və hücumlar (hücumlar bölməsi ilə əlaqəli)

**🔴 POODLE (2014) — SSLv3 padding oracle (qeydlərində xüsusi qeyd!)**

- SSLv3-də CBC padding-i (0..7 bayt) formatı yoxlanılmır — hər hansı padding qəbul edilir
- Hücumçu: ictimai Wi-Fi-də xətti SSLv3-ə eniş (fallback) məcbur edir; hər dəfə son baytı təxmin edir (256 yoxlama ≤)
- Nəticə: sessiya cookie-si (məs. Gmail) oğurlanır
- **Müdafiə:** SSLv3 tam bağla (server + brauzer), TLS_FALLBACK_SCSV

**🔴 SWEET32 (2016) — 64-bit bloklu şifrələrin birthday hücumu (qeydlərində!)**

- 3DES (64-bit blok): ~32 GB (2³² blok) şifrəli trafikdə birthday paradox → toqluşma → hücumçu iki eyni şifrə blokunu tapır → XOR-layır → açıq mətn cütünün xoru (dəyişiklik etmir: struktur statistikası ilə mətn bərpa)
- Nümunə: TOR exit node-dan uzun müddətli HTTPS sessiyası
- **Müdafiə:** 3DES şifr paketlərini serverdən sil (sslscan ilə yoxla!)

**Digərləri:** BEAST (TLS1.0 CBC IV əlaqəsi — browser exploit), CRIME/BREACH (sıxılma + secret cookie), Heartbleed (2014, OpenSSL buffer over-read — yaddaşdan açar sızması, şifrə deyil realizasiya bug), Logjam/weak DH, ROBOT (RSA PKCS#1 v1.5 oracle)

---

# HİSSƏ V — ŞƏBƏKƏ TƏHLÜKƏSİZLİYİ ALƏTLƏRİ (QEYDLƏRİNİZDƏN)

## 21. Linux şəbəkə əsasları (qeydlərinizin ilk sətirləri)

### 21.1. `sudo su` — superuser rejimi

```bash
sudo su            # root shell-ə keçid (sənin şifrən ilə)
sudo su -          # login shell (root-un mühit dəyişənləri ilə)
whoami             # kim olduğunu yoxla
```

Niyə təhlükəsizlikdə lazımdır: port skanları (`ike-scan` hamısı raw socket istəyir), `tcpdump` interfeyslərin promiscuous rejiminə keçməli olur.

### 21.2. `ip route` — marşrut cədvəli

```bash
ip route           # bütün marşrutlar (qeydlərindəki sətir)
ip route show table main
default via 192.168.1.1 dev eth0 proto dhcp metric 100
```

- **default via X:** defolt gateway (bütün internet trafiki buradan)
- Hücumçu perspektividən: `ip route` + `ip neigh` (ARP cədvəli) ilə şəbəkə topologiyasını öyrənmək → MITM planlaşdırma (ARP spoofing üçün arpspoof)

## 22. sslscan — dərin istifadə

```bash
sudo apt install sslscan
sslscan --show-certificate kiberbulusmus.com.tr
sslscan --tlsall 92.115.23.23:443
sslscan --no-failed --xml=nəticə.xml host
```

**Nəyi analiz edirsən:**

1. **Protokollar:** SSLv2/SSLv3 (POODLE!)/TLS1.0/1.1 (köhnə)/1.2/1.3
2. **Şifr paketləri siyahısı:** ECB? CBC? 3DES (SWEET32!)? RC4? aşağı açar?
3. **Sertifikat:** issuer, expiry, signature alqoritmi (SHA-256?), key length (2048+?)
4. **Zəiflik bayraqları:** heartbeat, compression (CRIME təhlükəsi)

**Server hardening tövsiyələri (sslscan-ı təmiz görmək üçün):**

- Minimum TLS 1.2
- Yalnız `ECDHE+AESGCM`, `ECDHE+CHACHA20` (forward secrecy)
- HSTS header, OCSP stapling

## 23. OpenSSL ilə əllə yoxlama

```bash
openssl s_client -connect host:443 -tls1_2
openssl s_client -connect host:443 -cipher 'DES-CBC3-SHA'   # SWEET32 testi
openssl s_client -connect host:443 -ssl3                       # POODLE testi (refused olmalı)
openssl s_client -connect host:443 -servername host            # SNI ilə
```

## 24. Shodan — dərin axtarış (qeydlərinizdəki "city, Baku port :500")

Shodan — İnternetə qoşulmuş cihazların (server, kamera, router, SCADA) axtarış mühərrikidir. Google indeksləyir səhifələri; Shodan indeksləyir port/banner.

**Axtarış operatorları:**

```javascript
city:"Baku" country:"AZ" port:500          # Bakıda IKE (IPsec VPN) axtarışı
ssl.version:sslv3                          # hələ SSLv3 işlədənlər (POODLE hədəfi)
ssl.cert.subject.cn:"*.gov.az"             # domen sertifikatları
http.title:"ip camera"                     # IP kameralar
product:"OpenSSH" version:"7.4"            # köhnə SSH
vuln:CVE-2014-0160                         # Heartbleed-a həssaslar
org:"Aztelekom"                            # provayder üzrə
```

**Niyə UDP 500?** IKE/ISAKMP portu. `ike-scan` və Shodan birgə istifadə: Shodan ilə hədəfləri tap, `ike-scan` ilə konkret konfiqurasiyanı analiz et.

## 25. IPsec VPN və IKE — ƏN DƏRİN BÖLMƏ (qeydlərinizin əsası)

### 25.1. IPsec protokol stekinin ümumi görünüşü

```javascript
Tətbiq (IP paketi)
   ↓
[IPsec seçimləri:]
  AH  (Authentication Header)     → bütövlük + autentifikasiya, ŞİFRƏLƏMƏ YOX
  ESP (Encapsulating Security Payload) → şifrələmə + bütövlük (günlük standart)
   ↓
İki rejim:
  Transport mode  → yalnız IP payload qorunur (host-to-host)
  Tunnel mode     → bütün IP paketi yeni IP header-ə sarılır (gateway-to-gateway, VPN)
   ↓
İki əsas protokol:
  IKEv1 (ISAKMP)  → SA (Security Association) müqaviləsi — UDP 500 (+NAT-T UDP 4500)
  IKEv2 (2005)    → sadələşdirilmiş, daha təhlükəsiz, standart artıq
```

**SA (Security Association)** nədir? Tərəflərin razılaşdığı parametrlər paketi: şifrə alqoritmi (AES-CBC/CTR), hash (SHA-256), açar müddəti, DH qrupu, yaşam müddəti (lifetime).

### 25.2. IKEv1 iki fazası

- **Faza 1 (ISAKMP SA):** hər iki tərəfi autentifiksiya edir, mühafizə kanalı qurur (DH ilə)
- **Faza 2 (IPsec SA):** real trafik parametrlərini razılaşdırır (faza 1 tuneli içində)

### 25.3. Main Mode vs Aggressive Mode — PAKET-PAKET (qeydlərinizin mövzusu!)

**Main Mode (6 mesaj, 3 tur):**

```javascript
MM1: C→S: SA təklifi (şifrələr, hash-lər, DH qrupu)                    ─ şifrəsiz
MM2: S→C: seçilmiş SA + serverın sertifikatı/ID                        ─ şifrəsiz
MM3: C→S: DH açıq dəyəri gᵃ + nonce                                     ─ şifrəsiz
MM4: S→C: DH açıq dəyəri gᵇ + nonce                                     ─ şifrəsiz
MM5: C→S: [şifrəli] ID + autentifikasiya data (hash)                    ← IDENTİFİKASİYA ŞİFRƏLİ
MM6: S→C: [şifrəli] ID + autentifikasiya data
```

Autentifikasiya son turda, DH açarı ilə şifrələnmiş — hücumçu şəbəkədə ID görmür.

**Aggressive Mode (3 mesaj):**

```javascript
AM1: C→S: SA + DH gᵃ + nonce + ID (məs. FQDN: vpn.sirket.az) + vendor ID   ─ hamısı AÇIQ
AM2: S→C: SA + DH gᵇ + nonce + ID + AUTH (HASH = H(PSK | gᵃ | gᵇ | ...)) ─ AUTH AÇIQ ötürülür!
AM3: C→S: AUTH
```

### 25.4. PSK-nin ölümcül problemi (Aggressive Mode)

AM2-də AUTH hash açıqdır: `HASH = HMAC-MD5/SHA(PSK, gᵃ|gᵇ|cookie-lər|...)`.
Hücumçu (wifi-də/passive) bu paketi tutur → offline lüğət hücumu:

```javascript
hər kandidat PSK üçün HASH yenidən hesabla → uyğunluq tap
```

CPU ilə minlərlə/saniyə → zəif PSK (məs. "Cisco123") qırılır. **Main Mode-də bu mümkün deyil** (identifikasiya şifrəlidir). Buna görə qeydlərinizdəki təqdimat slaydı aggressive mode-u riskli adlandırır!

### 25.5. ike-scan — tam praktik

```bash
sudo apt install ike-scan
# 1) IKE hostlarının kəşfi (UDP 500)
sudo ike-scan -M 92.115.23.23
# 2) Aggressive mode handshake — PSK hash yaxalamaq
sudo ike-scan -A -M --id= vpn.sirket.az 92.115.23.23
# cavabda: "(PSK)" qeydi ilə hash
# 3) Hash-in qırılması
psk-crack -d /usr/share/wordlists/rockyou.txt tutulmus.psk
# və ya hashcat (mode 5400 = IKE-PSK MD5, 5300 = IKE-PSK SHA1)
hashcat -m 5400 ike_hash.txt rockyou.txt
# 4) Main mode cəhdi (əksər serverlər indi aggressive-i bağlayıb)
sudo ike-scan -M --main 92.115.23.23
# 5) Transförmatların brute-force (hansı şifrə/hash dəstəklənir)
sudo ike-scan -M --trans=5,2,1,2 92.115.23.23   # DES/MD5/PSK/q-2ci qrup
```

### 25.6. IKEv2 ilə müqayisə

- 4 mesaj (2-RTT), DoS əleyhinə cookie mexanizmi
- EAP inteqrasiyası (username/parol + MFA)
- Aggressive mode anlayışı YOXDUR → PSK offline hücumu yalnız zəif PSK + active cəhddən
- MOBIKE (mobility/multihoming)

### 25.7. VPN növləri ümumi (mövzu 11-12)

| VPN | Protokol | Port | Xüsusiyyət |
| --- | --- | --- | --- |
| IPsec site-to-site | ESP/IP | UDP 500/4500 | Standart korporativ, hardware sürətli |
| SSL/TLS VPN (OpenVPN) | TCP/UDP 1194 | sertifikat + username | NAT-dan keçir, asan konfiqurasiya |
| WireGuard | UDP 51820 | ~4 min sətir kod, ChaCha20-Poly1305 | müasir, sürətli, auditu asan |
| L2TP/IPsec | UDP 1701+500 | dubl qoruma (üstü IPsec) | köhnə Windows uyğunluğu |

## 26. Wireshark ilə trafik analizi (mövzu 10 — simmetrik açarla şifrəli trafik)

**Ssenari:** kriptograf hücumçu TLS trafikini tutur. Nə görür, nə görə bilmir?

```javascript
GÖRÜR:       IP ünvanları, portlar, DNS sorğuları (hələ şifrəsizdirsə), 
             paket ölçüləri/zamanı (trafik analizi — hansı səhifə baxılır), 
             TLS handshake-in İLK hissəsi (ClientHello, SNI!, sertifikat)
GÖRƏ BİLMİR:  məzmun, URL-lər (TLS 1.3-də hətta SNI belə gizlədilə bilər — ECH)
```

**TLS açar materialını dekript etmək** (yalnız öz sistemin / forensic):

```bash
export SSLKEYLOGFILE=~/tls.keys    # Firefox/Chrome-a əvvəlcədən göstər
wireshark → Preferences → Protocols → TLS → (Pre)-Master-Secret log filename
```

**IPsec-i dekript etmək:** IKE SA açarlarını Wireshark Preferences → ISAKMP-a daxil etmək (yalnız laboratoriya, ike-scan deyil, hədəfin öz logları ilə).

---

# HİSSƏ VI — PRAKTİK PROQRAMLAŞDIRMA (Python ilə tam nümunələr)

## 27. Python praktiki — dərin nümunələr

### 27.1. Sezar — brute-force qırıcı (freq. analizli)

```python
def sezar_desifrele(c, k):
    return "".join(chr((ord(ch)-ord('A')-k)%26+ord('A')) if ch.isupper()
                   else chr((ord(ch)-ord('a')-k)%26+ord('a')) if ch.islower() else ch
                   for ch in c)

sifre = "WKH HDJOH KDV ODQGHG"   # the eagle has landed, k=3
for k in range(26):
    print(k, sezar_desifrele(sifre, k))
# 26 variantdan yalnız biri İngilis kimi oxunur
```

### 27.2. Vigenère — Kasiski həyata keçirən tam skript

```python
from collections import Counter
import math

def kasiski(sifre, uzunluq=3):
    tekrarlar = {}
    for i in range(len(sifre)-uzunluq):
        fraq = sifre[i:i+uzunluq]
        for j in range(i+1, len(sifre)-uzunluq):
            if sifre[j:j+uzunluq] == fraq:
                mesafe = j - i
                tekrarlar[fraq] = tekrarlar.get(fraq, []) + [mesafe]
    eboblar = [math.gcd(*v) for v in tekrarlar.values() if len(v) > 0]
    return Counter(eboblar).most_common(1)[0][0] if eboblar else 1

def tezlik_kir(sifre, m):
    achar = []
    az = [9.4, 1.4, 3.3, 3.0, 0.4, 2.4, 2.6, 1.8, 8.2, 0.0, 1.0, 4.3, 5.1, 7.2, 5.9, 2.9, 3.1, 5.1, 7.3, 5.4, 4.6, 3.3, 0.1, 0.5, 1.3, 0.7]  # AZ tezlikləri
    en = [8.2, 1.5, 2.8, 4.3, 12.7, 2.2, 2.0, 6.1, 7.0, 0.2, 0.8, 4.0, 2.4, 6.7, 7.5, 1.9, 0.1, 6.0, 6.3, 9.1, 2.8, 1.0, 2.4, 0.2, 2.0, 0.1]
    for i in range(m):
        sutun = sifre[i::m]
        en_iyi_k, en_iyi_x = 0, -1
        for k in range(26):
            kaydirilmis = sum(en[(ord(ch)-ord('a')-k)%26] for ch in sutun if ch.isalpha())
            if kaydirilmis > en_iyi_x: en_iyi_x, en_iyi_k = kaydirilmis, k
        achar.append(en_iyi_k)
    return achar
```

### 27.3. AES ECB "pinguin problemi" (GCM ilə müqayisə)

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives import padding
import os

açar = os.urandom(16); iv = os.urandom(16)

# ECB — eyni blok eyni şifrə
ecb = Cipher(algorithms.AES(açar), modes.ECB()).encryptor()
# GCM — AEAD, həm bütövlük
gcm = Cipher(algorithms.AES(açar), modes.GCM(iv)).encryptor()
gcm_aad = b"header"
gcm.authenticate_additional_data(gcm_aad)
şifrə = gcm.update(b"Gizli melumat") + gcm.finalize()
tag = gcm.tag   # bütövlük etiketi — bunu saxlamaq lazımdır!
```

### 27.4. ChaCha20-Poly1305

```python
from cryptography.hazmat.primitives.ciphers.aead import ChaCha20Poly1305
açar = ChaCha20Poly1305.generate_key()
aead = ChaCha20Poly1305(açar)
nonce = os.urandom(12)
şifrə = aead.encrypt(nonce, b"melumat", b"aad")
assert aead.decrypt(nonce, şifrə, b"aad") == b"melumat"
```

### 27.5. RSA ilə tam dövrə

```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes

gizli = rsa.generate_private_key(public_exponent=65537, key_size=2048)
açıq = gizli.public_key()

şifrə = açıq.encrypt(b"sirli mesaj",
    padding.OAEP(mgf=padding.MGF1(algorithm=hashes.SHA256()),
                 algorithm=hashes.SHA256(), label=None))
mətn = gizli.decrypt(şifrə, padding.OAEP(mgf=padding.MGF1(hashes.SHA256()),
                                         algorithm=hashes.SHA256(), label=None))

# İmza (Ed25519 daha tövsiyə olunur)
imza = gizli.sign(b"sənəd", padding.PSS(mgf=padding.MGF1(hashes.SHA256()),
                  salt_length=padding.PSS.MAX_LENGTH), hashes.SHA256())
açıq.verify(imza, b"sənəd",
            padding.PSS(mgf=padding.MGF1(hashes.SHA256()),
                        salt_length=padding.PSS.MAX_LENGTH), hashes.SHA256())
```

### 27.6. Diffie-Hellman simulyasiyası + MITM göstərici

```python
from cryptography.hazmat.primitives.asymmetric import dh
parameters = dh.generate_parameters(generator=2, key_size=2048)

alice_gizli = parameters.generate_private_key()
bob_gizli   = parameters.generate_private_key()
# Açıq kanal:
alice_açıq = alice_gizli.public_key(); bob_açıq = bob_gizli.public_key()
# Ortaq açar:
alice_ortaq = alice_gizli.exchange(bob_açıq)
bob_ortaq   = bob_gizli.exchange(alice_açıq)
assert alice_ortaq == bob_ortaq   # MITM yoxdursa bərabərdir!
```

> **MITM simulyasiyası tapşırığı:** mallory_adlı iki cüt açar əlavə edin — alice-mallory-bob xəttində mallory hər tərəflə ayrıca müqavilə qurur, mesajları oxuyub ötürür. Sertifikatlı autentifikasiya (TLS) bunu bağlayır.

---

# HİSSƏ VII — MÜASİR İSTİQAMƏTLƏR

## 28. Blockchain və kriptoqrafiya (mövzu 11-12)

- **Blok strukturu:** blok header-i = (öncəki blokun hash-i ‖ timestamp ‖ Merkle root ‖ nonce) → hər blok öncəkinə bağlıdır → bir blok dəyişsə, sonrakı bütün hash-lər pozulur
- **Merkle ağacı:** minlərlə əməliyyatı bir 256-bit root-da cəmləyir — SPV (yüngül klient) tək yarpağı isbatla yoxlayır
- **İmza:** Bitcoin ECDSA/secp256k1; Ethereum indi BLS (aggregate imza)
- **Mining = hash puzzle:** nonce tap ki, hash < hədəf. İşin sübutu (PoW)
- **Təhlükəsizlik müzakirəsi (mövzu 6):** 51% hücumu, açar sızması (Mt. Gox), smart contract bug-lar, ECDSA nonce təkrarı

## 29. Post-kvant kriptoqrafiya (mövzu 13 + post-quantum diskussiyası)

### 29.1. Kvant hücumlarının xəritəsi

| Alqoritm | Kvant hücumu | Nəticə |
| --- | --- | --- |
| RSA, DH, ECC, DSA | **Shor alqoritmi** (1994): period tapma | 💀 Tam qırılır — qeyri-müəyyən dövrdə |
| AES-256, SHA-256 | **Grover** (1996): kvadratik sürətlənmə | ⚠️ Effektiv 128→64 bit? Xeyr: AES-256 → 128 bit qalır — təhlükəsiz |

### 29.2. Shor niyə öldürücüdür?

Faktorizasiya və DLP həm period/ordering problemə reduksiya olunur; kvant Fourier transformu periodu efektiv tapır. 4000+ stabil kubit lazımdır (indiki IBM ~1000 qubit 2026) — **"kriptoqrafik əhəmiyyətli" kvant kompüteri hələ yoxdur, amma "harvest now, decrypt later" təhlükəsi REALDIR** (bu gün yığılan şifrəli arxivlər gələcəkdə açıla bilər!).

### 29.3. NIST PQC standartları (2024 təsdiqi)

| Standart | Əsas | Vəzifə |
| --- | --- | --- |
| **FIPS 203 ML-KEM** (CRYSTALS-Kyber) | Lattice (modul qırışıqlıq üzərində) | Açar mübadiləsi/şifrələmə |
| **FIPS 204 ML-DSA** (CRYSTALS-Dilithium) | Lattice | Rəqəmsal imza |
| **FIPS 205 SLH-DSA** (SPHINCS+) | Hash əsasında | Ehtiyat imza (stateless) |

### 29.4. Lattice qısa izah

LWE (Learning With Errors): A·s + e (mod q) — e kiçik səhv; s-i e olmadan tapmaq ağırdır. Kyber-də ring-LWE: polinomlar üzərində — sürətli (NTT transform). İmza: Fiat-Shamir + lüstruktur.
Müqayisə: RSA-2048 imza ~256 bayt; Dilithium-2: ~2.4 KB (qəbul edilə bilən trade-off).

## 30. Steqanoqrafiya və məlumat gizlətmə (mövzu 14-15)

### 30.1. LSB (Least Significant Bit)

24-bit BMP-də hər piksel 3 bayt (R,G,B). Hər baytın son bitini dəyişmək: rəng fərqi insan gözü tərəfindən hiss edilməz (256 səviyyədən 1).

```javascript
1 şəkil (1024×768×3) = 2.3 Mbayt → ~2.3 Mbit gizli mətn = ~280 KB!
```

Python ilə:

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
            b = (b & 0xFE) | int(bitler[i])   # son biti əvəz et
            pixels[x, y] = (r, g, b)
            i += 1
img.save("stego.bmp")
```

**Aşkarlanma:** chi-square testi, histogram analizi (LSB-lər təsadüfi olur — təbiи şəkillərdə LSB-lər yarı-yarıya 0/1-dir, LSB mesajda eyni paylanır), stegdetect/stegsolve alətləri.

### 30.2. Digər üsullar

- **Metadata:** EXIF JPEG, ID3v2 MP3 (steghide: JPEG-də DCT koefisientlərində gizlətmə)
- **Audio:** LSB, faz modulyasiyası, echo hiding
- **Video:** hər N-ci framdə LSB
- **Network:** paket ölçüsü/zamanı ilə məlumat (covert channels — qeydlərdəki Wireshark mövzusunun qaranlıq tərəfi)
- **Stego-malware:** komanda-serverlə LSB şəkillərində əlaqə (C&C); exfiltrasiya

### 30.3. Steqanoqrafiya vs Kriptoqrafiya

Kriptoqrafiya: məzmunu gizlədir, VARLIĞI açıqdır. Steqanoqrafiya: VARLIĞI gizlədir. İdeal: ikisi birgə (gizlədilmiş + şifrəli mesaj).

## 31. QR kod + kriptoqrafiya (mövzu 14)

- QR strukturu: format info (BCH kodlaşdırma, Reed-Solomon — SƏHV DÜZƏLTMƏ, təhlükəsizlik deyil!), data codewords
- **Wi-Fi QR:** `WIFI:T:WPA;S:SSID;P:parol;;` — parol AÇIQ mətnlədir! → həssas nöqtələr
- **Təhlükəsiz yanaşma:** QR yalnız URL göstərir → HTTPS + qısa ömürlü token + imzalı link
- **QR phishing (quishing):** linki görmədən açmaq — həll: URL preview, imzalı dərsliklər

## 32. CTF platformaları — öyrənmə yolu (mövzu 7, 8, 15)

| Platforma | Xüsusiyyət | Hansı mövzular |
| --- | --- | --- |
| **cryptohack.org** | İnteraktiv kriptoqrafiya müsabiqəsi | XOR, RSA, ECC, AES — qeydlərindəki "cryptohub" |
| **picoCTF** | Məktəb/yeni başlayanlar üçün | hər kateqoriya, addım-addım |
| **CryptoHack / CTFtime** | Yarış təqvimi | kripto kateqoriyalı CTF-lər |
| **RootMe / HackTheBox** | Praktik laboratoriya | şəbəkə + kripto |

**Mini-CTF planı (mövzu 15):** 1) Sezar+Vigenère zənciri; 2) RSA zəif açar (e=3, kiçik m); 3) ECB piksel tapşırığı; 4) PNG LSB gizli mesaj; 5) picoCTF-dən kripto tag-li 3 sual.

---

# HİSSƏ VIII — YEKUN: YADDA SAXLANMALI 25 FAKT

1. Təhlükəsizlik açardadır, alqoritmdə deyil (Kerckhoffs)
2. Şannon: difuziya + konfüzyon
3. OTP = yeganə məhzəmsiz sistem; şərt: açar ≥ mətn, təsadüfi, təkrarsız
4. Sezar: 25 açar; Vigenère: period analizi (Kasiski) ilə qırılır
5. DES 56 bit — 1998-də hardware ilə qırıldı
6. Feistel şəbəkəsi: eyni sxemlə şifrə+deşifrə (açarlar tərs sırayla)
7. AES: SubBytes/ShiftRows/MixColumns/AddRoundKey; 10/12/14 raund
8. ECB eyni blok=eyni şifrə → siluet qalır → işlətmə!
9. CBC: padding oracle; CTR: nonce təkrarı fəlakət; GCM: AEAD qızıl standart
10. RC4: ilk bayt yönəlmələri + IV açar birləşməsi → ölü
11. ChaCha20: ARX (add-rotate-xor), 20 raund, Poly1305 = AEAD; mobil TLS-də AES-dən sürətli
12. RSA: n=pq; ed≡1 mod φ(n); OAEP mütləq; 2048 bit minimum
13. DH/ECC-də DLP/ECDLP → qeyri-mümkün; ECC 256 bit ≈ RSA 3072
14. Forward secrecy = efemer DHE/ECDHE (TLS 1.3-də məcburi)
15. MD5 — toqquşmalar (Flame!); SHA-1 — SHAttered; SHA-256/SHA-3 işlək
16. HMAC = daxili+çöl hash (uzantı hücumuna qarşı)
17. bcrypt/scrypt/Argon2id parollar üçün; düz SHA yox; salt + pepper
18. ECDSA: k təkrarı = gizli açarın ölümü (PS3 hack)
19. X.509: CA imzalı açıq açar; SAN-u hostname-ə bax; OCSP stapling
20. TLS 1.3: 1-RTT, 0-RTT (replay riski), yalnız AEAD + PFS
21. POODLE = SSLv3 padding oracle; SWEET32 = 64-bit blok birthday (32GB) → 3DES öldü
22. IKE Aggressive mode PSK hash-i açıq verir → ike-scan -A + psk-crack
23. IKEv2 > IKEv1; Main > Aggressive; WireGuard (ChaCha20) müasir seçim
24. Shor RSA/DH/ECC-i öldürür; Kyber/Dilithium standartlaşdı; AES-256 Grover-dən sağ çıxır
25. Steqanoqrafiya varlığı gizlədir, kriptoqrafiya məzmunu; LSB chi-square ilə aşkarlanır

---

# HİSSƏ IX — İMTAHAN ÜÇÜN 30 SUAL (çətindən asanaya)

**A. Nəzəri (qısa cavablı)**

1. Kerckhoffs prinsipini formallaşdırın və niyə "təhlükəsizlik obfuscation-dadır" yanlışdır?
2. Shannon difuziya/konfüzyon tərifləyin və AES-də hansı əməliyyat hansına uyğundur?
3. Ciphertext-only hücum modelində hücumçu nə bilir? OTP niyə buna davamlıdır?
4. Sezarın açar fəzası nə qədərdir? Affine şifrəsinin açar fəzası 312 olmasına baxmayaraq niyə hələ də zəifdir?
5. Kasiski metodunda periodun ƏBOB ilə tapılmasının məntiqini izah edin.
6. Vigenère-ə qarşı Friedman testi (IC) necə işləyir? IC dəyərlərini yazın.
7. Hill şifrəsində açar matrisinə hansı şərt qoyulur və niyə?
8. OTP-nin üç şərti və təkrarlanan açarın niyə fəlakət olduğunu `C₁⊕C₂ = P₁⊕P₂` ilə göstərin.
9. DES-in Feistel şəbəkəsində deşifrələmənin niyə eyni sxemlə mümkün olduğunu riyazi yazın.
10. DES-də expansion, S-box, P permutasiyanın rolu nədir? S-box niyə qeyri-xətti olmalıdır?
11. AES-in bir raundunu addım-addım yazın. MixColumns-dakı GF(2⁸) polinomunu yazın.
12. AES-ə qarşı biclique hücumunun nəticəsi və niyə praktik olmadığını yazın.
13. CBC, CTR, GCM arasındakı paralellik, IV/nonce tələbləri və tipik hücumları müqayisə edin.
14. PKCS#7 padding nədir? Padding oracle hücumunun mexanizmi necədir?
15. RC4-ün KSA və PRGA fazalarını yazın. WEP-də IV problemini izah edin.
16. ChaCha20-nin state quruluşunu (sözlərin mənbəyini) və quarter round-u yazın. Niyə S-boxsuz dizayn cache-timing-ə davamlıdır?
17. RSA açar generasiyasını və düzgünlüyün Euler teoremi ilə isbatını yazın.
18. Textbook RSA-nın 3 zəifliyi (determinizm, malleability, küçük mesaj) və OAEP-in hər birini necə bağladığını yazın.
19. Diffie-Hellman-i qeyd-kağız üzərində iki nəfər arasında protokol kimi yazın; MITM hücumunu arxaqadında göstərin.
20. ECC-də nöqtə toplama düsturlarını yazın. Niyə ECC 256 bit ≈ RSA 3072 bit verir?
21. MD5-in Merkle-Damgård strukturunu və kompressiya funksiyasında zəifliyin nəticəsini yazın.
22. HMAC-ın iki-roundlu quruluşunun (ipad/opad) niyə vacib olduğunu izah edin.
23. Rainbow table necə işləyir? Salt onu niyə öldürür? Argon2id-in memory-hard parametrlərinin GPU əleyhinə effekti nədir?
24. ECDSA imza düsturlarını yazın; k sızmasının açarı necə ifşa etdiyini `k = (h₁−h₂)/(s₁−s₂)` ilə göstərin.
25. X.509 sertifikatında SAN, KeyUsage, BasicConstraints sahələri nə üçündür? Misconfiguration nəticəsi?
26. TLS 1.2 və 1.3 handshakelerini müqayisə edin: RTT, alqoritm dəsti, 0-RTT riski.
27. POODLE hücum addımlarını (downgrade + oracle + bayt bayt sızma) yazın.
28. SWEET32-də birthday paradoxun rolu: 64-bit blokda 2^32 blokdan sonra nə baş verir?
29. IKE Main və Aggressive mode-un mesaj axınını yazın; Aggressive-də PSK hash-inin niyə offline qırıla bildiyini göstərin.
30. Shor və Grover alqoritmlərinin effekti; indiki hibrid yanaşma (X25519+Kyber) nədir?

**B. Praktik tapşırıqlar**

31. `sezar_desifrele` funksiyası ilə şifrəni qırın: `ESP BFTNVMCZHY` (tip: İngilis)
32. picoCTF-də "Mod 26" və "Easy1" tapşırıqlarını həll edin, həll yolunuzu yazın.
33. Öz localhost-da `openssl s_server` qaldırın və `openssl s_client` ilə TLS 1.3 əlaqəsi qurun; master secret-in yaranma yerini izah edin.
34. `hashcat -m 0` ilə MD5 hash `5f4dcc3b5aa765d61d8327deb882cf99`-u rockyou ilə qırın (parol "password"dir — prosesi göstərin).
35. Python ilə DH simulyasiyasına MITM (mallory) əlavə edin; TLS-in bunu necə bağladığını bir cümlə ilə yazın.

---

# Əlavə: Oxu siyahısı

- Bruce Schneier — "Applied Cryptography" (20 yaşlı, amma intuisiya üçün əla)
- Jean-Philippe Aumasson — "Serious Cryptography" (müasir, tövsiyə #1)
- Dan Boneh & Victor Shoup — "A Graduate Course in Applied Cryptography" (pulsuz PDF)
- NIST FIPS 197 (AES), FIPS 203/204/205 (PQC)
- RFC 8446 (TLS 1.3), RFC 7296 (IKEv2), RFC 4301 (IPsec)
- cryptohack.org — məsələlər

*Hazırladı: təqdimat və şəxsi qeydlər əsasında. Uğurlar!*
