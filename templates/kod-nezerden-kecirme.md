# Kod nəzərdən keçirmə çek-listi

| | |
|---|---|
| Pull request | |
| Müəllif | |
| Rəyçi | |
| Tarix | |
| Dəyişən sətir sayı | |

Üç ilkin şərt. Ödənmirsə, rəy başlamır:

- [ ] Dəyişikliyin təsviri var: nə edilir, niyə edilir, necə yoxlanıb
- [ ] CI yaşıldır
- [ ] Dəyişiklik bir işə aiddir və 400 sətirdən kiçikdir

Böyük pull request-də tapılan qüsur sayı azalır, çünki diqqət tükənir.
Böyükdürsə, bölünməsini xahiş etmək rəyin özündən faydalıdır.

## 1. Doğruluq

- [ ] Kod təsvirdə yazılan işi görür, əlavəsini yox
- [ ] Sərhəd halları: boş siyahı, sıfır, mənfi ədəd, bir element, çox uzun mətn
- [ ] `null` və təyin olunmamış dəyər hər yolda düşünülüb
- [ ] Müqayisə operatorları düz istiqamətdədir (`>` ilə `>=` fərqi)
- [ ] Tarix və vaxt zonası: gecə yarısı, ayın sonu, yay vaxtı
- [ ] Pul və ölçü hesablamaları yuvarlaqlaşdırma xətası vermir
- [ ] Eyni əməliyyat iki dəfə gələndə nəticə dəyişmir
- [ ] Eyni anda iki istifadəçi bunu edəndə nə olur, cavab bilinir

## 2. Təhlükəsizlik

- [ ] Kənardan gələn hər dəyər serverdə yoxlanılır (brauzerdəki yoxlama sayılmır)
- [ ] Sorğular parametrlə qurulur, sətir birləşdirməklə yox
- [ ] Səlahiyyət hər sorğuda yoxlanılır: ünvandakı ID-ni dəyişməklə başqasının
      məlumatı açılmır
- [ ] İstifadəçi mətni səhifədə göstərilərkən qaçırılır
- [ ] Fayl yüklənməsində tip, ölçü və ad yoxlanılır
- [ ] Açar, parol və token nə kodda, nə testdə, nə də log-dadır
- [ ] Log-a şəxsi məlumat düşmür
- [ ] Yeni asılılıq əlavə olunubsa: nəyə lazımdır, kim saxlayır, alternativi var
- [ ] Yeni ünvan (endpoint) açıqdırsa, açıq olması qəsdəndir

## 3. Xəta idarəetməsi

- [ ] Boş `catch` yoxdur: ya loga yazılır, ya yuxarı ötürülür
- [ ] Xəta mesajı kontekst daşıyır: hansı istifadəçi, hansı əməliyyat, hansı ID
- [ ] İstifadəçiyə göstərilən mətn daxili detal sızdırmır
- [ ] Xarici servisə hər çağırışda vaxt həddi var
- [ ] Təkrar cəhd varsa, sonsuz deyil və fasiləsi artır
- [ ] Yarımçıq qalan əməliyyat məlumatı yarı vəziyyətdə qoymur
- [ ] Xarici servis cavab vermirsə, sistem nə edir, bu koddan görünür

## 4. Testlər

- [ ] Yeni davranışın testi var
- [ ] Baq düzəlişidirsə, baqı təkrarlayan test var və düzəlişdən əvvəl qırmızı idi
- [ ] Test nəticəni yoxlayır, sadəcə funksiyanın çağırıldığını yox
- [ ] Testlər bir-birindən və icra sırasından asılı deyil
- [ ] Vaxtdan, şəbəkədən və təsadüfi ədəddən asılı test yoxdur
- [ ] Testin adı nəyi yoxladığını deyir
- [ ] Xəta yolu da yoxlanılıb, təkcə uğurlu yol yox

## 5. Oxunaqlıq

- [ ] Adlar niyyəti izah edir, qısaltma sirr saxlamır
- [ ] Funksiya bir iş görür və ekrana sığır
- [ ] Şərhlər «nə edir»i yox, «niyə belədir»i izah edir
- [ ] Ölü kod, şərhə alınmış blok və qalmış konsol çıxışı yoxdur
- [ ] Sehrli rəqəmin adı var
- [ ] Təkrarlanan məntiq üçüncü dəfə yazılmır
- [ ] Kod layihədəki mövcud üsula uyğundur, yeni üsul gətirmirsə

## 6. Performans

- [ ] Dövrənin içində baza sorğusu yoxdur
- [ ] Yeni sorğunun işlətdiyi sütunda indeks var
- [ ] Siyahılar səhifələnir, «hamısını gətir» yoxdur
- [ ] Böyük fayl bütöv yaddaşa yüklənmir
- [ ] Keş açarı istifadəçiləri qarışdırmır
- [ ] Ağır iş istifadəçini gözlətmir, arxa fona verilir
- [ ] Ölçü var: «yavaşdır» yox, «bu sorğu 800 ms çəkir»

## 7. Buraxılış təsiri

- [ ] Baza miqrasiyası geri qaytarıla bilir
- [ ] Miqrasiya böyük cədvəli uzun müddət kilidləmir
- [ ] Köhnə və yeni kod eyni anda işləyə bilir (deploy ani deyil)
- [ ] Yeni mühit dəyişəni sənədləşib və serverdə var
- [ ] Xarici API-nin köhnə istifadəçiləri sınmır
- [ ] Problem çıxsa, geri qaytarma yolu bilinir

## Rəyin dili

- Şəxsə yox, koda yazılır: «bu funksiya», «sən» yox.
- Fikir və tələb ayrılır: «Xahiş: …» / «Fikir: …» / «Sual: …».
- Sual da rəydir. Anlamadığın yer, oxuyan növbəti adamın da anlamayacağı yerdir.
- Bir bənd üçün iki dəfə yazıb razılığa gəlmirsənsə, söhbətə keçilir.
- Təriflənəsi həll varsa, yazılır. Rəy yalnız qüsur siyahısı deyil.

## Nəticə

| | |
|---|---|
| Qərar | təsdiq / düzəlişdən sonra təsdiq / yenidən baxılsın |
| Bağlanmalı bəndlər | |
| Sonraya buraxılanlar (task nömrəsi ilə) | |
