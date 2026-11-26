<p align="center">
  <a href="https://ag.tavardt.com/mx/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# Auditor de Seguridad Vibe Coding y Arquitectura de IA

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-5b3cf5.svg)](https://tavardt-mexico.github.io/vibe-coding-security-scanner/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Security](https://img.shields.io/badge/Security-AI%20Architecture-blue.svg)](https://tavardt-mexico.github.io/vibe-coding-security-scanner/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Guía corporativa de ingeniería defensiva, mitigación de fuga de credenciales y herramienta CLI determinista para auditoría de seguridad en aplicaciones generadas con asistentes de inteligencia artificial (Vibe Coding).

***

## Panorama Ejecutivo: La Realidad de la Seguridad en el Vibe Coding

La adopción generalizada del desarrollo asistido por inteligencia artificial (mediante plataformas como Cursor, Lovable, v0, Bolt, Windsurf, Replit, Claude Code y GitHub Copilot) permite a equipos técnicos y fundadores desplegar aplicaciones completas con una velocidad sin precedentes. Sin embargo, los generadores automáticos de código optimizan la funcionalidad visual inmediata en lugar del aislamiento perimetral, la arquitectura de mínimo privilegio o el saneamiento de artefactos en producción.

Al priorizar la velocidad de entrega, el software generado hereda de manera constante vulnerabilidades estructurales críticas:
* Fugas de configuraciones de desarrollo y depuración hacia compilaciones en producción.
* Claves maestras de bases de datos y tokens de IA expuestos en paquetes estáticos descargados por el navegador.
* Tablas en bases de datos relacionales creadas sin reglas de autorización declarativa (Row Level Security).
* Omisión de cabeceras HTTP de protección perimetral en servidores y proxies inversos.

Este repositorio establece un estándar de arquitectura defensiva empresarial, modelos de configuración listos para producción en frameworks modernos y una utilidad determinista en Python (`audit_vibe_security.py`) para identificar y remediar estas fallas antes del despliegue público.

***

## Superficie Detallada de Riesgo en Aplicaciones Generadas por IA

### 1. Exposición de Sourcemaps en Producción (.js.map)
Las herramientas de IA suelen configurar empaquetadores frontend (Vite, Webpack, Turbopack, Rollup) con valores predeterminados que emiten mapas de código fuente junto con los scripts minificados. Al publicarse en servidores web o redes de distribución de contenido (CDN), los archivos `.js.map` permiten la desofuscación completa del código:
* Reconstrucción exacta del código fuente original en TypeScript y JavaScript.
* Visibilidad total de modelos de datos internos, esquemas de bases de datos e interfaces de API.
* Exposición de rutas administrativas ocultas, endpoints internos y comentarios técnicos del desarrollo.

### 2. Claves de Proveedores de IA y Tokens BaaS Compilados en el Cliente
Dado que los motores de IA con frecuencia carecen de contexto sobre la separación física entre el servidor y el navegador del usuario, las credenciales confidenciales se insertan directamente en el código del cliente. Cuando se utilizan variables con prefijos como `NEXT_PUBLIC_` o `VITE_`, sus valores literales se incrustan en los paquetes JavaScript estáticos:
* Claves de API de OpenAI, Anthropic y Google Gemini sujetas a consumo no autorizado.
* Tokens administrativos `service_role` y JWT de Supabase que eluden por completo el control de acceso.
* Claves de acceso a la nube AWS, credenciales de Google Cloud y webhooks de pago.

### 3. Ausencia de Row Level Security (RLS) en Bases de Datos Relacionales
El ecosistema de Vibe Coding fomenta el acceso directo del cliente a la base de datos mediante SDKs modernos. No obstante, las herramientas de IA instancian rutinariamente tablas en PostgreSQL sin emitir `ALTER TABLE ... ENABLE ROW LEVEL SECURITY;`. Sin políticas de RLS activas:
* Cualquier usuario que inspeccione la URL del proyecto puede ejecutar consultas directas vía REST o GraphQL.
* Bases de datos completas con información confidencial, registros personales y registros comerciales quedan expuestas a solicitudes anónimas.

### 4. Omisión de Cabeceras HTTP de Seguridad Perimetral
Las aplicaciones generadas automáticamente rara vez implementan cabeceras de seguridad HTTP por defecto:
* `Content-Security-Policy` (CSP): Esencial para bloquear la inyección y ejecución de scripts maliciosos de terceros.
* `Strict-Transport-Security` (HSTS): Exige el uso exclusivo de transporte cifrado HTTPS.
* `X-Frame-Options` y `X-Content-Type-Options`: Previenen ataques de clickjacking y confusión de tipos MIME.

### 5. Rutas Expuestas de Archivos de Entorno (.env)
Configuraciones incorrectas en el servidor web permiten el acceso directo a rutas como `/.env`, `/.env.local` y `/.env.production`, facilitando la extracción automatizada de credenciales con una simple petición HTTP.

***

## Herramienta Oficial de Auditoría CLI

Las aplicaciones en navegadores web operan bajo estrictas restricciones de aislamiento y políticas de CORS que impiden a los scripts del cliente inspeccionar cabeceras arbitrarias o evaluar dominios externos. Para ofrecer diagnósticos técnicos reproducibles y con cero telemetría externa, TAVARDT proporciona una utilidad en Python orientada a la ejecución en terminal y flujos de CI/CD.

### Flujo de Auditoría Determinista

La herramienta (`audit_vibe_security.py`) ejecuta un proceso estructurado en cinco fases:
1. **Auditoría de Perímetro Raíz:** Establece conexión TLS, valida códigos de respuesta y verifica cabeceras defensivas de seguridad.
2. **Descubrimiento de Scripts en el DOM:** Analiza el marcado HTML para identificar dinámicamente los scripts cargados en la aplicación.
3. **Comprobación de Sourcemaps:** Evalúa la disponibilidad pública de archivos `.js.map` correspondientes a cada script detectado.
4. **Inspección de Secretos en Bundles:** Aplica expresiones regulares de alta precisión para detectar tokens activos de IA y claves de infraestructura.
5. **Escaneo de Rutas de Entorno:** Comprueba si los archivos estructurales de entorno (`/.env`, `/.env.local`, `/.env.production`) se encuentran debidamente protegidos.

### Instrucciones de Uso

Descargue y ejecute la utilidad directamente contra el endpoint objetivo:

```bash
# 1. Descargar el script oficial del repositorio
curl -sO https://raw.githubusercontent.com/TAVARDT-Mexico/vibe-coding-security-scanner/main/audit_vibe_security.py

# 2. Ejecutar auditoria determinista
python audit_vibe_security.py https://su-aplicacion.com
```

***

## Modelos de Mitigación Defensiva en Producción

El repositorio proporciona configuraciones validadas para blindar la arquitectura de la aplicación:

### Configuración Vite (`vite.config.ts`)
Desactiva sourcemaps en producción y normaliza la salida de compilación:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  build: {
    sourcemap: false,
    chunkSizeWarningLimit: 1000,
    rollupOptions: {
      output: {
        manualChunks: undefined
      }
    }
  }
});
```

### Configuración Next.js (`next.config.js`)
Bloquea la generación de mapas de código en el cliente e inyecta cabeceras defensivas:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  productionBrowserSourceMaps: false,
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
          { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' }
        ]
      }
    ];
  }
};

module.exports = nextConfig;
```

### Endurecimiento de Row Level Security en Supabase (`supabase-rls.sql`)
Asegura el aislamiento estricto de tablas y revoca permisos anónimos:

```sql
/* 1. Activacion obligatoria de Row Level Security en esquemas publicos */
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.organizations ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.transactions ENABLE ROW LEVEL SECURITY;

/* 2. Revocacion de privilegios de ejecucion publica para el rol anon */
REVOKE ALL ON ALL TABLES IN SCHEMA public FROM anon;

/* 3. Creacion de politica estricta de lectura asociada a la identidad del usuario */
CREATE POLICY "Los usuarios solo consultan su propio perfil" 
ON public.profiles FOR SELECT 
TO authenticated 
USING (auth.uid() = id);
```

### Regla de Protección Perimetral en Cloudflare WAF
Regla perimetral para descarte inmediato de sondeos maliciosos:

```text
(http.request.uri.path contains ".js.map") or 
(http.request.uri.path contains ".env") or 
(http.request.uri.path contains ".git")
Accion: Block (Descartar conexion inmediatamente)
```

***

## Preguntas Frecuentes sobre Arquitectura (FAQ Técnico)

### ¿Cuáles son las causas principales de vulnerabilidades en el Vibe Coding?
Los modelos de IA generan código mediante coincidencia estadística y no a través de modelado formal de amenazas. Por ello, con frecuencia colocan credenciales secretas en componentes del cliente, omiten el uso de Row Level Security y dejan opciones de depuración activas en compilaciones finales.

### ¿Por qué resulta riesgoso desplegar sourcemaps (.js.map) en entornos productivos?
Los archivos sourcemap proporcionan la correspondencia inversa exacta desde los archivos minificados hacia el código fuente original. Publicarlos en producción permite a atacantes comprender la lógica empresarial, descubrir endpoints administrativos y localizar debilidades con mínimo esfuerzo.

### ¿Cómo deben consumir APIs de inteligencia artificial las aplicaciones frontend?
Las aplicaciones cliente nunca deben comunicarse directamente con proveedores de IA utilizando credenciales maestras. Todas las solicitudes deben intermediarse a través de endpoints de backend seguros o funciones serverless en el borde que autentiquen al usuario, apliquen límites de tasa y conserven las claves de forma segura en el servidor.

### ¿La herramienta determinista en Python ocasiona interrupciones en el servicio auditado?
No. La utilidad opera estrictamente como un diagnóstico pasivo de solo lectura. Emite solicitudes HTTP convencionales tipo GET y HEAD para revisar cabeceras y verificar la existencia de archivos públicos, sin modificar datos ni saturar la infraestructura auditada.

***

## Marcos de Cumplimiento y Estándares Empresariales

Arquitectura defensiva y controles de seguridad alineados con la Ley Federal de Proteccion de Datos Personales en Posesion de los Particulares (LFPDPPP) y las guias de ciberseguridad del INAI.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnósticos:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

© 2026 TAVARDT. Todos los derechos reservados.
