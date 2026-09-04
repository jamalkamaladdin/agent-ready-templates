# Ehtiyat nüsxə və bərpa qaydası

| | |
|---|---|
| Sistem / layihə | |
| Verilənlər bazası | PostgreSQL / MySQL / digər |
| Bazanın həcmi | |
| Sənədin sahibi | |
| Son yenilənmə | |
| Son bərpa sınağı | |

## 1. İki rəqəm

Bu iki rəqəm razılaşdırılmayıb, qalan hər şey təxmindir.

| | Dəyər | Nə deməkdir |
|---|---|---|
| Nə qədər məlumat itə bilər | ____ saat | Nüsxələr arasındakı fasilə. Gündə bir nüsxə, gün ərzindəki işi itirmək riski deməkdir. |
| Nə qədər dayanma dözüləndir | ____ saat | Nasazlıqdan sistemin işə qayıtmasına qədər keçən vaxt. Nüsxəni endirmək və açmaq da bu vaxta daxildir. |

Rəqəmlər biznes tərəfi ilə birlikdə yazılır: «heç nə itməməlidir» cavab deyil,
onun qiyməti var və o qiymət hesablanır.

## 2. Nə, nə vaxt, harada

| Nə | Tezlik | Harada saxlanılır | Nə qədər qalır | Kim yoxlayır |
|---|---|---|---|---|
| Bazanın tam nüsxəsi | hər gün 03:00 | | 30 gün | |
| Bazanın həftəlik nüsxəsi | bazar günü | | 6 ay | |
| Aylıq arxiv | ayın 1-i | | 1 il | |
| Tranzaksiya log-ları (WAL / binlog) | fasiləsiz | | 7 gün | |
| Yüklənmiş fayllar (media) | hər gün | | 30 gün | |
| Konfiqurasiya və mühit dəyişənləri | dəyişəndə | parol menecerində | | |

Media və konfiqurasiya çox vaxt unudulur. Baza bərpa olunur, sayt qalxır,
amma bütün şəkillər yoxdur.

## 3. Harada saxlanılır

Üç nüsxə, iki fərqli mühit, biri tamamilə ayrı yerdə:

- [ ] Nüsxə bazanın işlədiyi serverdə saxlanılmır
- [ ] Ən azı bir nüsxə başqa provayderdədir (server hesabı bağlansa da qalır)
- [ ] Saxlama yerinin girişi ayrı hesabdadır, tətbiqin açarı ilə silinə bilmir
- [ ] Nüsxə şifrələnib, açarın harada olduğu yazılıb
- [ ] Yalnız əlavə etməyə icazə verən (append-only) rejim mümkündürsə açılıb

Sonuncu bənd zərərli proqram üçündür: serverə girən adam nüsxələri də silə
bilirsə, nüsxə yoxdur.

## 4. Nüsxənin alınması

~~~bash
# PostgreSQL, custom format: seçmə bərpaya imkan verir
pg_dump --no-owner --format=custom --file=/tmp/baza-$(date +%F).dump baza

# Faylın oxunaqlı olduğunu yoxla (siyahı verirsə fayl bütövdür)
pg_restore --list /tmp/baza-$(date +%F).dump > /dev/null && echo "nüsxə oxunur"

# MySQL / MariaDB, cədvəlləri kilidləmədən
mysqldump --single-transaction --routines --triggers \
  --default-character-set=utf8mb4 baza | gzip > /tmp/baza-$(date +%F).sql.gz
~~~

Yaradılan nüsxənin ölçüsü qeyd olunur. Ölçü birdən iki dəfə kiçilibsə,
səbəb tapılana qədər nüsxə etibarlı sayılmır.

## 5. Bərpa qaydası

Bu bölmə nasazlıq günü açılır. Ona görə burada izah yox, əmr olur.

**Addım 1.** Nüsxəni tap və endir. Tam yol:

~~~text
(saxlama yeri, qovluq quruluşu və fayl adı qaydası)
~~~

**Addım 2.** Faylın bütövlüyünü yoxla.

~~~bash
sha256sum -c baza-2026-01-01.dump.sha256
~~~

**Addım 3.** Bərpanı əvvəlcə **ayrı** bazaya et. İşləyən bazanın üstünə
bərpa etmək, səhv nüsxə ilə vəziyyəti daha da pisləşdirməyin ən qısa yoludur.

~~~bash
createdb baza_berpa
pg_restore --no-owner --dbname=baza_berpa baza-2026-01-01.dump
~~~

**Addım 4.** Məlumatı yoxla: ən böyük cədvəllərin sətir sayı, son yazının
tarixi, bir neçə real qeyd.

~~~sql
SELECT max(created_at) FROM sifarisler;
SELECT count(*) FROM istifadeciler;
~~~

**Addım 5.** Tətbiqi bərpa olunmuş bazaya bağla, əsas axınları yoxla, sonra
adları dəyişdir.

**Addım 6.** İşin bitdiyini elan et, hadisə hesabatını yaz.

| Addım | Gözlənilən vaxt | Kim edir |
|---|---|---|
| Nüsxənin tapılması və endirilməsi | | |
| Bərpa | | |
| Yoxlama | | |
| Trafikin qaytarılması | | |

## 6. Bərpa sınağı

Sınaqdan keçməmiş nüsxə nüsxə deyil, ümiddir. Sınaq rübdə bir dəfə, təqvimə
salınmış tarixdə keçirilir və nəticəsi bura yazılır.

| Tarix | Kim | Nüsxənin tarixi | Bərpa müddəti | Nəticə | Qeyd |
|---|---|---|---|---|---|
| | | | | keçdi / keçmədi | |
| | | | | | |
| | | | | | |

Sınaq zamanı ölçülür:

- [ ] Nüsxə tapıldı və endirildi
- [ ] Bərpa xətasız keçdi
- [ ] Sətir sayları gözləniləndir
- [ ] Tətbiq bərpa olunmuş baza ilə işlədi
- [ ] Faktiki bərpa müddəti razılaşdırılmış həddən azdır
- [ ] Qaydadakı əmrlər olduğu kimi işlədi (işləmirsə, sənəd elə bu gün düzəlir)

## 7. Nəzarət

Nüsxə prosesinin ən çox rast gəlinən nasazlığı səssiz dayanmadır: cron sınır,
disk dolur, açar bitir və heç kim aylarla bilmir.

- [ ] Uğursuz nüsxə barədə bildiriş konkret adama gedir
- [ ] Nüsxə gözlənilən vaxtda gəlmirsə, bu da bildiriş yaradır
- [ ] Saxlama yerinin dolması izlənilir
- [ ] Nüsxənin ölçüsü qeyd olunur və kəskin dəyişəndə xəbərdarlıq verilir
- [ ] Aylıq olaraq bir nəfər siyahıya baxır və təsdiqləyir

## 8. Girişlər

| Nə lazımdır | Kimdədir | Harada saxlanılır |
|---|---|---|
| Saxlama yerinin girişi | | parol menecerində |
| Şifrələmə açarı | | |
| Baza istifadəçisi və parolu | | |
| Serverin SSH açarı | | |

Bu məlumatların hamısı yalnız bir nəfərdədirsə, plan həmin adamın telefonunun
işlək olmasından asılıdır. Ən azı iki nəfərin girişi olur.
