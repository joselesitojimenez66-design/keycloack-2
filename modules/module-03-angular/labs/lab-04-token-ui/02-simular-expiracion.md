# Lab 04 — Token UI

## Paso 02 — Simular expiración del token

---

## 🎯 Objetivo

Observar qué ocurre cuando el `access_token` está cerca de expirar y cómo podemos detectar su estado desde Angular.

En este paso:

* Inspeccionaremos el tiempo de expiración (`exp`)
* Calcularemos cuánto tiempo queda
* Veremos el comportamiento del wrapper

---

# 🧠 Contexto

El JWT contiene un claim:

```json
"exp": 1712345678
```

Ese valor representa:

> Fecha de expiración en formato Unix timestamp (segundos).

Si el token expira y no se refresca:

* Las llamadas al backend fallarán.
* El usuario deberá volver a autenticarse.

---

# ✍️ Modificar ProtectedComponent

Abrir:

```id="8qv0pu"
src/app/protected/protected.component.ts
```

Actualizar el componente:

```typescript id="sx51yf"
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

      <h3>Token Expiration</h3>
      <p>Expira en: {{ secondsRemaining }} segundos</p>
    </div>
  `
})
export class ProtectedComponent implements OnInit {

  private keycloakService = inject(KeycloakService);

  tokenParsed: any;
  secondsRemaining: number = 0;

  ngOnInit(): void {
    const keycloak = this.keycloakService.getKeycloakInstance();
    this.tokenParsed = keycloak.tokenParsed;

    if (this.tokenParsed?.exp) {
      const now = Math.floor(Date.now() / 1000);
      this.secondsRemaining = this.tokenParsed.exp - now;
    }
  }
}
```

---

# ▶️ Reiniciar aplicación

```bash id="23ypzi"
npm start
```

Ir a:

```text
http://localhost:4200/protected
```

---

# 🔎 Qué deberías ver

Debajo de los claims aparecerá algo como:

```
Expira en: 280 segundos
```

Ese número disminuirá si recargas la página.

---

# 🧠 Qué estamos validando

✔ El token contiene información de expiración (`exp`)
✔ Angular puede calcular el tiempo restante
✔ El estado del token es completamente visible en cliente

---

# ⚠️ Importante

En este momento:

* NO estamos refrescando el token manualmente.
* Solo estamos observando su expiración.

En el siguiente paso veremos cómo el wrapper permite refrescar el token automáticamente sin que el usuario lo perciba.

---

# 📌 Estado del laboratorio

Ya sabemos:

* Cómo acceder al token.
* Cómo leer sus claims.
* Cómo calcular su expiración.
* Qué ocurre cuando está cerca de expirar.