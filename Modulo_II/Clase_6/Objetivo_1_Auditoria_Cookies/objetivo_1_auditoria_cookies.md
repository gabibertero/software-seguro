# Objetivo 1: Auditoría de Cookies y Banderas de Seguridad

Revisé las cookies de tres sitios públicos desde el inspector del navegador. Anoté sus nombres y las banderas visibles, sin copiar los valores.

| Sitio | Cookie revisada | HttpOnly | Secure | SameSite | Observación |
|---|---|---|---|---|---|
| argentina.gob.ar | No encontré una cookie de sesión de la aplicación | — | — | — | Las cookies visibles del sitio eran de analítica. También aparecieron cookies de Cloudflare. |
| Mercado Libre | `ssid` | Sí | Sí | `None` | Tiene fecha de expiración futura. No inicié sesión, así que no confirmé si identifica una sesión autenticada. |
| Mercado Libre | `_csrf` | Sí | Sí | `None` | El inspector la muestra con duración de sesión. |
| Instagram | `sessionid` | Sí | Sí | No indicado | Tiene fecha de expiración futura. El inspector no mostró un valor para SameSite. |
| Instagram | `rur` | Sí | Sí | `Lax` | El inspector la muestra con duración de sesión. |

## ¿Qué protege HttpOnly?

La bandera `HttpOnly` impide que JavaScript lea directamente una cookie desde la página. Esto dificulta que un ataque XSS robe la cookie de sesión y la use para entrar como el usuario. No evita que el navegador envíe la cookie al sitio.

## Riesgo de no activar Secure

La bandera `Secure` hace que el navegador envíe la cookie solo por HTTPS. Si no está activada, la cookie podría viajar por una conexión HTTP sin cifrar y alguien que observe ese tráfico podría capturarla y reutilizar la sesión.

