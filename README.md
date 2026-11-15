# G.I.T · Grupo de Intervención Táctica

Sitio web de la unidad de Arma Reforger: personal, grados, cursos, misiones y una sección reservada para instructores y Estado Mayor.

Es un sitio estático que guarda sus datos en **Firebase (Cloud Firestore)**. Los cambios del administrador se publican en la base de datos y todos los visitantes los ven al instante, sin volver a subir archivos. Si Firebase no está configurado, el sitio sigue funcionando como antes con `data.json`.

## Archivos

| Archivo | Para qué sirve |
| --- | --- |
| `index.html` | La página completa (diseño, lógica y datos de respaldo). |
| `data.json` | Datos iniciales y de respaldo. Se usan para llenar Firestore la primera vez y si la base de datos no responde. |
| `firebase-config.js` | Tus claves de la app web de Firebase (no son secretas). |
| `firestore.rules` | Reglas de seguridad: todos leen, solo tu correo de administrador escribe. |
| `firebase.json` | Configuración para desplegar reglas y (opcional) el sitio con Firebase CLI. |
| `.nojekyll` | Evita que GitHub Pages procese el sitio con Jekyll. |

## Publicar en GitHub Pages

1. Crea un repositorio nuevo en GitHub (por ejemplo `git-arma-reforger`).
2. Pulsa **Add file → Upload files** y sube `index.html`, `data.json` y `.nojekyll`.
3. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` y guarda.
4. En un par de minutos el sitio queda en `https://TU-USUARIO.github.io/git-arma-reforger/`.

Con Git en la terminal:

```bash
git init
git add .
git commit -m "Sitio G.I.T"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/git-arma-reforger.git
git push -u origin main
```

## Configurar Firebase (una sola vez)

1. Entra a [console.firebase.google.com](https://console.firebase.google.com), **Agregar proyecto** y créalo (Analytics no es necesario).
2. **Compilación → Firestore Database → Crear base de datos**. Elige modo *producción* y una región (por ejemplo `southamerica-east1`).
3. **Compilación → Authentication → Comenzar → Correo electrónico/contraseña → Habilitar**. En la pestaña *Usuarios* pulsa **Agregar usuario** y crea tu cuenta de administrador (correo + contraseña larga). Después, en *Configuración → Acciones del usuario*, desactiva **Permitir crear cuentas** para que nadie más se registre.
4. **Configuración del proyecto (engranaje) → Tus apps → Web (`</>`)**, registra la app y copia el objeto `firebaseConfig` dentro de `firebase-config.js`.
5. **Firestore → Reglas**: pega el contenido de `firestore.rules`, cambia `TU_CORREO_ADMIN@gmail.com` por tu correo de administrador y pulsa **Publicar**. (Con Firebase CLI: `firebase deploy --only firestore:rules`.)
6. Si publicas en GitHub Pages, ve a **Authentication → Configuración → Dominios autorizados** y agrega `TU-USUARIO.github.io`.
7. Abre el sitio, pulsa **Acceso admin**, inicia sesión con tu correo y pulsa **Subir datos iniciales**. Desde ahí los datos viven en Firestore (colección `sitio`, documento `datos`).

## Actualizar los datos (integrantes, misiones, anuncios, cursos)

1. Pulsa **Acceso admin** e inicia sesión con tu correo de Firebase.
2. Haz los cambios con **Agregar** y **Editar**.
3. Pulsa **Publicar cambios**. Todos los visitantes ven la novedad al instante.

Si dos administradores editan a la vez, el segundo en publicar recibe un aviso y se carga la versión del primero (así nadie pisa cambios ajenos). **Descargar datos** genera una copia de seguridad en JSON.

## Seguridad

- El repositorio y el sitio son públicos. Todo lo que está en `data.json` lo puede leer cualquiera.
- No guardes en el repositorio correos, Steam ID ni nombres reales.
- Con Firebase, el acceso de administrador lo valida Firebase Authentication y la escritura la protegen las reglas de `firestore.rules` (solo los correos de la lista). Ya no hay una contraseña de administrador guardada en los datos públicos; la huella antigua que trae `data.json` queda sin uso (puedes borrar la clave `auth` de `settings`).
- La clave de la sección **Documentos** no está escrita en el código: el contenido se guarda cifrado. Usa una clave larga.
- Da acceso de escritura al repositorio de GitHub y al proyecto de Firebase solo a administradores.
- No subas documentos clasificados a este repositorio. La sección Documentos solo explica el procedimiento de entrega.
- El documento de Firestore tiene un límite de 1 MB: usa un escudo liviano.

## Cambiar el diseño

Todo el estilo y la lógica están dentro de `index.html`. Las fuentes (Zilla Slab y Special Elite) se cargan desde Google Fonts, por lo que se necesita conexión a internet.
