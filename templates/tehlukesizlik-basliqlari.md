# =============================================================================
# HTTP təhlükəsizlik başlıqları
#
# nginx üçün: bu faylı /etc/nginx/snippets/tehlukesizlik-basliqlari.conf kimi
# saxla və server blokunda çağır:
#
#     include snippets/tehlukesizlik-basliqlari.conf;
#
# Sonra: nginx -t   (sintaksisi yoxlayır)
#        systemctl reload nginx
#
# Caddy variantı faylın sonundadır, şərh içində.
#
# TƏLƏ: nginx-də add_header irsi «hamısı və ya heç biri» qaydası ilə işləyir.
# Hər hansı location blokunda bir dənə add_header yazsan, o blokda yuxarıdan
# gələn BÜTÜN add_header sətirləri sönür. Belə hallarda ya bu include-u həmin
# location-a da əlavə et, ya da başlıqları yalnız bir yerdə saxla.
#
# Yoxlama: curl -sI https://domen.az | sort
# =============================================================================

# --- 1. HSTS -----------------------------------------------------------------
# Brauzer bu başlığı bir dəfə görəndən sonra domenə yalnız https ilə gedir.
# includeSubDomains bütün alt domenlərə şamil olunur: hər birinin işlək
# sertifikatı olmalıdır, yoxsa onlar açılmır.
# preload əlavə etməzdən əvvəl iki dəfə düşün: brauzerin siyahısından
# çıxmaq aylar çəkir və o müddətdə geri dönüş yoxdur.
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains" always;

# --- 2. Content-Security-Policy ----------------------------------------------
# Səhifənin hansı mənbədən skript, stil, şəkil və şrift yükləyə biləcəyini
# müəyyən edir. XSS-in təsirini kəsən yeganə başlıq budur, həm də sınmağa ən
# meyllisi.
#
# QAYDA: əvvəlcə Content-Security-Policy-Report-Only ilə aç, bir neçə gün
# brauzer konsolunu izlə, siyahını real ehtiyaca görə dəqiqləşdir, sonra
# başlığın adından "-Report-Only" hissəsini sil.
#
# Analitika, xəritə, video və ya ödəniş widget-i işlədirsənsə, onların
# domenləri müvafiq sətirlərə əlavə olunur; əks halda səssizcə yüklənmirlər.
add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-src 'none'; frame-ancestors 'none'; form-action 'self'; base-uri 'self'; object-src 'none'; upgrade-insecure-requests" always;

# --- 3. Fayl tipinin təxmini --------------------------------------------------
# Brauzer məzmuna baxıb «bu, əslində skriptdir» qərarını verə bilməz.
# İstifadəçi yüklədiyi fayl saxlanan saytlarda bu bir sətir hücum yolu bağlayır.
add_header X-Content-Type-Options "nosniff" always;

# --- 4. Referrer --------------------------------------------------------------
# Xarici sayta keçəndə yalnız domen adı gedir, tam ünvan yox.
# Beləliklə /sifaris/12345 kimi daxili yollar kənar analitikaya düşmür.
add_header Referrer-Policy "strict-origin-when-cross-origin" always;

# --- 5. Brauzer imkanları -----------------------------------------------------
# Boş mötərizə «heç kimə icazə yoxdur» deməkdir. Saytın kameraya və ya
# yerləşməyə ehtiyacı varsa, həmin bənd (self) ilə əvəz olunur.
add_header Permissions-Policy "camera=(), microphone=(), geolocation=(), payment=(), usb=(), accelerometer=(), autoplay=(), browsing-topics=()" always;

# --- 6. Çərçivə qoruması ------------------------------------------------------
# CSP-dəki frame-ancestors bunu onsuz da bağlayır; X-Frame-Options isə
# köhnə brauzerlər üçün qalır. Saytın öz-özünü çərçivəyə salması lazımdırsa
# DENY əvəzinə SAMEORIGIN yazılır.
add_header X-Frame-Options "DENY" always;

# --- 7. Pəncərə və resurs təcridi --------------------------------------------
# Açılan pəncərə ilə açan səhifə arasındakı əlaqəni kəsir.
add_header Cross-Origin-Opener-Policy "same-origin" always;
# Resurslarımızın kənar saytdan yüklənməsini bağlayır. Şəkil və ya skript
# başqa domendən oxunmalıdırsa bu sətir çıxarılır.
add_header Cross-Origin-Resource-Policy "same-origin" always;

# --- 8. Server barmaq izi -----------------------------------------------------
# nginx versiyasını cavabdan və xəta səhifələrindən silir.
server_tokens off;
# X-Powered-By başlığı tətbiq qatından gəlir: onu framework tərəfdə söndür
# (Next.js üçün next.config faylında poweredByHeader: false).

# =============================================================================
# CADDY VARİANTI
#
# Aşağıdakı hissə Caddyfile üçündür. nginx onu oxumasın deyə şərh içindədir:
# kopyalayanda sətir başındakı "# " işarələrini sil.
#
# Caddy https-i və HSTS-siz də olsa sertifikatı özü qurur, ona görə burada
# yalnız başlıqlar var. Yoxlama: caddy validate --config Caddyfile
#
# domen.az {
#     encode gzip zstd
#     reverse_proxy 127.0.0.1:3000
#
#     header {
#         Strict-Transport-Security "max-age=63072000; includeSubDomains"
#         Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-src 'none'; frame-ancestors 'none'; form-action 'self'; base-uri 'self'; object-src 'none'; upgrade-insecure-requests"
#         X-Content-Type-Options "nosniff"
#         Referrer-Policy "strict-origin-when-cross-origin"
#         Permissions-Policy "camera=(), microphone=(), geolocation=(), payment=(), usb=(), accelerometer=(), autoplay=(), browsing-topics=()"
#         X-Frame-Options "DENY"
#         Cross-Origin-Opener-Policy "same-origin"
#         Cross-Origin-Resource-Policy "same-origin"
#
#         # Minusla başlayan sətir başlığı silir: Caddy öz adını yazmasın.
#         -Server
#         -X-Powered-By
#     }
# }
#
# Caddy-də başlıqlar bütün cavablara şamil olunur, nginx-dəki irs tələsi
# burada yoxdur.
# =============================================================================
