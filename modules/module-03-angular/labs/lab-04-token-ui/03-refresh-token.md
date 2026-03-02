# Lab 04 — Token UI

## Paso 03 — Refresco automático del token

---

## 🎯 Objetivo

Configurar el refresco automático del token para:

* Evitar que el usuario pierda sesión al expirar el `access_token`.
* Renovar el token silenciosamente en segundo plano.
* Comprender cómo el wrapper simplifica esta tarea.

---

# 🧠 Contexto

Un `access_token`:

* Tiene una duración limitada (por ejemplo 5 minutos).
* Cuando expira, deja de ser válido.
* Puede renovarse usando el `refresh_token`.

Sin refresco automático:

* Las llamadas al backend fallarán.
* El usuario tendrá que volver a autenticarse.

Con el wrapper:

👉 Podemos actualizar el token antes de que expire.

---

# 🛠 Paso 1 — Configurar actualización periódica

Abrir:

```text
src/app/protected/protected.component.ts
```

Actualizar el componente:

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
    this.updateTokenInfo();

    // Intentar refrescar si faltan menos de 30 segundos
    setInterval(async () => {
      const refreshed = await this.keycloakService.updateToken(30);

      if (refreshed) {
        console.log('Token refrescado automáticamente');
      }

      this.updateTokenInfo();
    }, 10000);
  }

  updateTokenInfo() {
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

```bash
npm start
```

Ir a:

```
http://localhost:4200/protected
```

---

# 🔎 Qué observar

1. El contador de expiración disminuirá.
2. Cuando queden menos de 30 segundos:

   * El token se refrescará automáticamente.
   * Verás en consola:

```
Token refrescado automáticamente
```

3. El contador volverá a aumentar.

---

# 🧠 Qué está ocurriendo internamente

```typescript
await this.keycloakService.updateToken(30);
```

Significa:

> Si el token expira en menos de 30 segundos, renuevalo.

Internamente:

* Se usa el `refresh_token`.
* No hay redirección.
* El usuario no percibe nada.

---

# 📊 Qué hemos conseguido

✔ Sesión persistente sin interrupciones
✔ Renovación automática del token
✔ Mejor experiencia de usuario
✔ Arquitectura lista para backend seguro

---

# 🔥 Comparativa con integración manual

Sin wrapper, tendríamos que:

* Controlar intervalos manualmente.
* Acceder directamente a `keycloak.updateToken()`.
* Manejar errores manualmente.
* Gestionar estados inconsistentes.

Con wrapper:

* Código más limpio.
* Integración Angular-friendly.
* Menos riesgo de errores.

---

# 📌 Estado final del Lab 04

Ahora el alumno entiende:

1️⃣ Qué contiene el token.
2️⃣ Cómo expira.
3️⃣ Cómo renovarlo automáticamente.
4️⃣ Por qué el wrapper es útil en producción.