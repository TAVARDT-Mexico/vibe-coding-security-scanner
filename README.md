# Auditor de Seguridad Vibe Coding y Arquitectura de IA

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-5b3cf5.svg)](https://tavardt-mexico.github.io/vibe-coding-security-scanner/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Security](https://img.shields.io/badge/Security-AI%20Architecture-blue.svg)](https://tavardt-mexico.github.io/vibe-coding-security-scanner/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Guía corporativa de arquitectura defensiva, mitigación de fuga de credenciales y herramienta CLI determinista para auditoría de aplicaciones desarrolladas con herramientas de IA (Vibe Coding).

## Superficie de Riesgo en Aplicaciones Creadas con IA

El desarrollo asistido por IA (Cursor, Lovable, v0, Bolt) introduce tres vulnerabilidades estructurales recurrentes:

1. **Fuga de Sourcemaps (.js.map):** Despliegues en producción que conservan mapas de código activos permiten reconstruir íntegramente la lógica de negocio y esquemas de datos originales en TypeScript.
2. **Exposición de Claves de IA en Bundles Client-Side:** Claves de OpenAI, Anthropic o tokens `service_role` de Supabase con prefijo `NEXT_PUBLIC_` o `VITE_` se descargan en el código estático por cualquier navegador.
3. **Ausencia de Row Level Security (RLS):** Bases de datos relacionales consultadas directamente mediante endpoints REST/GraphQL sin políticas de RLS activas permiten la extracción no autorizada de datos de múltiples usuarios.

***

## Herramienta Oficial de Auditoría CLI

Para evitar restricciones de sandbox de navegadores y políticas de CORS, TAVARDT ofrece una utilidad en Python para inspección directa por terminal o pipeline de CI/CD:

```bash
# 1. Obtener el script oficial
curl -sO https://raw.githubusercontent.com/TAVARDT-Mexico/vibe-coding-security-scanner/main/audit_vibe_security.py

# 2. Ejecutar auditoria directa contra la aplicacion
python audit_vibe_security.py https://su-aplicacion.com
```

### Capacidades de Inspección del Utilitario CLI

* **Auditoría de Sourcemaps:** Rastreo sistemático en directorios de compilación estándar para detectar archivos `.js.map` expuestos públicamente.
* **Inspección de Cabeceras HTTP:** Validación estricta de políticas `Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options` y `X-Frame-Options`.
* **Detección de Secretos Compilados:** Escaneo avanzado con expresiones regulares para localizar tokens de proveedores de IA y credenciales administrativas en bundles estáticos.

***

## Blueprints de Mitigación Defensiva

El repositorio incluye plantillas técnicas homologadas para blindar su arquitectura:

* **Vite (`vite.config.ts`):** Desactivación estricta de sourcemaps en producción y sanitización de nombres de paquetes.
* **Next.js (`next.config.js`):** Desactivación de `productionBrowserSourceMaps` e imposición de cabeceras defensivas.
* **Supabase SQL (`supabase-rls.sql`):** Activación obligatoria de `ROW LEVEL SECURITY` en esquemas públicos y revocación de permisos indiscriminados del rol `anon`.
* **Cloudflare WAF:** Reglas perimetrales para bloquear inmediatamente peticiones hacia archivos `.js.map`, `.env` y `.git`.

***

## Marcos de Cumplimiento y Estándares

Arquitectura defensiva y controles de ciberseguridad alineados con la LFPDPPP, directrices de la Guardia Nacional CERT-MX y estándares internacionales.

***

© 2026 TAVARDT. Todos los derechos reservados.
