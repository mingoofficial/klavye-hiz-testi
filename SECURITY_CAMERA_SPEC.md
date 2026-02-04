# Android Security Camera (Monitoring + Talkback) — Proje Taslağı

Bu doküman, **sabit bir Android cihazın kamera olarak** ve **başka bir Android cihazın monitör olarak** kullanıldığı, **canlı izleme + kayıt + insan algılama + bildirim + çift yönlü ses** özelliklerine sahip güvenlik kamerası uygulamasının teknik taslağıdır.

## Hedef Özellikler

- **Kamera cihazı (sabit)**
  - Canlı görüntü yayını
  - Anlık fotoğraf + video kayıt
  - İnsan algılama (local/on-device öncelikli)
  - İnsan algılanınca bildirim
  - Kamera ışığını/flash’ı aç/kapat
  - Mikrofon aktif: monitörden konuşma → kameradan ses çıkışı

- **Monitör cihazı**
  - Canlı görüntü izleme
  - Bildirim alma
  - Kamera ışığını/flash’ı kontrol etme
  - Mikrofonla konuşma (talkback)

## Minimum Mimari

1. **Canlı Yayın**
   - Tercih: **WebRTC** (düşük gecikme)
   - Alternatif: RTSP (daha kolay, ama gecikme yüksek olabilir)

2. **İnsan Algılama**
   - **On-device**: TensorFlow Lite / ML Kit (daha hızlı, gizlilik için iyi)
   - Alternatif: Sunucu tarafı algılama (daha güçlü, ama gecikme ve gizlilik dezavantajı)

3. **Bildirimler**
   - **Firebase Cloud Messaging (FCM)**
   - Eşleşme için Firebase Auth (Google hesap ile giriş)

4. **Video/Fotoğraf Kayıt**
   - Yerel depolama (kamera cihazı)
   - Opsiyonel: Google Drive veya Firebase Storage

5. **Sesli Konuşma (Talkback)**
   - WebRTC data/audio channel
   - Monitörden konuşma → kamera cihazı hoparlörü

## Önerilen Teknoloji Stack

- **Android**: Kotlin
- **UI**: Jetpack Compose
- **Streaming**: WebRTC (libwebrtc) veya Agora/LiveKit SDK
- **Algılama**: ML Kit (Pose/Object) veya TFLite + MobileNet/YOLOv5n
- **Push**: Firebase FCM
- **Auth**: Firebase Auth (Google)
- **Storage**: Local + opsiyonel Firebase Storage

## Akış

1. **Kamera cihazında** kullanıcı Google hesabıyla giriş yapar.
2. **Monitör cihazında** aynı Google hesabıyla giriş yapılır.
3. Sunucu/Firebase üzerinden cihazlar eşleştirilir.
4. Kamera cihazı canlı yayın açar.
5. İnsan algılama çalışır → algılarsa bildirim gönderir.
6. Monitör cihaz bildirim alır, canlı yayına bağlanır.
7. Monitör cihazdan konuşma yapılır → kamera hoparlöründen ses çıkar.
8. Monitör cihazdan kamera ışığı kontrol edilir.

## MVP (En Az) Özellik Seti

- Canlı görüntü
- İnsan algılama
- Bildirim
- Talkback

## Geliştirme Notları

- **Gizlilik**: Kullanıcı dışında kimse erişememeli.
- **Pil Tüketimi**: Algılama ve kamera açık olduğu için optimizasyon şart.
- **Offline Mod**: Aynı Wi-Fi’da yerel bağlantı seçeneği eklenebilir.

## Alternatif (Hazır Çözüm)

Eğer sıfırdan geliştirmek yerine hızlı çözüm istenirse:
- Alfred Camera (uygulama)
- tinyCam Monitor + IP Webcam

> Bu doküman proje başlangıcında gereksinimleri netleştirmek ve MVP planlamak için hazırlanmıştır.
