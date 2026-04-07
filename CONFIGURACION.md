# 🔥 Guía de configuración Firebase — Finca Santa Rosa

## PASO 1 — Obtener tu firebaseConfig

1. Ve a https://console.firebase.google.com
2. Selecciona tu proyecto
3. Haz clic en el ícono ⚙️ (Configuración del proyecto)
4. Baja hasta "Tus apps" → si no hay ninguna, haz clic en "</>" (Agregar app web)
5. Nombre: "Finca Santa Rosa" → Registrar app
6. Copia el bloque `firebaseConfig` que aparece, por ejemplo:

```js
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXX",
  authDomain: "mi-finca-xxxxx.firebaseapp.com",
  projectId: "mi-finca-xxxxx",
  storageBucket: "mi-finca-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

7. Abre el archivo `index.html` y busca esta sección:

```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROJECT_ID.firebaseapp.com",
  ...
```

Reemplázala con TUS valores reales.

---

## PASO 2 — Activar Google Sign-In

1. En Firebase Console → Authentication → Comenzar
2. Pestaña "Sign-in method"
3. Clic en "Google" → Activar → Guardar
4. En "Correo electrónico de soporte del proyecto" pon tu correo

---

## PASO 3 — Crear la base de datos Firestore

1. Firebase Console → Firestore Database → Crear base de datos
2. Selecciona "Comenzar en modo de producción"
3. Elige la región: `us-central1` (o la más cercana)
4. Haz clic en Crear

### Aplicar reglas de seguridad:
1. Dentro de Firestore → pestaña "Reglas"
2. Reemplaza el contenido con el archivo `firestore.rules` incluido:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /finca/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

3. Publicar

---

## PASO 4 — Activar Firebase Hosting

### Instalar Firebase CLI (una sola vez):
```bash
npm install -g firebase-tools
```

### Iniciar sesión:
```bash
firebase login
```

### Publicar la app:
1. Pon todos los archivos en una carpeta: `index.html`, `manifest.json`, `icon-192.png`, `apple-touch-icon.png`, `sw.js`, `firebase.json`, `firestore.rules`
2. Abre la terminal en esa carpeta y ejecuta:

```bash
firebase deploy --only hosting
```

3. Al terminar verás algo como:
   ```
   Hosting URL: https://mi-finca-xxxxx.web.app
   ```

¡Esa es la URL de tu app! Compártela con la familia.

---

## PASO 5 — Agregar dominio autorizado (para Google Sign-In)

1. Firebase Console → Authentication → Settings → Dominios autorizados
2. Agrega tu dominio .web.app (ya aparece automático)
3. Si usás un dominio propio, agrégalo también

---

## ESTRUCTURA de datos en Firestore

```
finca/
  datos/
    cows/          ← Animales del hato
    lecheReg/      ← Registros de ordeño
    vetReg/        ← Registros veterinarios
    bajas/         ← Animales dados de baja
    entregas/      ← Entregas de leche
```

---

## PRIMERA VEZ — Datos de ejemplo

La primera vez que alguien inicie sesión con una base de datos vacía,
la app detecta que no hay vacas y carga automáticamente los datos de ejemplo.

---

## ¿Problemas?

- **Error de dominio no autorizado**: Agregá la URL en Authentication → Dominios autorizados
- **Permission denied en Firestore**: Verificá que las reglas estén bien aplicadas
- **Popup bloqueado**: En iOS, asegurate de abrir la app desde Safari (no Chrome)
