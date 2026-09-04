# Hadisə hesabatı

| | |
|---|---|
| Hadisənin adı | |
| Tarix | |
| Başlama vaxtı (faktiki) | |
| Aşkarlanma vaxtı | |
| Bərpa vaxtı | |
| Ümumi müddət | |
| Ciddilik | 1 (tam dayanma) / 2 (əsas funksiya işləmir) / 3 (məhdud təsir) |
| Hadisəni idarə edən | |
| Hesabatı yazan | |
| Vəziyyət | qaralama / müzakirə olunub / bağlanıb |

Qayda: bu sənəddə adam adı yalnız «kim nə etdi» xronologiyasında keçir, səbəb
kimi yox. Səhv etməyə imkan verən sistem düzəldilir, adam yox. Günahlandıran
hesabat növbəti dəfə gec yazılır və ya heç yazılmır.

## 1. Bir abzasla nə oldu

Texniki termin olmadan, kənardan baxan adamın anlayacağı dildə. Nə işləmədi,
nə qədər davam etdi, nə ilə bərpa olundu.

## 2. Təsir

Rəqəmsiz təsir bölməsi «bir az problem oldu» deməkdir.

| | |
|---|---|
| Neçə istifadəçi təsirləndi | |
| Hansı funksiya işləmədi | |
| Uğursuz sorğu / sifariş sayı | |
| İtən və ya təkrar emal edilən məlumat | |
| Maliyyə təsiri (təxmini) | |
| Xarici bildiriş verildi? | bəli / xeyr, kim, nə vaxt |

## 3. Xronologiya

Vaxtlar bir zonada yazılır. «Təxminən» sözü işlənirsə, qeyd olunur.

| Vaxt | Nə oldu | Kim / haradan bilindi |
|---|---|---|
| | Dəyişiklik yayımlandı / yük artdı / xarici servis dayandı | |
| | İlk xəta log-da göründü | |
| | Bildiriş işə düşdü | |
| | Hadisə elan edildi | |
| | Səbəb barədə ilk fərziyyə | |
| | Fərziyyə səhv çıxdı | |
| | Real səbəb tapıldı | |
| | Düzəliş tətbiq edildi | |
| | Sistem normal işə qayıtdı | |
| | Yoxlama bitdi, hadisə bağlandı | |

## 4. Necə bilindi

- [ ] Monitorinq xəbər verdi
- [ ] Komanda üzvü təsadüfən gördü
- [ ] Müştəri şikayət etdi
- [ ] Xarici servis xəbər verdi

Aşkarlanma vaxtı ilə başlama vaxtı arasındakı fərq nə qədərdir və bu fərq
qəbul edilə biləndirmi? Müştəri sizdən əvvəl bilirsə, birinci düzəliş
monitorinqdədir.

## 5. Kök səbəb

Texniki tetikləyici (hansı dəyişiklik, hansı sorğu, hansı hədd) ayrıca, ona
yol açan şərait ayrıca yazılır. «Fayl silinib» səbəb deyil. Səbəb, o faylın
bir adamın əlindən silinə bilməsidir.

Beş «niyə»:

1. Niyə xidmət dayandı? →
2. Niyə? →
3. Niyə? →
4. Niyə? →
5. Niyə? →

Bir neçə səbəb üst-üstə düşübsə, hamısı yazılır. Nasazlıqların çoxu tək
səbəbdən olmur.

## 6. Nə yaxşı işlədi

Bunu yazmaq sənədin yarısıdır: işləyən şey növbəti dəfə də saxlanılmalıdır.

-
-

## 7. Nə çətinləşdirdi

Bərpanı gecikdirən hər şey: çatışmayan giriş, köhnə sənəd, tapılmayan log,
işləməyən bildiriş, əlçatmaz adam.

-
-

## 8. Düzəliş addımları

Hər bənd sahibi və son tarixi olan real tapşırıqdır. Sahibi olmayan bənd
hesabatla birlikdə arxivə gedir.

| # | Nə ediləcək | Növ | Sahibi | Son tarix | Task | Vəziyyət |
|---|---|---|---|---|---|---|
| 1 | | qarşısını alan | | | | |
| 2 | | tez aşkarlayan | | | | |
| 3 | | bərpanı sürətləndirən | | | | |
| 4 | | sənəd / təlim | | | | |

Növlərin üçü də olmalıdır. Yalnız «bir də belə etməyək» yazılıbsa, heç nə
dəyişməyib.

## 9. Təkrarlanma riski

- [ ] Eyni səbəb sistemin başqa hissəsində də var, harada:
- [ ] Bu nasazlığı indi tutacaq bildiriş var
- [ ] Düzəliş sınaqdan keçirilib (yalnız yazılmayıb)
- [ ] Bərpa qaydası sənəddə yenilənib
- [ ] Növbətçi bu ssenari ilə tanış edilib

## 10. Açıq suallar

Cavabı hələ bilinməyənlər. Boş qoymaqdansa «bilinmir» yazmaq düzdür.

-
-

## Əlavələr

- Log parçaları, qrafiklər, sorğu planları
- Əlaqədar pull request və deploy nömrələri
- Söhbət arxivinin linki
