# Mis Rutinas: cómo ponerla en tu móvil

Son unos 15 minutos y todo es gratis. Al terminar tendrás:

- Una app en la pantalla de inicio del móvil (y en el ordenador, si quieres).
- Tus datos guardados en tu propia base de datos de Google (Firebase), sincronizados entre dispositivos.
- Funcionamiento sin conexión: lo que hagas se sube solo cuando vuelva internet.

Necesitas una cuenta de Google y una cuenta de GitHub. GitHub es gratis y la creas en github.com.

---

## 1. Crea tu proyecto de Firebase

1. Entra en **https://console.firebase.google.com** con tu cuenta de Google.
2. Pulsa **Crear un proyecto** (o «Añadir proyecto»).
3. Ponle de nombre `mis-rutinas`, acepta las condiciones y continúa.
4. Desactiva **Google Analytics**, porque no hace falta, y pulsa **Crear proyecto**.

## 2. Activa el acceso con email y contraseña

1. En el menú de la izquierda: **Compilación → Authentication → Comenzar**.
2. En la pestaña **Método de inicio de sesión**, elige **Correo electrónico/contraseña**.
3. Activa el primer interruptor y pulsa **Guardar**.

## 3. Crea la base de datos

1. Menú de la izquierda: **Compilación → Firestore Database → Crear base de datos**.
2. Ubicación: elige una de Europa, por ejemplo `eur3 (europe-west)`. Siguiente.
3. Elige **Iniciar en modo de producción** y pulsa **Crear**.
4. Cuando se abra, ve a la pestaña **Reglas**. Borra lo que haya y pega el contenido del archivo `firestore.rules`:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

5. Pulsa **Publicar**. Con estas reglas, solo tú puedes ver y cambiar tus rutinas.

## 4. Registra la app web y copia su configuración en `config.js`

**a) Registra la app web**

1. Pulsa **Descripción general del proyecto** (*Project Overview*), arriba a la izquierda, para ir a la página principal del proyecto.
2. Busca los iconos de plataformas (iOS, Android, Web…) y pulsa el de **Web**, que tiene el símbolo **`</>`**.
   - Si no ves esos iconos, pulsa **+ Agregar app** (*Add app*) y elige **`</>` Web**.
3. Escribe un apodo, por ejemplo `Mis Rutinas`. **No** marques «Configurar Firebase Hosting». Pulsa **Registrar app** (*Register app*).

**b) Copia el bloque de configuración**

4. En la siguiente pantalla, «Agregar el SDK de Firebase», aparece un bloque de código. Da igual si está marcada la opción **npm** o **`<script>`**: en los dos casos verás unas líneas así:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "mis-rutinas-xxxx.firebaseapp.com",
  projectId: "mis-rutinas-xxxx",
  storageBucket: "mis-rutinas-xxxx.firebasestorage.app",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef123456"
};
```

5. Copia **solo ese trozo**: desde `const firebaseConfig = {` hasta el `};` de cierre. No copies las líneas de `import …` ni la de `const app = initializeApp(…)`. Si aparece también una línea `measurementId`, no pasa nada: puedes copiarla.
6. Pulsa **Ir a la consola** (*Continue to console*).

**c) Pégalo en `config.js`**

7. Abre `config.js` con el Bloc de notas (Windows) o TextEdit (Mac).
8. Borra el bloque de ejemplo, que empieza por `const firebaseConfig = {`, y pega el tuyo en su lugar. Guarda el archivo.

**¿Cerraste la pantalla sin copiarlo?** Puedes volver a verlo así:
pulsa el **engranaje ⚙** junto a «Descripción general del proyecto» → **Configuración del proyecto** → pestaña **General** → baja hasta **Tus apps** → selecciona tu app web → en «Configuración del SDK», marca **Config**. Ahí aparece el mismo bloque `const firebaseConfig = {…}`.

Estos datos no son secretos. Lo que protege tus rutinas son las reglas del paso 3.

## 5. Publica la app gratis en GitHub Pages

1. Entra en **https://github.com** y pulsa **New repository**.
2. Nombre: `mis-rutinas`. Déjalo como **Public** y pulsa **Create repository**.
3. En la página del repositorio pulsa **uploading an existing file**.
4. Arrastra **todos los archivos** de la carpeta `rutinas-app` (no la carpeta en sí), con el `config.js` ya editado, y pulsa **Commit changes**.
5. Ve a **Settings → Pages**. En «Branch» elige `main` y la carpeta `/ (root)`, y pulsa **Save**.
6. En uno o dos minutos aparecerá tu dirección, del tipo:
   **https://TU-USUARIO.github.io/mis-rutinas/**

## 6. Autoriza tu dirección en Firebase

1. Firebase → **Authentication → Configuración → Dominios autorizados → Agregar dominio**.
2. Escribe `TU-USUARIO.github.io` y guarda.

## 7. Instálala en el móvil

- **Android (Chrome):** abre tu dirección, pulsa el menú **⋮** y luego **Instalar aplicación** (o «Añadir a pantalla de inicio»).
- **iPhone (Safari):** abre tu dirección, pulsa **Compartir** y luego **Añadir a pantalla de inicio**.
- **Ordenador (Chrome o Edge):** en la barra de direcciones aparece un icono para instalarla.

## 8. Primera vez

Escribe tu email y una contraseña de al menos 6 caracteres y pulsa **Crear cuenta nueva**. En los demás dispositivos entra con los mismos datos y verás lo mismo.

---

## Preguntas frecuentes

**¿Y si borro las cookies o cambio de móvil?**
No pierdes nada. Vuelves a entrar con tu email y contraseña y todo está ahí.

**¿Cuesta algo?**
No. El plan gratuito de Firebase (Spark) permite decenas de miles de lecturas al día, mucho más de lo que usa una persona. GitHub Pages también es gratis.

**¿Cómo actualizo la app si hay cambios?**
Sube el nuevo `index.html` al repositorio de GitHub (Add file → Upload files) sin tocar tu `config.js`. La próxima vez que abras la app con conexión se actualizará sola.

**El temporizador no suena con el móvil bloqueado.**
Las apps web no pueden sonar con la pantalla apagada. Mientras corre un temporizador, la app mantiene la pantalla encendida. Si das permiso de notificaciones, también avisa con una notificación al acabar, siempre que el móvil no haya congelado la app. Para que la alarma sea fiable, deja la app abierta.

**¿Se puede probar antes de configurarlo?**
Sí. Abre `index.html` y pulsa «Probar sin cuenta». En ese modo los datos se guardan solo en ese navegador.
