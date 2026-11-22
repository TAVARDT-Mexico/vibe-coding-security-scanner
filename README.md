# Vibe Coding Security Scanner y Auditor de Aplicaciones Creadas con IA | TAVARDT México

[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Vibe Coding Security](https://img.shields.io/badge/Vibe%20Coding-Security%20Hardened-00d26a.svg)](README.md)
[![Language](https://img.shields.io/badge/Language-Español%20(México)-green.svg)](README.md)
[![Engineering](https://img.shields.io/badge/TAVARDT-Cybersecurity-5b3cf5.svg)](https://ag.tavardt.com/mx/ciberseguridad/)

Escáner open source para auditar aplicaciones desarrolladas con herramientas vibe coding (Cursor, Lovable, Bolt, v0). Detecta llaves API expuestas en frontend, brechas de RLS en Supabase y sourcemaps.

## Aplicación Web Interactiva

Acceda a la herramienta online y audite su pipeline en tiempo real:

- [https://tavardt-mexico.github.io/vibe-coding-security-scanner/](https://tavardt-mexico.github.io/vibe-coding-security-scanner/)

---

## 1. Visión General del Escáner de Seguridad Vibe Coding

El desarrollo asistido por inteligencia artificial (*vibe coding*) permite a emprendedores y equipos publicar plataformas funcionales mediante **Cursor, Lovable.dev, Bolt.new, v0 y Replit**.

No obstante, la ausencia de filtros de ciberseguridad suele dejar expuestas llaves maestras, bases de datos sin autenticación y código fuente sin ofuscar.

Este repositorio entrega una herramienta en Python para evaluar **la superficie pública de cualquier aplicación web activa**, localizando vulnerabilidades críticas antes de sufrir un ataque.

---

## 2. Vulnerabilidades y Vectores Auditados

| Vector de Riesgo | Causa en Vibe Coding | Impacto Técnico |
| :--- | :--- | :--- |
| **Llaves API en Frontend** | Colocación de llaves OpenAI (`sk-...`), Stripe o AWS en variables públicas del cliente. | Cualquier usuario extrae las llaves mediante DevTools y gasta su saldo. |
| **Supabase RLS Inactivo** | Tablas de base de datos creadas sin activar *Row Level Security* (RLS). | Con la `anon_key` pública cualquiera puede descargar la base de clientes vía REST. |
| **Sourcemaps en Producción** | El deploy incluye archivos `.js.map` en el servidor web. | El código fuente original queda a la vista de cualquier persona. |
| **Archivos Confidenciales Abiertos** | Rutas como `/.env` o `/.git/HEAD` accesibles vía HTTP. | Fuga de variables de entorno y credenciales de bases de datos. |
| **Ausencia de Cabeceras Seguras** | Omisión de políticas CSP, HSTS y X-Frame-Options. | Riesgo de Clickjacking e inyección de scripts externos. |

---

## 3. Guía de Instalación y Ejecución

### Instalación Rápida

```bash
git clone https://github.com/TAVARDT-Mexico/vibe-coding-security-scanner.git
cd vibe-coding-security-scanner
```

### Ejecutar Escaneo en una Aplicación

```bash
python scanner.py https://su-aplicacion.vercel.app
```

### Parámetros Disponibles

```bash
# Reporte estructurado en formato JSON
python scanner.py https://su-aplicacion.vercel.app --json

# Omitir inspección de archivos sourcemap
python scanner.py https://su-aplicacion.vercel.app --no-sourcemaps
```

---

## 4. Blindaje Perimetral y Soporte Técnico

Si su diagnóstico arrojó credenciales filtradas, tablas sin RLS en Supabase o requiere blindaje perimetral especializado:

* **Portal Corporativo:** [TAVARDT México](https://ag.tavardt.com/mx/ciberseguridad/)
* **Contacto Técnico Directo:** [Contactar con el Equipo de Ingeniería](https://ag.tavardt.com/mx/contacto/)

---

## Licencia

Distribuido bajo licencia de código abierto MIT. Consulte el archivo [`LICENSE`](LICENSE) para más detalles.  
Copyright (c) 2026 TAVARDT México.
