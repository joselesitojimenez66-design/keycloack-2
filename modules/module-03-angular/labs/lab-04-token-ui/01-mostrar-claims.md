# Lab 04 — Token UI

## Paso 01 — Mostrar los Claims del Token en la UI (Wrapper Oficial)

---

## 🎯 Objetivo

Mostrar en la interfaz Angular los **claims del access token** utilizando el wrapper oficial `keycloak-angular`.

En este laboratorio ya trabajamos exclusivamente con la integración profesional basada en:

```text
keycloak-angular
```

---

## 📍 Ubicación

Dentro de:

```
apps/angular-app/angular-app
```

---

# 🧠 Contexto

En el laboratorio anterior:

* Eliminamos el bootstrap manual.
* Eliminamos `keycloak.service.ts`.
* Sustituimos la integración por `provideKeycloak`.
* Simplificamos el guard.

Ahora veremos cómo acceder al token usando el wrapper.

---

# ✍️ Modificar componente Protected

Abrir:

```
src/app/protected/protected.component.ts
```

Reemplazar contenido por:

```typescript
import { Component, OnInit, inject } from '@angular/core';
import { KeycloakService } from 'keycloak-angular';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-protected',
  standalone: true,
  imports: [CommonModule],
  template: `
    <h2>Protected Area</h2>

    <div *ngIf="tokenParsed">
      <h3>Access Token Claims</h3>
      <pre>{{ tokenParsed | json }}</pre>
    </div>
  `
})
export class ProtectedComponent implements OnInit {

  private keycloakService = inject(KeycloakService);

  tokenParsed: any;

  ngOnInit(): void {
    const keycloak = this.keycloakService.getKeycloakInstance();
    this.tokenParsed = keycloak.tokenParsed;
  }
}
```

---

# ▶️ Reiniciar aplicación

```bash
npm start
```

---

# 🌐 Validar

Accede a:

```
http://localhost:4200/protected
```

Si no estás autenticado, se forzará login automáticamente (por `login-required`).

Tras autenticación, deberías ver:

* El título **Protected Area**
* Un bloque JSON con los claims del token

---

# 🔎 Qué estamos viendo

El objeto mostrado corresponde al contenido decodificado del JWT:

* `preferred_username`
* `email`
* `realm_access`
* `resource_access`
* `exp`
* `iat`
* `iss`
* `aud`

---

# 🧠 Qué estamos validando

✔ El wrapper expone la instancia interna de Keycloak.
✔ Angular puede acceder al `access_token`.
✔ Los claims del JWT están disponibles en cliente.
✔ Podemos usar la información del token para autorización en UI.

---

# 📌 Estado del módulo ahora

* Autenticación profesional con wrapper.
* Guard limpio.
* Acceso a token desde Angular.
* Base preparada para:

  * Expiración
  * Refresh automático
  * Logout centralizado