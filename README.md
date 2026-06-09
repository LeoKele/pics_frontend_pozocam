# PozoCam — WebApp para capturar videos y coordenadas GPS.

Este repositorio contiene la aplicación móvil **PozoCam**, desarrollada como parte del Proyecto Integrador de Ciencias de Datos (PICS) en la Universidad Nacional de Luján.

PozoCam es una Progressive Web App (PWA) diseñada para ejecutarse en el dispositivo móvil a bordo del vehículo de inspección. Su función es capturar video de la calzada y telemetría GPS sincronizada, y enviarla al [backend](https://github.com/LeoKele/pics_arquitectura/tree/main) para su procesamiento por el modelo YOLO.

---

## Descripción General

El flujo de uso es simple: el operador abre la app en el celular del vehículo, verifica la señal GPS, inicia la grabación y conduce el recorrido. Al terminar, presiona detener y sube el recorrido directamente al servidor. La app se encarga del resto.

Funcionalidades principales:

- **Previsualización en vivo** de la cámara trasera del dispositivo como fondo de pantalla completa.
- **Grabación de video** en formato WebM (VP8) con bitrate y resolución configurables.
- **Captura de telemetría GPS** cada 500ms durante la grabación, sincronizada con el timestamp del video.
- **Subida multipart segmentada** (fragmentos de 5MB, compatible con S3/MinIO) con reporte de progreso en tiempo real.
- **Respaldo automático en IndexedDB** (via `localforage`) por si la app se cierra o el navegador se reinicia antes de subir.
- **Descarga local** del video (`.webm`) y los datos GPS (`.json`) como alternativa offline.
- **Diseño orientado a mobile**: layout adaptativo portrait/landscape, safe areas para notch/barra inferior, sin zoom ni selección de texto.

---

## Stack Tecnológico

| Tecnología | Uso |
|---|---|
| **Next.js 14 (App Router)** | Framework React con soporte PWA |
| **TypeScript** | Tipado estático |
| **Tailwind CSS** | Estilos utilitarios mobile-first |
| **MediaRecorder API** | Grabación de video desde la cámara del dispositivo |
| **Geolocation API** | Seguimiento GPS en tiempo real (`watchPosition`) |
| **localforage (IndexedDB)** | Persistencia offline del video y telemetría pendiente |
| **Font Awesome 6** | Iconografía |

---

## Estructura del Repositorio

```
front-pozocam-react/
├── app/
│   ├── layout.tsx          # Layout raíz 
│   ├── page.tsx            # Componente principal
│   └── globals.css         # Estilos globales
├── components/
│   ├── TopBar.tsx          # Barra superior
│   ├── HUD.tsx             # Indicadores sobreimpresos
│   ├── RecControls.tsx     # Panel de mandos 
│   ├── SettingsModal.tsx   # Modal de configuración 
│   └── UploadModal.tsx     # Modal de progreso de subida 
├── hooks/
│   ├── useCamera.ts        # Gestión del stream de cámara 
│   ├── useGPS.ts           # Seguimiento GPS 
│   └── useRecorder.ts      # Lógica de grabación
├── services/
│   └── uploader.ts         # Motor de subida S3 Multipart
└── types/
    └── index.ts            # Interfaces TypeScript
```

---

## Cómo levantar el proyecto

La aplicación cliente de la Pozocam se encuentra desplegada y accesible públicamente a través de Netlify en el siguiente enlace:

URL de Producción: https://pozocam.netlify.app/

El despliegue está configurado para conectarse de forma transparente con nuestra infraestructura en Google Cloud, resolviendo los problemas nativos de seguridad del navegador (bloqueos por Mixed Content al mezclar https:// con http://) mediante un sistema de proxy interno.


Para asegurar la comunicación entre el frontend seguro en Netlify y nuestro backend (FastAPI) y servidor de almacenamiento (MinIO), utilizamos el motor de redirecciones nativo de Netlify. `(_redirects)` en la carpeta /public.

---
