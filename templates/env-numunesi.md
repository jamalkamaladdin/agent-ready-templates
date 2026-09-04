# =============================================================================
# .env.example
#
# Bu fayl repozitoriyaya düşür, .env isə .gitignore-dadır. Burada yalnız
# dəyişənin adı və izahı olur; real açar, parol və token buraya yazılmır.
#
# İşə başlamaq: cp .env.example .env, sonra boş sətirləri doldur.
# Yeni dəyişən əlavə edəndə əvvəlcə bura yaz. Komanda dəyişəni ancaq bu
# faylda görür. Serverdə unudulan bir sətir gecə saat üçdə tapılır.
# =============================================================================

# --- Tətbiq ------------------------------------------------------------------

# İşləmə rejimi: development | test | production
APP_ENV=development

# Xəta izlərinin cavabda görünməsi. Production-da həmişə false qalır.
APP_DEBUG=true

# Saytın kanonik ünvanı, sonda kəsiksiz. Yönləndirmə, e-poçt linki və
# webhook cavabı bu dəyərdən qurulur; səhv olsa linklər localhost-a aparır.
APP_URL=http://localhost:3000

# Serverin dinlədiyi port.
PORT=3000

# Sessiya və kuki imzası üçün açar. Hər mühitdə ayrı olur:
#   openssl rand -base64 32
# Dəyişdirsən bütün aktiv sessiyalar bağlanır.
APP_SECRET=

# Log səviyyəsi: debug | info | warn | error
LOG_LEVEL=debug

# Vaxt zonası. Baza və hesabatlar bunun üstündə hesablanır.
TZ=Asia/Baku

# --- Verilənlər bazası -------------------------------------------------------

# Tam bağlantı sətri. Parolda @ : / ? # varsa URL-kodlaşdır (@ = %40).
DATABASE_URL=postgres://app:parol@localhost:5432/app_dev

# Eyni anda saxlanılan bağlantı sayı. Serverdəki max_connections-dan
# kiçik olmalıdır; bir neçə nüsxə işləyirsə hamısının cəmi hesablanır.
DATABASE_POOL_MAX=10

# Uzun sorğunun kəsildiyi hədd (millisaniyə). Boş buraxsan bir sorğu
# bütün pool-u tuta bilər.
DATABASE_STATEMENT_TIMEOUT=10000

# Uzaq bazada sertifikat yoxlaması: require | prefer | disable
DATABASE_SSL_MODE=disable

# --- Keş və növbə ------------------------------------------------------------

# Redis bağlantısı. Parol varsa: redis://:parol@host:6379
REDIS_URL=redis://localhost:6379

# Açarların önlüyü. Bir Redis nüsxəsini bir neçə mühit bölüşürsə,
# bunu ayırmasan test məlumatı production keşinə qarışır.
REDIS_PREFIX=app_dev

# Keşin standart ömrü (saniyə).
CACHE_TTL=300

# --- Poçt --------------------------------------------------------------------

# Lokalda Mailpit işlədilir: məktub çölə çıxmır, http://localhost:8025-də görünür.
MAIL_HOST=localhost
MAIL_PORT=1025

# Şifrələmə: none | tls | ssl. Mailpit üçün none, canlıda tls.
MAIL_ENCRYPTION=none

MAIL_USERNAME=
MAIL_PASSWORD=

# Göndərənin ünvanı. Domenin SPF və DKIM qeydləri ilə uyğun olmalıdır,
# yoxsa məktub spam qovluğuna düşür.
MAIL_FROM_ADDRESS=noreply@localhost
MAIL_FROM_NAME="Layihə"

# --- Obyekt saxlama (S3 uyğun) -----------------------------------------------

# AWS-də boş buraxılır; MinIO, R2 və digər servislərdə öz ünvanı yazılır.
S3_ENDPOINT=
S3_REGION=auto
S3_BUCKET=
S3_ACCESS_KEY_ID=
S3_SECRET_ACCESS_KEY=

# MinIO və lokal serverlər üçün true, AWS üçün false.
S3_FORCE_PATH_STYLE=false

# Faylın ictimai ünvanının kökü. CDN varsa CDN domeni yazılır.
S3_PUBLIC_URL=

# Yüklənən faylın yuxarı həddi (bayt). Server tərəfdə də yoxlanılır.
UPLOAD_MAX_BYTES=10485760

# --- Xarici servislər --------------------------------------------------------

# Ödəniş. Test açarı sk_test_ ilə başlayır; canlı açarı təsadüfən
# development mühitinə qoymaq ən bahalı səhvlərdən biridir.
STRIPE_SECRET_KEY=
# Webhook imzası. Bu olmadan gələn sorğunun Stripe-dan gəldiyi bilinmir.
STRIPE_WEBHOOK_SECRET=

# LLM inteqrasiyası
OPENAI_API_KEY=
# Bir sorğunun maksimum token həddi: xərci burada saxlayırsan.
OPENAI_MAX_TOKENS=2000

# Xəta izləmə. Boş qalsa xətalar yalnız serverin log faylında qalır.
SENTRY_DSN=

# Analitika ölçmə ID-si. Yalnız production-da göndərilir.
ANALYTICS_ID=

# Forma spam qoruması
RECAPTCHA_SITE_KEY=
RECAPTCHA_SECRET_KEY=

# --- Bayraqlar və hədlər -----------------------------------------------------

# Yeni funksiyanı kod dəyişmədən açıb-bağlamaq üçün. Bayraq bir neçə həftədən
# artıq qalırsa, ya funksiya hazırdır, ya da silinməlidir.
FEATURE_NEW_CHECKOUT=false

# true olanda sayt yalnız texniki qeyd göstərir.
MAINTENANCE_MODE=false

# Qeydiyyatın açıq olması.
SIGNUP_ENABLED=true

# Bir IP-dən dəqiqədə icazə verilən sorğu sayı.
RATE_LIMIT_PER_MINUTE=60

# Planlanmış tapşırıqların işləməsi. Bir neçə nüsxə qalxanda yalnız
# birində true olur, yoxsa hər tapşırıq neçə dəfə icra edilir.
CRON_ENABLED=true
