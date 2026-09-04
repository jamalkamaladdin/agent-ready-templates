# Sayt köçürmə planı

| | |
|---|---|
| Sayt | |
| Köhnə hostinq | |
| Yeni hostinq | |
| Planlanan keçid tarixi və saatı | |
| Gözlənilən dayanma müddəti | |
| Köçürməyə cavabdeh | |
| Geri dönüş qərarını verən | |

Keçid gecə yarısı deyil, komandanın oyaq olduğu saatda edilir. Cümə axşamı
səhər yaxşı vaxtdır: problem çıxsa, düzəltməyə iki iş günü qalır.

## 1. İnventar: nə köçür

Köçürülməyən şey yalnız burada yazılmayan şey olur.

| Nə | Harada | Ölçü / say | Kim məsuldur |
|---|---|---|---|
| Fayllar (kod, yüklənmiş media) | | | |
| Verilənlər bazası | | | |
| E-poçt qutuları | | | |
| DNS qeydləri | | | |
| SSL sertifikatı | | | |
| Cron tapşırıqları | | | |
| Mühit dəyişənləri və açarlar | | | |
| Üçüncü tərəf webhook-ları | | | |

Ayrıca yaz: hansı xarici servis saytın IP ünvanını tanıyır (ödəniş sistemi,
SMS provayderi, API tərəfdaşı). Onların ağ siyahısı yeni IP ilə əvvəlcədən
yenilənməlidir.

## 2. Versiyaların uyğunluğu

Köçürmələrin çoxu fayl itkisindən yox, versiya fərqindən sınır.

| | Köhnə server | Yeni server |
|---|---|---|
| Dil / runtime versiyası | | |
| Verilənlər bazası versiyası | | |
| Veb server | | |
| Genişlənmələr və modullar | | |
| Vaxt zonası və kodlaşdırma | | |

Fərq varsa, köçürmədən əvvəl həll olunur.

## 3. İki gün əvvəl: TTL azaldılması

DNS qeydinin TTL-i nə qədərdirsə, keçiddən sonra köhnə ünvan o qədər müddət
yaşamağa davam edir. Ona görə keçiddən 24-48 saat əvvəl TTL 300 saniyəyə
endirilir, keçiddən bir gün sonra isə geri qaldırılır.

~~~bash
# Cari TTL və dəyər
dig +noall +answer domen.az A
dig +noall +answer www.domen.az
~~~

- [ ] A qeydinin TTL-i 300 edildi
- [ ] www qeydinin TTL-i 300 edildi
- [ ] Dəyişiklik yayılıb (köhnə TTL qədər gözlənilib)

## 4. Sınaq köçürməsi

Yeni server real trafik almadan tam işlək vəziyyətə gətirilir və brauzerdən
yalnız hosts faylı ilə yoxlanılır.

~~~bash
# Linux/macOS: /etc/hosts     Windows: C:\Windows\System32\drivers\etc\hosts
203.0.113.10  domen.az www.domen.az
~~~

- [ ] Fayllar köçürülüb
- [ ] Baza köçürülüb və sayt onunla açılır
- [ ] Konfiqurasiya və mühit dəyişənləri qurulub
- [ ] Sayt bütün əsas səhifələrdə açılır
- [ ] Admin panelinə giriş işləyir
- [ ] Forma göndərilir, məktub gəlir
- [ ] Ödəniş varsa test rejimində keçib
- [ ] Cron tapşırıqları işə düşüb
- [ ] Log-larda 500 xətası yoxdur

Sınaq bitəndən sonra hosts sətri silinir. Unudulsa, sonrakı yoxlamaların
hamısı yanlış nəticə verir.

## 5. Dondurma pəncərəsi

Keçiddən əvvəl məzmun dəyişikliyi dayandırılır. Səbəb sadədir: köhnə saytda
yazılan yazı yeni bazaya düşmür və sonra əl ilə axtarılır.

| | |
|---|---|
| Dondurma başlayır | |
| Kim xəbərdar edilib | |
| Sifariş və qeydiyyat qəbulu | dayandırılır / davam edir |

## 6. Yekun köçürmə

Sınaqdan sonra dəyişən yalnız məlumatdır, ona görə yalnız o təzələnir.

~~~bash
# Fayllar (yalnız fərq köçürülür)
rsync -az --exclude=".git" ./ istifadeci@yeni-server:/var/www/sayt/

# PostgreSQL
pg_dump --no-owner --format=custom -d baza > baza.dump
pg_restore --no-owner -d baza baza.dump

# MySQL / MariaDB
mysqldump --single-transaction --default-character-set=utf8mb4 baza > baza.sql
mysql --default-character-set=utf8mb4 baza < baza.sql
~~~

- [ ] Baza yenidən köçürüldü, sətir sayı iki tərəfdə uyğundur
- [ ] Yüklənmiş fayllar sinxronlaşdırıldı
- [ ] Sayt hosts ilə bir daha yoxlanıldı

## 7. Keçid: DNS

| Tip | Ad | Köhnə dəyər | Yeni dəyər | Dəyişir? |
|---|---|---|---|---|
| A | @ | | | bəli |
| A / CNAME | www | | | bəli |
| MX | @ | | | **xeyr** |
| TXT | @ (SPF) | | | **xeyr** |
| TXT | \_dmarc | | | **xeyr** |
| CNAME | digər alt domenlər | | | |

Poçt saytla eyni serverdə deyilsə, MX və SPF qeydlərinə toxunulmur. Köçürmə
zamanı itən poçt ən gec bilinən və ən çətin bərpa olunan itkidir.

- [ ] A qeydi dəyişdirildi (saat: ____)
- [ ] www qeydi dəyişdirildi
- [ ] MX və TXT qeydləri olduğu kimi qaldı
- [ ] Yeni serverdə sertifikat alındı və https açılır

## 8. Keçiddən sonrakı ilk 30 dəqiqə

~~~bash
# Hansı IP cavab verir
dig +short domen.az @1.1.1.1
dig +short domen.az @8.8.8.8

# Sayt və başlıqlar
curl -sI https://domen.az
curl -s -o /dev/null -w "%{http_code} %{time_total}s\n" https://domen.az
~~~

- [ ] Sayt yeni serverdən açılır
- [ ] http ünvanı https-ə yönləndirilir
- [ ] Sertifikat etibarlıdır və domen adına uyğundur
- [ ] Şəkillər və stillər yüklənir (qarışıq məzmun xəbərdarlığı yoxdur)
- [ ] Forma göndərildi, məktub gəldi
- [ ] Server log-unda 500 və 404 axını yoxdur
- [ ] Analitikada canlı ziyarət görünür
- [ ] Search Console-da yeni xəta yığını yoxdur

## 9. Geri dönüş planı

Qərar hissə görə yox, şərtə görə verilir. Şərt əvvəlcədən yazılır:

> Keçiddən sonra ____ dəqiqə ərzində sayt açılmırsa, ödəniş və ya forma
> işləmirsə, geri dönülür.

Geri dönüş addımları:

1. DNS qeydi köhnə IP ünvanına qaytarılır (TTL hələ 300-dür, yayılma sürətlidir).
2. Köhnə serverdə sayt yenə işlək vəziyyətdədir, dondurma qaydasının səbəbi budur.
3. Keçid müddətində yeni bazaya düşən yazı və sifarişlər siyahı halında saxlanılır.
4. Səbəb yazılır, plan düzəldilir, yeni tarix təyin edilir.

- [ ] Köhnə serverin IP-si və giriş məlumatı əlçatandır
- [ ] Köhnə serverin son ehtiyat nüsxəsi alınıb
- [ ] Geri dönüş qərarını kimin verdiyi bəllidir

## 10. Bir həftə sonra

- [ ] Analitikada trafik keçiddən əvvəlki səviyyədədir
- [ ] Search Console-da indekslənmə xətası artmayıb
- [ ] TTL normal dəyərinə qaytarıldı (3600 və ya daha çox)
- [ ] Ehtiyat nüsxə yeni serverdə qurulub və bir dəfə sınaqdan keçirilib
- [ ] Monitorinq yeni serverə baxır
- [ ] Köhnə serverin bağlanma tarixi təyin edilib: ____
- [ ] Köhnə serverin son nüsxəsi ayrıca saxlanılıb

Köhnə server ən azı 30 gün saxlanılır. Bu müddətdə çatışmayan bir fayl
tapılırsa, onu geri gətirmək bir dəqiqəlik işdir.

## Nəticə

| | |
|---|---|
| Faktiki keçid vaxtı | |
| Faktiki dayanma müddəti | |
| Geri dönüldü? | bəli / xeyr |
| Sonradan çıxan problemlər | |
