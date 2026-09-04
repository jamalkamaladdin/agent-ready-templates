# Sayt qəbulu — çek-list

| | |
|---|---|
| Sayt | |
| Təhvil verən | |
| Təhvil alan | |
| Tarix | |

Qayda: bir bənd «bəli» deyilsə, o bənd təhvildən əvvəl bağlanır. Yoxlanmayan
bəndin qarşısına «yoxlanmadı» yazılır — boş buraxılmır.

## 1. Domen və sertifikat

- [ ] Domen sifarişçinin adına və onun hesabındadır (icraçının hesabında yox)
- [ ] Domenin bitmə tarixi bilinir və təqvimə yazılıb
- [ ] Domen avtomatik yenilənməyə qoyulub
- [ ] Sayt https ilə açılır, brauzer xəbərdarlıq göstərmir
- [ ] http ünvanı avtomatik https-ə yönləndirilir
- [ ] www və www-suz variantlardan biri digərinə yönləndirilir (ikisi ayrıca açılmır)
- [ ] Sertifikatın avtomatik yenilənməsi qurulub, bitmə tarixi bilinir
- [ ] DNS qeydlərinin harada idarə olunduğu və girişin kimdə olduğu yazılıb

## 2. Hostinq və giriş

- [ ] Hostinq/server hesabı sifarişçinin adınadır
- [ ] Bütün girişlər (panel, FTP/SSH, baza) təhvil verilib
- [ ] Parollar açıq mesajla yox, parol menecerində və ya bağlı kanalla ötürülüb
- [ ] İcraçının müvəqqəti girişləri ya bağlanıb, ya da müddəti razılaşdırılıb
- [ ] Mənbə kodu sifarişçinin çata bildiyi repozitoriyadadır

## 3. Ehtiyat nüsxə

- [ ] Ehtiyat nüsxə avtomatik alınır — tezliyi: ____
- [ ] Nüsxə saytın özündən başqa yerdə saxlanılır
- [ ] Neçə gün saxlanıldığı bilinir: ____
- [ ] Bərpa bir dəfə sınaqdan keçirilib (nüsxə var deyil — nüsxədən sayt qalxdı)
- [ ] Bərpa qaydası bir səhifə mətnlə yazılıb

## 4. Admin paneli

- [ ] Admin ünvanı və girişi verilib, sifarişçi özü daxil ola bilir
- [ ] İcraçının admin hesabı ya silinib, ya da razılaşdırılıb
- [ ] Adminin parolu güclüdür və ilk girişdə dəyişdirilib
- [ ] Mümkündürsə iki mərhələli giriş aktivdir
- [ ] Sifarişçi əsas əməliyyatları özü edə bilir: yazı əlavə etmək, şəkil dəyişmək, məzmun redaktə etmək
- [ ] Bunlar canlı nümayiş edilib, sadəcə danışılmayıb

## 5. Sürət

- [ ] PageSpeed Insights mobil nəticəsi yoxlanılıb — bal: ____
- [ ] Ən ağır səhifə mobil bağlantıda 3 saniyəyə açılır
- [ ] Şəkillər sıxılıb və müasir formatdadır
- [ ] Şəkillərin ölçüsü göstərilib (səhifə yüklənərkən məzmun sıçramır)
- [ ] Keş başlıqları qurulub, təkrar giriş birincidən sürətlidir

## 6. Mobil və brauzerlər

- [ ] 360px enində üfüqi sürüşdürmə yoxdur
- [ ] Mətn böyütmədən oxunur, düymələr barmaqla rahat basılır
- [ ] Menyu telefonda açılır və bağlanır
- [ ] Cədvəl və şəkillər ekrandan kənara çıxmır
- [ ] Chrome, Safari, Firefox və Edge-in son versiyalarında baxılıb
- [ ] iPhone-da Safari-də ayrıca yoxlanılıb

## 7. SEO əsasları

- [ ] Hər səhifənin öz başlığı və təsviri var, təkrarlanmır
- [ ] Hər səhifədə bir dənə h1 başlığı var
- [ ] Şəkillərin alt mətni yazılıb
- [ ] robots.txt açıqdır və saytı bağlamır
- [ ] sitemap.xml var və açılır
- [ ] Sayt Google Search Console-a əlavə olunub və təsdiqlənib
- [ ] Hazırlıq mərhələsindəki noindex bayrağı götürülüb
- [ ] Səhifələrin ünvanları oxunaqlıdır və dəyişməyəcək
- [ ] Köhnə sayt varsa, köhnə ünvanlar yenilərinə yönləndirilib
- [ ] 404 səhifəsi var və oradan geri qayıtmaq mümkündür

## 8. Təhlükəsizlik başlıqları

Brauzerin developer alətlərində və ya pulsuz onlayn yoxlayıcı ilə baxılır.

- [ ] Strict-Transport-Security qoyulub
- [ ] Content-Security-Policy qoyulub və sayt onunla işləyir
- [ ] X-Content-Type-Options: nosniff
- [ ] Referrer-Policy qoyulub
- [ ] X-Frame-Options və ya CSP frame-ancestors ilə çərçivəyə salınma bağlanıb
- [ ] Server və framework versiyaları başlıqlarda görünmür
- [ ] Admin və giriş formalarında sorğu limiti var
- [ ] Fayl yüklənməsi varsa: tip və ölçü yoxlanılır
- [ ] Baza girişi internetdən açıq deyil
- [ ] .env və konfiqurasiya faylları brauzerdən açılmır

## 9. Analitika və ölçmə

- [ ] Analitika qurulub və hesab sifarişçinin adınadır
- [ ] Real ziyarət analitikada göründü (canlı yoxlanılıb)
- [ ] Hədəf hadisələr ölçülür: forma göndərilməsi, zəng düyməsi, WhatsApp
- [ ] Kuki və məlumat toplanması barədə bildiriş var (tələb olunursa)

## 10. Formalar və bildirişlər

- [ ] Hər forma real məlumatla doldurulub göndərilib
- [ ] Məktub gəldi — spam qovluğuna düşmədi
- [ ] Məktubun gedəcəyi ünvan sifarişçinindir və dəyişdirilə bilir
- [ ] Göndərəndən sonra istifadəçi aydın təsdiq görür
- [ ] Səhv doldurulanda anlaşılan xəbərdarlıq çıxır
- [ ] Spam qoruması var və adi istifadəçiyə mane olmur
- [ ] Ödəniş varsa: real ödəniş bir dəfə edilib və geri qaytarılıb

## 11. Məzmun

- [ ] Nümunə mətnlər («Lorem ipsum», «Başlıq buraya») qalmayıb
- [ ] Əlaqə məlumatı, ünvan və iş saatları doğrudur
- [ ] Telefon nömrəsi klik edilir, e-poçt ünvanı işləyir
- [ ] Sosial şəbəkə linkləri doğru hesablara aparır
- [ ] Xəritə düzgün nöqtəni göstərir
- [ ] Bütün daxili linklər yoxlanılıb, sınıq link yoxdur

## 12. Sənədləşmə

- [ ] Hansı texnologiya işlədilib — bir səhifə yazılı
- [ ] Necə yenilənir və deploy edilir — addım-addım
- [ ] Mühit dəyişənlərinin siyahısı (adlar; dəyərlər ayrıca kanalla)
- [ ] Üçüncü tərəf servislərin siyahısı, hansı hesabla və nə qədər ödənişlə
- [ ] Zəmanət müddəti və nəyi əhatə etdiyi yazılıb
- [ ] Zəmanətdən sonrakı dəstək şərtləri razılaşdırılıb

## Nəticə

| | |
|---|---|
| Yoxlanan bənd | |
| Bağlanmamış bənd | |
| Təhvil qəbul edilir | bəli / xeyr |
| Qeyd | |
