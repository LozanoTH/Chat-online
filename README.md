# Chat Web en Tiempo Real con Firebase

Proyecto de chat web en un solo archivo HTML, optimizado para uso mÃ³vil (especialmente Android WebView), con autenticaciÃ³n bÃ¡sica, CAPTCHA, presencia en lÃ­nea y mensajerÃ­a en tiempo real usando Firebase Realtime Database.

---

## DescripciÃ³n general

Este proyecto implementa un sistema de chat en tiempo real sin backend propio. Toda la lÃ³gica vive en el frontend y Firebase actÃºa como base de datos, sistema de presencia y sincronizaciÃ³n.

CaracterÃ­sticas clave:
- Login y registro de usuarios
- CAPTCHA simple anti-bots
- Hash de contraseÃ±as con SHA-256
- Chat en tiempo real
- Contador de usuarios en lÃ­nea
- Presencia online/offline
- UI adaptada a pantallas mÃ³viles
- Soporte para modo oscuro automÃ¡tico
- Optimizado para Android WebView

---

## TecnologÃ­as usadas

- HTML5
- CSS3 (responsive, mobile-first)
- JavaScript vanilla
- Firebase Realtime Database
- CryptoJS (hash SHA-256)

No se usan frameworks. Todo es puro y directo.

---

## Estructura del proyecto

```text
/
â”œâ”€â”€ chat.html        Archivo principal (UI + lÃ³gica)
â””â”€â”€ README.md        DocumentaciÃ³n
```

No hay carpetas extra. No hay build. No hay dependencias locales.

---

## Funcionamiento general

El flujo del sistema es el siguiente:

1. El usuario abre `chat.html`
2. Se muestra la pantalla de login/registro
3. El usuario pasa el CAPTCHA
4. Se valida contra Firebase
5. Se inicia sesiÃ³n
6. Se establece presencia online
7. Se sincronizan mensajes en tiempo real

Todo ocurre del lado del cliente.

---

## AutenticaciÃ³n

### Registro

- Usuario entre 3 y 20 caracteres
- Solo letras, nÃºmeros y `_`
- ContraseÃ±a mÃ­nimo 6 caracteres
- CAPTCHA obligatorio
- ContraseÃ±a almacenada como hash SHA-256

Los datos se guardan en:

```text
android_chat/users/{username}
```

Ejemplo:

```json
{
  "username": "usuario1",
  "password": "hash_sha256",
  "color": "hsl(120, 70%, 60%)",
  "created": 1730000000000
}
```

---

### Login

- VerificaciÃ³n de usuario existente
- ComparaciÃ³n de hash
- CAPTCHA obligatorio
- Bloqueo temporal tras varios intentos fallidos

La sesiÃ³n se recuerda en `localStorage`.

---

## CAPTCHA

CAPTCHA simple generado en frontend:

- 5 caracteres
- Letras y nÃºmeros sin caracteres ambiguos
- RegeneraciÃ³n manual
- RegeneraciÃ³n automÃ¡tica tras error

No es criptogrÃ¡ficamente seguro. Es disuasivo, no infalible.

---

## Chat en tiempo real

### EnvÃ­o de mensajes

Cada mensaje contiene:

```json
{
  "username": "usuario",
  "text": "mensaje",
  "color": "hsl(...)",
  "timestamp": 1730000000000
}
```

Ruta en Firebase:

```text
android_chat/messages
```

LÃ­mite:
- 500 caracteres por mensaje
- Se cargan solo los Ãºltimos 50 mensajes

---

### RecepciÃ³n

- Listener `child_added`
- Render inmediato
- Scroll automÃ¡tico suave
- DiferenciaciÃ³n visual entre mensajes propios y ajenos

---

## Presencia online

Sistema de presencia usando `onDisconnect()`.

Ruta:

```text
android_chat/online/{username}
```

Estados:
- online
- offline

Se actualiza automÃ¡ticamente al cerrar pestaÃ±a, perder conexiÃ³n o salir.

---

## Contador de usuarios en lÃ­nea

- Escucha global a `android_chat/online`
- Cuenta solo usuarios con estado `online`
- ActualizaciÃ³n en tiempo real

---

## Limpieza automÃ¡tica

Cada hora:
- Se revisa el total de mensajes
- Si hay mÃ¡s de 100:
  - Se conservan los Ãºltimos 50
  - El resto se elimina

Evita crecimiento infinito de la base de datos.

---

## Seguridad

Lo que SÃ hace:
- Hash de contraseÃ±as
- Escape bÃ¡sico de HTML
- CAPTCHA
- LÃ­mites de longitud

Lo que NO hace:
- No usa Firebase Auth
- No tiene reglas avanzadas de seguridad
- No protege contra ataques avanzados
- No cifra mensajes

No es para producciÃ³n crÃ­tica.

---

## Requisitos

- Cuenta Firebase
- Realtime Database habilitada
- Reglas mÃ­nimas de lectura/escritura

Ejemplo simple (NO segura):

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

---

## Uso en Android WebView

Funciona correctamente en:
- WebView Android
- Navegadores mÃ³viles
- Escritorio

Recomendado:
- Desactivar zoom
- Habilitar JavaScript
- Mantener HTTPS

---

## Limitaciones conocidas

- Seguridad bÃ¡sica
- CAPTCHA dÃ©bil
- Sin roles
- Sin salas
- Sin archivos
- Sin cifrado end-to-end

Es un chat simple. Nada mÃ¡s.

---

## Casos de uso

- Prototipos
- Pruebas de Firebase
- Chats internos
- Aprendizaje
- WebView apps

No usar para datos sensibles.

---

## Licencia

Uso libre.
Modifica.
Rompe.
Aprende.

Fin.
