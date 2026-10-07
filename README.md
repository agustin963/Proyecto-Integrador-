# Proyecto-Integrador-


---

## 1. Parte Inicial del Proyecto

* **Nombre del Proyecto:** DeducIA Edge *(Nombre provisional)*
* **Tipo de Producto:** Progressive Web App (PWA) con IA embebida en el cliente (Edge AI).
* **Propuesta de Valor:** Auditoría y validación instantánea de deducibilidad fiscal en el dispositivo móvil sin envío de datos a servidores externos (Arquitectura *Zero-Data Leak*).

---

## 2. Alcance del Proyecto

### Dentro del Alcance (In Scope)

* **Captura y OCR Local:** Procesamiento de imágenes (tickets y facturas PDF/XML/impresos) mediante modelos ligeros de visión computacional optimizados para WebAssembly / WebGPU directamente en el navegador.
* **Motor de Reglas Fiscales:** Evaluación de requisitos de deducibilidad conforme a la Ley del Impuesto sobre la Renta (LISR) y Ley del Impuesto al Valor Agregado (LIVA) vigentes en México.
* **Semáforo de Deducibilidad:** Clasificación visual inmediata (Verde = Deducible, Amarillo = Requiere revisión/Forma de pago, Rojo = No deducible).
* **Almacenamiento Local Seguro:** Guardado de comprobantes y metadatos localmente mediante IndexedDB / OPFS.
* **Soporte Offline (PWA):** Instalación desde el navegador y funcionamiento 100% sin conexión a internet.
* **Exportación de Reportes:** Generación de resúmenes precalculados en PDF/CSV para envío directo a despachos contables.

### Fuera del Alcance (Out of Scope)

* Timbrado o emisión de Comprobantes Fiscales Digitales por Internet (CFDI).
* Conexión directa mediante WS/API del SAT para descarga masiva en la versión inicial.
* Sincronización en la nube o almacenamiento centralizado de datos de facturación.

---

## 3. Muestra o Sustento (Fundamento Normativo y Tecnológico)

### Sustento Normativo (SAT México)

* **LISR Art. 27:** Requisitos de las deducciones autorizadas (pagos mayores a $2,000 MXN mediante medios electrónicos, relación estricta con la actividad, datos del emisor/receptor).
* **LISR Art. 113-E a 113-J (RESICO):** Reglas específicas de deducciones para el Régimen Simplificado de Confianza y cálculo de IVA acreditable.
* **LIVA Art. 5:** Requisitos para el acreditamiento del IVA en comprobantes fiscales.

### Sustento Tecnológico

* **WebAssembly (Wasm) + WebGPU:** Ejecución de modelos de visión e inferencia a nivel de cliente para lograr respuestas en milisegundos sin latencia de red.
* **Edge AI / Small Language Models (SLMs):** Cuantización de modelos para procesamiento eficiente en memoria RAM de dispositivos móviles gama media/alta.

---

## 4. Objetivos

### Objetivo General

Desarrollar e implementar una PWA con IA embebida en el cliente para validar de forma offline, inmediata (en milisegundos) y privada la deducibilidad fiscal de facturas y tickets bajo la normativa vigente del SAT en México.

### Objetivos Específicos

1. **Desarrollar el motor OCR e inferencia local:** Lograr la extracción de datos clave (RFC, fecha, monto, IVA, método de pago, concepto) con un tiempo de procesamiento inferior a 1,500 ms por ticket/factura.
2. **Implementar el motor de reglas fiscales:** Parametrizar las reglas de LISR/LIVA para clasificar correctamente el nivel de deducibilidad en los regímenes de RESICO y Persona Física con Actividad Empresarial/Profesional.
3. **Garantizar la privacidad *Zero-Data Leak*:** Asegurar que el 100% del procesamiento y almacenamiento de datos sensibles (RFCs, montos, conceptos) permanezca exclusivamente en el almacenamiento local del navegador del usuario.
4. **Construir la experiencia PWA offline:** Habilitar capacidades de Service Workers y caching para garantizar la usabilidad completa de la aplicación sin conectividad a internet.

---

## 5. Justificación

* **Privacidad y Seguridad (Zero-Data Leak):** Los contribuyentes y despachos enfrentan desconfianza al subir información financiera y RFCs a servidores de terceros. Procesar todo en el cliente elimina vectores de ataque en la nube y riesgos de filtración.
* **Reducción de Costos Operativos:** Al descentralizar el cómputo al dispositivo del usuario (Edge AI), el costo de infraestructura backend se reduce prácticamente a cero, permitiendo escalabilidad masiva sin gastos crecientes de servidor.
* **Prevención de Pérdida Monetaria:** Muchos contribuyentes pierden saldo a favor o IVA acreditable por no identificar a tiempo errores comunes (ej. pagar más de $2,000 en efectivo o erogaciones no deducibles para su régimen). El semáforo visual resuelve esto al momento de la compra.
* **Fricción Cero de Adopción:** La arquitectura PWA evita las comisiones, restricciones y tiempos de espera de aprobación de App Store y Google Play Store, facilitando acceso instantáneo desde una URL.

---

## 6. Partes Interesadas (Stakeholders)

| Parte Interesada | Rol / Interés | Expectativa Principal |
| --- | --- | --- |
| **Freelancers y Contribuyentes PFF** | Usuarios Finales (RESICO / Sueldos / Serv. Profesionales) | Detección rápida de deducciones, usabilidad sencilla e higiene fiscal. |
| **Despachos y Contadores** | Usuarios Aliados / Canal de Distribución | Recepción de comprobantes precalculados, organizados y sin errores de origen. |
| **Equipo de Desarrollo (FrontEnd & AI)** | Ejecutores Técnicos | Optimización de modelos Wasm, experiencia de usuario fluida y rendimiento de PWA. |
| **SAT (Entidad Reguladora)** | Marco de Referencia Externo | Cumplimiento estricto de la normativa fiscal (LISR/LIVA). |

---

## 7. Matriz de Riesgos

| Riesgo | Impacto | Probabilidad | Estrategia de Mitigación |
| --- | --- | --- | --- |
| **R1. Alto consumo de recursos en móviles de gama baja** | Alto | Media | Aplicar cuantización agresiva a los modelos de visión/OCR y establecer *fallbacks* a motores OCR livianos basados en Wasm. |
| **R2. Cambios en la legislación fiscal del SAT** | Alto | Media | Diseñar el motor de reglas de forma modular (JSON de políticas) para actualizar las reglas sin reescribir la arquitectura de IA. |
| **R3. Errores de lectura en comprobantes dañados o borrosos** | Medio | Alta | Implementar validaciones cruzadas (comprobar subtotal + IVA = total) y permitir edición manual rápida en el semáforo visual. |
| **R4. Borrado accidental de datos por limpieza del navegador** | Alto | Baja | Implementar solicitud de almacenamiento persistente (`navigator.storage.persist()`) y alertas de respaldo/exportación local. |

---

## 8. Cronograma / Desglose de Trabajo (WBS)

```
DeducIA Edge
├── 1. Investigación y Arquitectura
│   ├── 1.1 Definición del esquema JSON de reglas LISR/LIVA
│   └── 1.2 Selección y pruebas del modelo OCR/Inferencia embebido (Wasm/WebGPU)
├── 2. Desarrollo del Core FrontEnd & AI Local
│   ├── 2.1 Implementación del Pipeline de captura y preprocesamiento de imagen
│   ├── 2.2 Integración del motor de inferencia en el navegador
│   └── 2.3 Desarrollo del motor de validación de reglas de deducibilidad
├── 3. Capa de Datos y PWA
│   ├── 3.1 Configuración de IndexedDB / OPFS para guardado local
│   └── 3.2 Service Workers, Manifest y soporte offline completo
├── 4. Interfaz de Usuario (UI/UX)
│   ├── 4.1 Pantalla de escaneo y semáforo visual de resultados
│   └── 4.2 Módulo de exportación de reportes (PDF/CSV)
└── 5. Pruebas y Optimización
    ├── 5.1 Pruebas de velocidad de inferencia (< 1,500 ms) en diversos dispositivos
    └── 5.2 Validaciones de casos borde de normativa SAT

```

### Fases del Cronograma (Estimación general)

```
[Semana 1 - 2]  Fase 1: Arquitectura y pruebas del motor AI embebido
[Semana 3 - 5]  Fase 2: Desarrollo Core (OCR local + Motor de reglas SAT)
[Semana 6 - 7]  Fase 3: Implementación PWA, IndexedDB y UI/UX
[Semana 8]      Fase 4: Pruebas de rendimiento, optimización y entrega

```
