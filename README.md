# DeducIA Edge Pro 🚀

**DeducIA Edge Pro** es una Progressive Web App (PWA) de auditoría y validación instantánea de deducibilidad fiscal bajo la normativa del SAT (México). Mediante una arquitectura **Edge AI (Zero-Data Leak)**, el sistema extrae datos de tickets y procesa reglas fiscales complejas directamente en el dispositivo del usuario, garantizando privacidad absoluta, funcionamiento 100% offline y costos de infraestructura de servidor cercanos a cero.

---

## 📌 1. La Problemática

El entorno fiscal en México para los contribuyentes físicos (RESICO, Actividad Empresarial) y los despachos contables presenta tres ineficiencias críticas:

1. **Falta de Validación en el Punto de Venta:** Los contribuyentes realizan gastos comerciales cotidianos (combustibles, viáticos, insumos) sin saber si cumplen los requisitos del SAT en el momento de la compra. Errores comunes (como pagar gasolina en efectivo o facturar con un Uso de CFDI incorrecto) provocan la pérdida del derecho a deducir el ISR y acreditar el IVA.
2. **Desconfianza y Riesgos de Privacidad (Data Leak):** Las soluciones contables tradicionales en la nube exigen que el usuario suba información financiera altamente sensible (RFCs, montos, conceptos de compra) a servidores de terceros, abriendo vectores de ataque y fugas de datos.
3. **Saturación Operativa en los Despachos:** Los contadores pierden hasta el 40% de su tiempo mensual revisando, clasificando y corrigiendo facturas mal emitidas o rechazadas que sus clientes les envían al cierre de mes, limitando la escalabilidad del despacho.

---

## 🎯 2. Impacto Comercial y Operativo

El proyecto adopta un modelo **B2B2C** diseñado para mitigar las problemáticas anteriores de forma escalable:

* **Para el Despacho Contable (Cliente B2B):** Funciona como un canal de distribución y control de higiene fiscal. El despacho adquiere el software y proporciona acceso a sus clientes. Al automatizar la validación en origen, el contador elimina el trabajo manual de corrección y puede absorber el doble de clientes sin aumentar su personal auxiliar.
* **Para el Contribuyente Final (Usuario C):** Recibe una aplicación gratuita (patrocinada por su contador) que actúa como un asistente en su bolsillo. El "Semáforo Fiscal" le advierte en milisegundos si un gasto es válido o cómo corregirlo antes de abandonar el establecimiento.
* **Para el Negocio/Desarrollador:** Al delegar el 100% del procesamiento (OCR y reglas) al hardware del cliente, los costos operativos de cómputo en la nube se reducen a **\$0.00 pesos**. Esto permite ofrecer tarifas fijas altamente competitivas y retener un margen de ganancia neto cercano al 95%.

---

## 💡 3. Innovación Tecnológica

La ventaja competitiva de **DeducIA Edge Pro** radica en su diseño técnico:

* **Arquitectura Zero-Data Leak:** Ninguna factura, imagen, RFC o metadato financiero sale del teléfono del usuario hacia servidores externos. El procesamiento es estrictamente local.
* **Pipeline Híbrido de Inferencia:** 
  * *Para imágenes:* Ejecuta modelos de visión computacional y OCR ultraligeros en el cliente mediante WebAssembly (Wasm).
  * *Para archivos XML:* Realiza un parseo sintáctico inmediato de texto plano en JavaScript nativo (0 ms de latencia).
* **Desacoplamiento de Lógica Fiscal:** El motor de Inteligencia Artificial solo se encarga de la extracción de texto. Las leyes fiscales se evalúan mediante un árbol de decisiones estructurado en un JSON modular. Esto permite actualizar la aplicación ante reformas del SAT en minutos, sin necesidad de reentrenar o modificar los modelos de IA.

---

## 🛠️ 4. Stack Tecnológico Recomendado y Justificación

Para garantizar la estabilidad en producción y cumplir la promesa de "fricción cero", se recomienda el siguiente stack:

| Componente | Tecnología | Justificación |
| :--- | :--- | :--- |
| **Frontend Framework** | **Next.js (App Router) / React** o **Vite + React** | Permite construir una interfaz SPA (Single Page Application) altamente reactiva, idónea para compilar estructuras PWA y gestionar ciclos de vida optimizados. |
| **PWA & Offline** | **Workbox (Service Workers)** | Proporciona control total sobre las estrategias de almacenamiento en caché de los activos y scripts de IA, garantizando un funcionamiento 100% independiente de internet. |
| **Motor OCR Local** | **PaddleOCR (v4 Slim) Wasm** o **ONNX Runtime Web** | Modelos cuantizados a 4-bits que ocupan entre 15MB y 20MB de almacenamiento. Ideales para descargarse una sola vez en la PWA y procesar imágenes localmente sin consumir memoria excesiva. |
| **Persistencia Local** | **IndexedDB (vía Dexie.js) + OPFS** | Sistema de base de datos relacional nativo del navegador. Al usar `navigator.storage.persist()`, se previene que el sistema operativo móvil borre los comprobantes por falta de espacio. |
| **Seguridad de Datos** | **Web Crypto API** | Cifrado asimétrico nativo del navegador. Se utiliza para compilar y empaquetar los respaldos locales (`.deducia`) con una contraseña del usuario, protegiendo los datos exportados a la nube personal. |
| **Hosting e Infraestructura** | **Vercel / Netlify / AWS S3** | Al ser una aplicación estática basada en computación del lado del cliente, el hosting es sumamente económico y tiene soporte nativo para redes de distribución de contenido (CDN). |

---

## 🗂️ 5. Estructura de Módulos y Submódulos (Fase MVP)

Para iniciar el desarrollo ordenado del Producto Mínimo Viable (MVP), se plantea la siguiente arquitectura modular:

### Modulo 1: Core de Captura e Inferencia Local (AI Pipeline)
* **Submódulo 1.1: Captura y Preprocesamiento de Imagen:** Control de cámara nativa mediante HTML5, escalado, conversión a escala de grises y ajuste de contraste local en Canvas para mejorar la legibilidad del ticket físico.
* **Submódulo 1.2: Motor OCR (Wasm Worker):** Inicialización del modelo cuantizado dentro de un *Web Worker* independiente para no congelar la interfaz de usuario durante la extracción del texto (RFC, montos, conceptos).
* **Submódulo 1.3: Parser Nativo XML/PDF:** Lector estructurado de archivos digitales `.xml` para extraer etiquetas clave del estándar CFDI (`FormaPago`, `RegimenFiscalReceptor`, etc.) de forma instantánea.

### Módulo 2: Motor Normativo y Árbol de Reglas (Fiscal Engine)
* **Submódulo 2.1: Intérprete JSON de Reglas:** Lógica en TypeScript que recibe las variables extraídas del Módulo 1 y las evalúa contra el esquema de políticas fiscales vigentes.
* **Submódulo 2.2: Clasificador de Regímenes:** Filtro condicional que parametriza las diferencias de acreditación entre RESICO y Actividad Empresarial (ej. manejo de deducciones personales).

### Módulo 3: Interfaz de Usuario y Experiencia del Semáforo (UI/UX)
* **Submódulo 3.1: Dashboard de Perfil Fiscal:** Configuración inicial del usuario (RFC, Régimen Fiscal, Actividad Económica predominante).
* **Submódulo 3.2: Vista del Semáforo Dinámico:** Pantalla de resultados de escaneo con colores reactivos (Verde / Amarillo / Rojo) acompañados de descripciones claras en lenguaje ciudadano y acciones de corrección sugeridas.

### Módulo 4: Capa de Datos, Persistencia y Resiliencia (Storage & Security)
* **Submódulo 4.1: Gestor IndexedDB:** Repositorio local de comprobantes escaneados, metadatos y estados de deducibilidad.
* **Submódulo 4.2: Exportador de Backups Cifrados:** Utilidad para empaquetar la base de datos local en un archivo legible cifrado (`.deducia`) y módulo generador de reportes mensuales en formatos CSV y PDF limpios para el contador.
