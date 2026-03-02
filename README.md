# Formación Técnica — Administración y Uso de Keycloak

## 🎯 Objetivo

Gestionar una plataforma de identidades federadas y control de acceso basada en Keycloak, integrándola en un ecosistema moderno de microservicios con aplicaciones frontend, backend, federación de identidades y despliegue seguro.

El enfoque es eminentemente práctico (70%), mediante laboratorios atómicos por módulo, ejecutados completamente en contenedores dentro de GitHub Codespaces.

---

# 🏗 Arquitectura del Entorno de Laboratorio

Todo el entorno se ejecuta en Docker dentro de GitHub Codespaces.

Componentes del ecosistema:

- Keycloak (versión actual - Quarkus)
- Base de datos (evolución a PostgreSQL)
- Aplicación Angular (SPA protegida con OIDC + PKCE)
- API Node.js protegida por JWT
- OpenLDAP (federación)
- NGINX reverse proxy (fase avanzada)
- Escenario de alta disponibilidad (módulo final)

La arquitectura evoluciona progresivamente a lo largo del curso.

---

# 📚 Estructura del Curso por Módulos

El curso está dividido en 8 módulos progresivos.

Cada módulo incluye:

- Material teórico en Markdown
- Laboratorios prácticos guiados
- Ejercicios técnicos
- Scripts y configuraciones listas para ejecutar

No existe proyecto final.  
Los laboratorios son atómicos entre módulos y encadenables dentro del mismo módulo cuando sea necesario.

---

# 🧱 MÓDULO 1 — Fundamentos IAM y Arquitectura

## Contenidos

- Conceptos modernos de IAM
- OAuth2 vs OpenID Connect
- Tokens (Access, ID, Refresh)
- Flujos de autenticación
- Arquitectura interna de Keycloak
- Instalación inicial en modo desarrollo

## Laboratorios

- Levantar Keycloak en Docker (modo dev)
- Crear primer Realm
- Crear cliente OIDC
- Ejecutar Authorization Code Flow manual
- Inspección y análisis de tokens

---

# 🧱 MÓDULO 2 — Gestión de Realms y Autorización

## Contenidos

- Realm roles vs Client roles
- Grupos y roles compuestos
- Service Accounts
- Policies y Permissions
- Modelado organizativo

## Laboratorios

- Modelado jerárquico de roles
- Autorización basada en recursos
- Client Credentials Flow
- Simulación multi-realm

---

# 🧱 MÓDULO 3 — Integración Angular (OIDC + PKCE)

## Contenidos

- Integración con keycloak-js
- Authorization Code Flow con PKCE
- Guards de rutas
- Ciclo de vida del token
- Refresh automático
- Logout centralizado

## Laboratorios

- Conectar SPA Angular standalone a Keycloak
- Proteger rutas mediante guard
- Mostrar información del token en UI
- Simular expiración y refresh
- Logout global

---

# 🧱 MÓDULO 4 — Backend Node Protegido

## Contenidos

- Bearer tokens
- Validación JWT
- JWKS
- Protección por roles y scopes
- Autenticación service-to-service

## Laboratorios

- Crear API Node protegida
- Middleware de validación JWT
- Protección por roles
- Testing con curl y Postman
- Flujo service account

---

# 🧱 MÓDULO 5 — Login Flows, MFA y Personalización

## Contenidos

- Authentication flows
- Required actions
- MFA (TOTP)
- Gestión avanzada de usuarios
- Themes personalizados
- Extensiones básicas (introducción a SPI)

## Laboratorios

- Crear flow personalizado
- Activar MFA obligatorio
- Crear tema custom
- Añadir atributos personalizados
- Probar flujos alternativos

---

# 🧱 MÓDULO 6 — LDAP, Federation e Introducción a SAML

## Contenidos

- User Federation
- Integración con LDAP
- Sincronización de usuarios
- Identity Brokering
- Arquitecturas híbridas
- Introducción conceptual a SAML
  - Assertion
  - SP vs IdP
  - Metadata XML
  - Comparativa con OIDC
  - Casos enterprise

## Laboratorios

- Levantar OpenLDAP en Docker
- Conectar LDAP a Keycloak
- Sincronización de usuarios
- Configurar Identity Brokering entre realms

(SAML se aborda únicamente a nivel conceptual, sin laboratorio práctico.)

---

# 🧱 MÓDULO 7 — Seguridad Avanzada y Reverse Proxy

## Contenidos

- PKCE en profundidad
- Estrategias de expiración de tokens
- SSL y certificados
- Reverse proxy con NGINX
- Headers X-Forwarded
- Hardening básico

## Laboratorios

- Desplegar Keycloak detrás de NGINX
- Configurar HTTPS local
- Ajustar headers correctamente
- Configurar expiraciones seguras
- Simular escenarios de ataque básicos

---

# 🧱 MÓDULO 8 — Producción, Alta Disponibilidad y Mantenimiento

## Contenidos

- Migración a PostgreSQL
- Configuración de entorno productivo
- Clustering básico
- Exportación e importación de realms
- Backup y recuperación
- Health checks
- Integración con API Gateway (conceptual)

## Laboratorios

- Migrar Keycloak a PostgreSQL
- Ejecutar múltiples instancias
- Probar sesión compartida
- Exportar e importar realms
- Verificar endpoints de salud

---

# 🐳 Ejecución del Entorno

Los alumnos deben:

1. Hacer fork del repositorio
2. Abrirlo en GitHub Codespaces
3. Ejecutar:

    docker compose up

El entorno completo se levantará automáticamente mediante contenedores.

---

# 📌 Enfoque Metodológico

- 70% práctica real
- Laboratorios técnicos guiados
- Ecosistema de microservicios
- Evolución desde entorno simple a producción
- Preparación para entornos empresariales reales

---

# 🔐 Competencias que se adquieren

- Gestión avanzada de Keycloak
- Modelado de autorización empresarial
- Integración SPA + API con OIDC
- Seguridad y hardening en producción
- Federación de identidades
- Arquitecturas con alta disponibilidad
