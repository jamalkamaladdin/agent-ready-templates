# =============================================================================
# .github/workflows/ci.yml
#
# Node və Next.js layihəsi üçün minimum CI. Dörd addım bir işdə gedir:
# typecheck, lint, test, build. Ayrı-ayrı işlərə bölmək paralellik verir,
# amma hər iş asılılıqları yenidən qurur — kiçik layihədə bu, qazancdan çoxdur.
#
# Paket meneceri npm-dirsə: pnpm/action-setup addımını sil, cache: npm yaz,
# əmrləri "npm ci" və "npm run ..." ilə əvəz et.
# =============================================================================

name: CI

on:
  push:
    branches: [main]
  pull_request:

# Eyni branch-a ardıcıl push olanda köhnə icra dayandırılır: növbə yığılmır.
concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true

# Standart icazə oxumaqdır. Deploy addımı əlavə edəndə icazə də orada verilir.
permissions:
  contents: read

jobs:
  checks:
    runs-on: ubuntu-latest
    # Asılmış icra saatlarla dayanmasın deyə.
    timeout-minutes: 15

    steps:
      - name: Kodu götür
        uses: actions/checkout@v4

      - name: pnpm quraşdır
        uses: pnpm/action-setup@v4
        with:
          version: 9

      - name: Node quraşdır və paket keşini qoş
        uses: actions/setup-node@v4
        with:
          # .nvmrc varsa versiya oradan oxunur, iki yerdə saxlanmır.
          node-version-file: .nvmrc
          cache: pnpm

      - name: Asılılıqları qur
        # frozen-lockfile: lock faylı package.json ilə uyğun deyilsə icra dayanır.
        # Bu, "lokalda başqa versiya quruldu" problemini elə burada tutur.
        run: pnpm install --frozen-lockfile

      - name: Tipləri yoxla
        run: pnpm exec tsc --noEmit

      - name: Lint
        run: pnpm lint

      - name: Testlər
        # Vitest və Jest CI-da izləmə rejiminə keçməməlidir, yoxsa icra donur.
        run: pnpm test --run
        env:
          CI: true

      - name: Next.js build keşi
        uses: actions/cache@v4
        with:
          path: .next/cache
          # Mənbə dəyişəndə açar dəyişir, restore-keys isə ən yaxın köhnə
          # keşi gətirir: build sıfırdan başlamır.
          key: next-${{ runner.os }}-${{ hashFiles('pnpm-lock.yaml') }}-${{ hashFiles('src/**', 'app/**') }}
          restore-keys: |
            next-${{ runner.os }}-${{ hashFiles('pnpm-lock.yaml') }}-
            next-${{ runner.os }}-

      - name: Build
        run: pnpm build
        env:
          # Build vaxtı lazım olan dəyişənlər buraya yazılır. Sirr olanlar
          # repozitoriyanın Settings > Secrets bölməsindən gəlir, faylda yox.
          NEXT_TELEMETRY_DISABLED: 1
          # NEXT_PUBLIC_SITE_URL: ${{ vars.SITE_URL }}
          # DATABASE_URL: ${{ secrets.DATABASE_URL }}

# -----------------------------------------------------------------------------
# Növbəti addım: bu workflow yaşıl olandan sonra main branch-ı qorumaya al.
# Settings > Branches > Add rule: "Require status checks to pass" seçilir və
# "checks" işi tələb kimi işarələnir. Bu edilməsə CI xəbərdarlıq edir, amma
# heç nəyin qarşısını almır.
# -----------------------------------------------------------------------------
