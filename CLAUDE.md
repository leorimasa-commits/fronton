# Frontón · app Android

App de entrenamiento de tenis en frontón (16 sesiones de 40 min, 4 niveles, zurdo/diestro,
esquemas del frontón del club, multimedia). Toda la app es una sola página: `www/index.html`.
Capacitor 6 la empaqueta como app Android (`appId: ar.fronton.tenis`).

## Estructura
- `www/index.html` — la app completa (HTML + CSS + JS, sin dependencias). Editá acá.
- `capacitor.config.json` — nombre e id de la app.
- `android/` — proyecto nativo generado por Capacitor. No editar a mano salvo íconos/manifiesto.
- `.github/workflows/build-apk.yml` — compila el APK en GitHub Actions en cada push.

## Compilar el APK
Local (requiere Android Studio o Android SDK + JDK 17):
```
npm install
npx cap sync android
cd android && ./gradlew assembleDebug
```
Resultado: `android/app/build/outputs/apk/debug/app-debug.apk`.

En GitHub: cada push a main compila y publica el APK en https://github.com/leorimasa-commits/fronton/releases/latest/download/fronton.apk (también queda como artefacto `fronton-apk` en Actions).

## Después de cada cambio en www/
`npx cap sync android` y volver a compilar.

## Notas de la app
- Pared plana: lo recto vuelve recto, lo diagonal sale en la diagonal opuesta. Los ejercicios respetan eso.
- Frontón del club: línea blanca = red, 3 círculos, piso de 12 m con pasillo central, 1ª y 2ª línea.
- Textos con {Do}/{Da}/{Ro}/{Ra} se resuelven según la mano elegida (función `tx`); esquemas se espejan en `fig`.
- El progreso se guarda en localStorage (clave `fronton`), incluida la alarma elegida (`store.alarm`: fuerte/suave/vibrar).
- Cronómetro por timestamp (`endAt`), no por conteo de ticks. Al llegar a cero: alarma propia + cartel "¡Tiempo!".
- En la app instalada usa los plugins `@capacitor/local-notifications` (notificación programada 1,5 s después del fin, por si el teléfono está bloqueado) y `@capacitor-community/keep-awake` (pantalla encendida mientras corre). Se acceden por `window.Capacitor.Plugins` (no hay bundler).
- Pendiente para producción: APK firmado (release) y publicación en Play Store.
