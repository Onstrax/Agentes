---
title: 'Resumen Completo: Arquitectura y Optimización Móvil'
drive_id: 1pcgwTR-Zv2v_fYOZ202E5ho2eM0spRdib6wJrybiCK8
drive_url: https://docs.google.com/document/d/1pcgwTR-Zv2v_fYOZ202E5ho2eM0spRdib6wJrybiCK8
modified_time: '2026-05-07T17:28:45.160Z'
source: google-drive
---

## titulo: "Arquitectura, Diseño y Optimización de Software Móvil" materia: "Desarrollo de Aplicaciones Móviles / Arquitectura de Software" tipo: resumen_maestro fecha: "2026-05-07" tags: \[arquitectura, performance, design-thinking, android, ios, flutter\]

# 🏗️ Guía Maestra: Del Diseño a la Optimización de Sistemas Móviles

## 🎨 1. Diseño y Documentación (Design Thinking)

### El Proceso de Ideación

El diseño no es solo estética, es resolver problemas. Se utilizan modelos ligeros (**Agile Modeling**) para definir qué hace el sistema.

- **Context Canvas:** Define personas y características iniciales.

- **VD-Map (Value-Data Map):** Conecta las fuentes de datos con las preguntas de negocio.

  - **Tipos de preguntas:**

    1.  **Internas:** Eficiencia operativa.

    2.  **UX:** ¿Cómo interactúa el usuario?

    3.  **Externas:** Datos de terceros.

    4.  **Ganancia (Profit):** Cómo monetizar.

- **Escenarios:**

  - **Funcionales:** Descripciones tipo "User Story" de lo que el usuario hace.

  - **Atributos de Calidad:** ¿Qué pasa si se agota la memoria? ¿Si la conexión es lenta?

### Metáforas de Diseño

- **Material Design (Android):** Basado en el mundo físico (papel y tinta). Concepto clave: **Elevación** (eje Z) mediante sombras.

- **Human Interface Guidelines (iOS):** Foco en claridad, deferencia y profundidad. Diseño "Intencional".

## 🏛️ 2. Arquitectura de Software y Estilos

### El "Mojo" del Arquitecto

La arquitectura es el diseño de las estructuras y la **justificación (rationale)** detrás de ellas.

- **Estilos Arquitectónicos:**

  1.  **Data-flow (Tuberías y Filtros):** Ejemplo: Comandos de consola UNIX (ls \| grep).

  2.  **Call-return:** El cliente invoca al proveedor (Cliente-Servidor, SOA, REST).

  3.  **Event-based:** Comunicación implícita a través de un bus de eventos (Publish-Subscribe).

  4.  **Repository-based:** Datos centralizados (Bases de datos SQL/NoSQL).

### Tácticas Arquitectónicas

Son recetas para lograr atributos de calidad específicos.

- **Escalabilidad:**

  - *Vertical (Captain America):* Más recursos a una sola máquina.

  - *Horizontal (Minions):* Más máquinas pequeñas trabajando juntas.

### Patrones MV\*

- **MVC (iOS):** El Controlador media estrictamente entre Vista y Modelo (Passive View).

- **MVVM (Android Jetpack):** El ViewModel usa **LiveData** para que la Vista "observe" cambios sin tener una referencia directa.

## 🐧 3. El Corazón del Sistema: Kernels y Stacks

### Comparativa de Kernels

- **Monolítico (Linux/Android):** Todo corre en el espacio de kernel. Es rápido pero un error en un driver puede tumbar el sistema.

- **Microkernel (Mach/QNX):** Solo lo esencial (IPC, memoria virtual) está en el kernel; lo demás son "servidores" en espacio de usuario. Más robusto y extensible.

- **Híbrido (XNU - iOS/Darwin):** Combina la velocidad del monolítico con la modularidad del microkernel.

### Stack de Android

1.  **Kernel Linux:** Gestión de drivers y energía (Wakelocks).

2.  **HAL (Hardware Abstraction Layer):** Interfaz para fabricantes de hardware.

3.  **Runtime (ART/Dalvik):** Ejecución de archivos .dex. **ART** introdujo compilación **AOT** (Ahead-of-Time).

4.  **Native Libraries:** C/C++ (OpenGL, WebKit).

5.  **Java Framework:** APIs de Android (Activity Manager, View System).

## ⚡ 4. Performance y "Bugs Exterminator"

### El Juego del Hilo Único (Single Thread Game)

Las apps móviles ejecutan la UI en un solo hilo.

- **Problemas:** Si el hilo se bloquea \> 5 seg, aparece el **ANR** (Application Not Responding). Si la tarea dura \> 16ms, hay **GUI Lag** (choppiness).

- **Solución:** Multi-threading.

  - **iOS:** Grand Central Dispatch (GCD) con colas de prioridad (UserInteractive, Background, etc.).

  - **Android:** Coroutines (Kotlin), AsyncTask (deprecado), Services.

### Gestión de Memoria (ARC vs. GC)

- **Garbage Collector (Android):** El sistema reclama memoria automáticamente. Cuidado con objetos creados dentro de bucles.

- **Automatic Reference Counting (iOS):** Cuenta cuántas referencias fuertes tiene un objeto.

  - **Strong Reference Cycle:** Dos objetos se apuntan mutuamente y nunca se liberan.

  - **Solución:** Usar referencias weak (débiles).

### Micro-optimizaciones

- Usar SparseArray en Android en vez de HashMap para evitar el **Autoboxing** (convertir int a Integer gasta 12 bytes extra).

- Evitar el **Overdrawing:** No pintar fondos que no se ven (simplificar jerarquías de vistas).

## 🌐 5. Conectividad Eventual y Datos

### Estrategias de Caché

- **Permanente:** Se descarga todo al inicio (Ej: Juegos).

- **Temporal:** Expira por tiempo (Ej: Noticias).

- **Sin Caché:** Siempre requiere red.

### Antipatrones de Conectividad

1.  **Blocked Application:** La app se congela al buscar red.

2.  **Stuck Progress:** El spinner de carga gira infinitamente porque falló el callback de error.

3.  **Lost Content:** El usuario escribe un mensaje, se va el internet y el texto desaparece (no se encoló).

### Local Storage

- **Relacional (SQLite):** Datos estructurados, integridad ACID.

- **NoSQL (Key-Value, Document):** Alta escalabilidad (Ej: Firebase, Redis).

## 📈 6. Resumen de Métricas de Memoria (Richard Euler)

Para medir el impacto real de tu app, no uses el tamaño total, usa:

- **PSS (Proportional Set Size):** Es la medida más justa; cuenta la memoria privada de tu app más la parte proporcional de las librerías compartidas.

- **APK Size:** Un APK pequeño suele implicar menos uso de RAM al cargar clases y recursos.

> \[!TIP\]
>
> **Ejemplo de la vida real:**
>
> Imagina una app de mapas.

- **Táctica:** Caching de tiles locales para mejorar el performance.

- **Escenario de Calidad:** "Si el usuario entra a un túnel (pérdida de red), la app debe mostrar el mapa borroso con los datos en caché y no cerrar la sesión".

- **Optimización:** Usar una referencia Weak al Context de la Activity dentro del hilo de descarga para evitar un Memory Leak si el usuario rota la pantalla.
