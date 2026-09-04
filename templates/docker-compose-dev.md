# =============================================================================
# Lokal inkişaf yığını: PostgreSQL, Redis, Mailpit, Adminer.
#
#   docker compose up -d          qaldırır
#   docker compose ps             vəziyyəti göstərir (health sütununa bax)
#   docker compose logs -f db     bir servisin loguna baxır
#   docker compose stop           saxlayır, məlumat qalır
#
# Volume-ları da silmək lazım gələndə "down" əmrinə volume bayrağı əlavə edilir;
# o bayraq bazanı sıfırlayır, ona görə əl ilə və bilərək yazılır.
#
# `version:` sahəsi qəsdən yoxdur: Compose V2 onu oxumur, yalnız xəbərdarlıq verir.
# Bu fayl yalnız lokal maşın üçündür. Parollar sadədir və elə qalmalıdır:
# production konfiqurasiyasını bunun üstünə qurmaq olmaz.
# =============================================================================

name: layihe-dev

services:
  db:
    image: postgres:17-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: parol
      POSTGRES_DB: app_dev
      # Yalnız baza ilk dəfə qurulanda oxunur; sonrakı başlanğıcda təsiri yoxdur.
      POSTGRES_INITDB_ARGS: "--encoding=UTF8"
    ports:
      # Soldakı host portudur. Maşında başqa Postgres varsa "5433:5432" yaz.
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data
      # Buradakı .sql və .sh fayllar yalnız boş bazada, bir dəfə icra olunur.
      - ./docker/db-init:/docker-entrypoint-initdb.d:ro
    healthcheck:
      # Konteynerin qalxması ilə bazanın sorğu qəbul etməsi eyni an deyil.
      test: ["CMD-SHELL", "pg_isready -U app -d app_dev"]
      interval: 5s
      timeout: 3s
      retries: 10
      start_period: 10s
    # Yavaş sorğu lokalda da görünsün deyə: 200 ms-dən uzun hər sorğu loga düşür.
    command:
      - "postgres"
      - "-c"
      - "log_min_duration_statement=200"

  cache:
    image: redis:7-alpine
    restart: unless-stopped
    # appendonly: yenidən qaldırdıqda keş itmir.
    # maxmemory: bir dövrə çıxıb bütün RAM-ı yeməsin deyə hədd qoyulur.
    command:
      - "redis-server"
      - "--appendonly"
      - "yes"
      - "--maxmemory"
      - "256mb"
      - "--maxmemory-policy"
      - "allkeys-lru"
    ports:
      - "6379:6379"
    volumes:
      - cache-data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 10

  mail:
    # Bütün gedən məktubları tutur. Lokaldan real ünvana məktub getmir.
    image: axllent/mailpit:latest
    restart: unless-stopped
    ports:
      - "1025:1025"   # SMTP, tətbiq bura göndərir
      - "8025:8025"   # veb interfeys: http://localhost:8025
    environment:
      MP_MAX_MESSAGES: 500
      MP_DATABASE: /data/mailpit.db
      MP_SMTP_AUTH_ACCEPT_ANY: 1
      MP_SMTP_AUTH_ALLOW_INSECURE: 1
    volumes:
      - mail-data:/data
    healthcheck:
      test: ["CMD", "/mailpit", "readyz"]
      interval: 10s
      timeout: 3s
      retries: 5

  adminer:
    # Bazaya brauzerdən baxmaq üçün: http://localhost:8080
    # Server xanası "db", istifadəçi "app", parol "parol".
    image: adminer:5
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      ADMINER_DEFAULT_SERVER: db
      ADMINER_DESIGN: dracula
    depends_on:
      db:
        # Sadəcə "depends_on: [db]" konteynerin qalxmasını gözləyir,
        # bazanın hazır olmasını yox. Fərq ilk saniyələrdə görünür.
        condition: service_healthy

volumes:
  db-data:
  cache-data:
  mail-data:

# -----------------------------------------------------------------------------
# Tətbiqin .env faylı üçün uyğun dəyərlər:
#
#   DATABASE_URL=postgres://app:parol@localhost:5432/app_dev
#   REDIS_URL=redis://localhost:6379
#   MAIL_HOST=localhost
#   MAIL_PORT=1025
#
# Tətbiq də konteynerdə işləyirsə localhost əvəzinə servis adları yazılır:
# db, cache, mail.
# -----------------------------------------------------------------------------
