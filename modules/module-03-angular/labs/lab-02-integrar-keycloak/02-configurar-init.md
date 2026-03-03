# Lab 02 — Integrar Keycloak (Bootstrap Manual)

## Paso 02 — Configurar e integrar la inicialización

---

## 🎯 Objetivo

Configurar correctamente la inicialización de Keycloak y asegurarnos de que:

* Keycloak se inicializa antes de que Angular arranque.
* Se procesan posibles redirecciones desde el servidor.
* El estado de autenticación queda disponible desde el inicio.

En este paso **no forzaremos login obligatorio todavía**.
Queremos entender primero el proceso de bootstrap.

---

## 📍 Ubicación

Dentro de:

```
apps/angular-app/angular-app/src/app
```

---

# 🛠 Paso 1 — Crear archivo de configuración

Crear:

```
keycloak.config.ts
```

Contenido:

```typescript
export const keycloakConfig = {
  url: 'http://localhost:8080',
  realm: 'training',
  clientId: 'spa-client'
};
```

---

## 🧠 Por qué separar la configuración

Separar configuración permite:

* Cambiar entorno fácilmente (dev / prod)
* No mezclar credenciales con lógica
* Reutilizar en futuras integraciones (wrapper)

---

# 🛠 Paso 2 — Crear servicio de inicialización

Crear:

```
keycloak.service.ts
```

Contenido:

```typescript
import Keycloak from 'keycloak-js';
import { keycloakConfig } from './keycloak.config';

let keycloak: Keycloak;

export function initializeKeycloak(): Promise<boolean> {
  keycloak = new Keycloak({
    url: keycloakConfig.url,
    realm: keycloakConfig.realm,
    clientId: keycloakConfig.clientId
  });

  return keycloak.init({
    onLoad: 'check-sso',
    pkceMethod: 'S256',
    checkLoginIframe: false
  });
}

export function getKeycloakInstance(): Keycloak {
  return keycloak;
}
```

---

## 🧠 Qué hace init()

`keycloak.init()`:

1. Procesa si la URL contiene `?code=...`
2. Intercambia el authorization code por tokens
3. Valida sesión existente
4. Deja disponible:

```typescript
keycloak.authenticated
keycloak.token
keycloak.tokenParsed
```

---

## 🔎 Por qué usamos `check-sso`

Usamos:

```typescript
onLoad: 'check-sso'
```

Porque:

* No fuerza login automático.
* Permite que la aplicación arranque sin sesión.
* Nos permite observar el estado antes de proteger rutas.

Más adelante cambiaremos a `login-required`.

---

# 🛠 Paso 3 — Integrar en el arranque (bootstrap real)

Abrir:

```
src/main.ts
```

Reemplazar por:

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { App } from './app/app';
import { initializeKeycloak } from './keycloak.service';


initializeKeycloak().then(() => {
  console.log('Keycloak initialized successfully');
  bootstrapApplication(App, appConfig)
  .catch((err) => console.error(err));
}).catch(err => {
    console.error('Keycloak init failed', err);
});
```

---

## 🧠 Qué está pasando realmente

ANTES de que Angular arranque:

1. Se inicializa Keycloak.
2. Se valida si existe sesión.
3. Se procesan posibles tokens.
4. Solo entonces Angular se monta.

Esto evita:

* Guards ejecutándose antes de tiempo.
* Loops de login.
* Estados inconsistentes.

Esto es bootstrapear autenticación manualmente.

---

# ✅ Estado esperado tras este paso

✔ keycloak-js instalado
✔ Configuración separada
✔ Servicio de inicialización creado
✔ Angular bloquea su arranque hasta que Keycloak resuelve

Si ejecutas la aplicación ahora, debería arrancar sin forzar login.

---

## ➡️ Siguiente paso

En:

```
03-validar-login.md
```

Añadiremos botones de login y logout para observar el flujo Authorization Code + PKCE en acción.
