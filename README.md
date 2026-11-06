# CompraFácil

Aplicación familiar de compras en Flutter, Dart y Material 3. Una sola base de código para web, Android, iOS y Windows.

## Estado

Publicada en **https://comprafacil-omega.vercel.app** y conectada al proyecto Supabase `comprafacil` en São Paulo. RLS, Realtime y almacenamiento privado están configurados. El SMTP de Gmail se verificó con un registro real: después de reemplazar la credencial rechazada por una contraseña de aplicación de Google, se creó la cuenta del propietario y se confirmó su correo. Falta validar la colaboración desde dos cuentas reales de la familia.

El modo local sigue disponible al compilar sin variables de Supabase y se identifica expresamente en la interfaz.

Diseño visual actualizado según la referencia Duolingo adjunta: Nunito, verde y azul, bordes definidos y botones con relieve sólido. Detalles en `docs/DISENO_ACTUAL.md`. El cambio conserva las operaciones y los datos de la aplicación.

La interfaz abre directamente las listas y utiliza tres accesos: Listas, Familia y Más. Los filtros y campos opcionales se despliegan cuando hacen falta; las funciones anteriores se conservan.

<img src="docs/images/mobile.png" width="300" alt="CompraFácil en un celular: listas y tres accesos principales">

## Arquitectura

- `lib/domain`: entidades, catálogo, parser local y contratos.
- `lib/data`: SQLite con Drift, repositorio offline y adaptador de Supabase.
- `lib/application`: estado y operaciones con Riverpod.
- `lib/presentation`: navegación con GoRouter, tema y pantallas adaptables.
- `supabase`: esquema, RLS y funciones transaccionales.
- `test`: pruebas de dominio, persistencia, sincronización y widgets.
- `scripts`: preparación y compilación reproducible para Vercel.

## Ejecución

Para abrir la versión ya compilada, hacer doble clic en **Abrir CompraFacil.cmd**. Requiere Node.js (presente en este equipo) y abre `http://localhost:4173`. El servidor escucha solamente en esta computadora. Para desarrollar:

Instalar Flutter estable, Git y las herramientas de la plataforma correspondiente. Con el SDK local de esta carpeta:

```powershell
$env:PATH = "$PWD\.tools\git\cmd;$PWD\.tools\flutter\bin;" + $env:PATH
flutter pub get --enforce-lockfile
flutter run -d chrome
```

Sin variables de Supabase la aplicación ofrece un espacio local persistente. Para activar la nube, copiar `config.example.json` a `config.local.json`, completar la URL y la clave pública del proyecto, aplicar el esquema y ejecutar:

```powershell
flutter run -d chrome --dart-define-from-file=config.local.json
```

Nunca usar una clave `service_role` en el cliente. `config.local.json` queda excluido de Git.

## Verificación

```powershell
flutter analyze
flutter test
flutter build web --release --no-wasm-dry-run
node scripts/finalize-web.mjs
```

La compilación de Android requiere Android SDK; Windows requiere Visual Studio con Desktop development with C++; iOS requiere macOS y Xcode. La inspección de este equipo detectó que Android SDK y Visual Studio no están instalados. No se considera ninguna plataforma validada por el solo hecho de generar su estructura.

## Despliegue

Ver `docs/DEPLOYMENT.md`. Vercel sirve la compilación web de Flutter. Los binarios Android, iOS y Windows se distribuyen por separado.

Para agregar familiares, ver `docs/USO_FAMILIAR.md`. Verificaciones en `docs/VERIFICACION.md`. El inventario de cuentas y despliegues queda en el archivo local `docs/HOSTING.json`, excluido de Git.
