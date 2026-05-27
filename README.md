# 🍔 UAM-Snacks — Sistema de Gestión para la Cafetería Estudiantil

¡Bienvenido al **Proyecto Final Integrador: UAM-Snacks**! 🎓✨  
Este sistema web fue diseñado y desarrollado como un proyecto educativo colaborativo, dividido en **4 submódulos** independientes pero totalmente integrados. El objetivo principal es aplicar conceptos fundamentales de **Estructuras de Datos y Algoritmos** utilizando una arquitectura web compacta, moderna y responsiva basada en **HTML, CSS, JavaScript** y **Bootstrap 5**.

El sistema utiliza la identidad visual oficial de la **Universidad Autónoma de Manizales (UAM)**, destacando sus colores institucionales:
*   **Azul UAM:** `#0069A3` (utilizado en cabeceras, botones principales y fondos de tarjetas).
*   **Amarillo UAM:** `#F4D73B` (utilizado para resaltar estados activos, alertas y botones secundarios).

---

## 📂 Estructura de Archivos del Proyecto

El proyecto está compuesto por **5 archivos autónomos** localizados en la raíz del directorio, los cuales se comunican entre sí sin necesidad de un servidor backend, facilitando su despliegue y aprendizaje:

```text
Proyecto final UAM A2026/
│
├── index.html          # Menú Principal y portal de acceso unificado.
├── modulo1.html        # Gestión de Menú y Precios (Grupo 1).
├── modulo2.html        # Control y Gestión de Mesas (Grupo 2).
├── modulo3.html        # Buscador Inteligente de Productos (Grupo 3).
└── modulo4.html        # Cierre de Caja y Generación de Reportes (Grupo 4).
```

---

## 🔄 Arquitectura de Datos e Integración (`localStorage`)

Para compartir información en tiempo real entre páginas independientes sin un servidor web, el proyecto implementa **`localStorage`** del navegador. De esta manera, el estado persiste aunque se refresque la pestaña o se navegue entre diferentes módulos.

A continuación se detalla el esquema de almacenamiento compartido:

| Clave en LocalStorage | Tipo de Dato | Módulo Emisor (Escribe) | Módulo Receptor (Lee) | Descripción del Contenido |
| :--- | :--- | :--- | :--- | :--- |
| `uam_menu` | `Array <Object>` | **Módulo 1** | **Módulos 2, 3, 4** | Lista de snacks con estructura `[{nombre: "Empanada", precio: 2500}]`. |
| `uam_mesas` | `Matrix (3x3)` | **Módulo 2** | **Módulo 4** | Representación 2D del estado físico de las mesas (`"Libre"` o `"Mesa X - [Pedido]"`). |
| `uam_ventas` | `Array <Object>` | **Módulo 2** | **Módulo 4** | Historial de pedidos completados en el día: `[{pedido: "Jugo", mesa: "2", hora: "14:35"}]`. |

---

## 🛠️ Detalle de los Módulos y Conceptos de Ciencias de la Computación

### 1. Menú Principal (`index.html`)
Es la puerta de entrada al sistema. Presenta un diseño moderno en cuadrícula responsiva (Grid de Bootstrap) con tarjetas interactivas que enlazan a cada módulo. Incluye animaciones suaves de desplazamiento (`hover effects`) y un fondo degradado premium.

*   **Característica Especial Integrada (Copia de Seguridad):** Contiene un cargador de archivos que utiliza la API de JavaScript **`FileReader`**. Los usuarios pueden subir cualquier reporte `.txt` descargado desde el Módulo 4 y el sistema extraerá automáticamente el bloque de estado serializado en JSON que se encuentra al final del archivo, restaurando de inmediato los productos del menú, el estado físico de las mesas y todas las ventas en el `localStorage`. Esto permite a los estudiantes comprender cómo funciona el análisis de archivos de texto y la deserialización de datos sin añadir complejidad a sus respectivos submódulos individuales.

---

### Módulo 1: Gestión de Menú y Precios (`modulo1.html`)
*   **Responsable:** Grupo 1
*   **Concepto de Computación Clave:** **Arreglos Unidimensionales (Arrays) & Algoritmo de Ordenamiento Burbuja (Bubble Sort)**
*   **Descripción:** Permite al administrador de la cafetería crear la lista de snacks y sus precios. 
*   **Algoritmo Destacado:** Implementa el método clásico de la Burbuja para ordenar de menor a mayor precio:
    ```javascript
    function ordenarBurbuja() {
        let n = menu.length;
        for (let i = 0; i < n - 1; i++) {
            for (let j = 0; j < n - i - 1; j++) {
                if (menu[j].precio > menu[j + 1].precio) {
                    // Intercambio de posiciones (Swap)
                    let aux = menu[j];
                    menu[j] = menu[j + 1];
                    menu[j + 1] = aux;
                }
            }
        }
        guardarMenu();
        renderizarLista();
    }
    ```

---

### Módulo 2: Gestión de Mesas (`modulo2.html`)
*   **Responsable:** Grupo 2
*   **Concepto de Computación Clave:** **Matrices (Arreglos Bidimensionales 3x3)**
*   **Descripción:** Modela físicamente las mesas de la cafetería en una cuadrícula interactiva. El usuario puede hacer clic en cualquier mesa para abrir un modal dinámico de Bootstrap, tomar una orden (cargando los productos activos de `uam_menu`) y registrar la venta.
*   **Lógica de Negocio:**
    *   Una mesa libre se visualiza en color **Azul UAM**.
    *   Una mesa ocupada se resalta en **Amarillo UAM**, mostrando el pedido asignado.
    *   Al liberar la mesa, el pedido se consolida automáticamente en el historial de ventas (`uam_ventas`).

---

### Módulo 3: Buscador Inteligente (`modulo3.html`)
*   **Responsable:** Grupo 3
*   **Concepto de Computación Clave:** **Recursividad (Llamadas a sí misma con condiciones de escape)**
*   **Descripción:** Permite buscar si un snack específico se encuentra dentro del inventario de forma recursiva. 
*   **Traza Visual:** Lo más innovador de este módulo es la **Traza de la Recursión**, la cual renderiza de forma visual y paso a paso el Call Stack (pila de llamadas), mostrando cuándo se avanza al siguiente índice y cómo se activan las condiciones de escape.
*   **Función Recursiva:**
    ```javascript
    function buscarRecursivo(arreglo, objetivo, indice) {
        // CONDICIÓN DE ESCAPE 1: Fin del arreglo (No encontrado)
        if (indice === arreglo.length) {
            registrarPaso(indice, "Fin del arreglo. ¡No encontrado!", "escape");
            return false;
        }
        
        // CONDICIÓN DE ESCAPE 2: Coincidencia exacta (Encontrado)
        if (arreglo[indice].toLowerCase() === objetivo.toLowerCase()) {
            registrarPaso(indice, `¡Encontrado! "${arreglo[indice]}" coincide con "${objetivo}"`, "exito");
            return true;
        }
        
        // PASO RECURSIVO: Llamar con el siguiente índice
        registrarPaso(indice, `Revisando "${arreglo[indice]}", no coincide. Pasando a posición ${indice + 1}`, "recursivo");
        return buscarRecursivo(arreglo, objetivo, indice + 1);
    }
    ```

---

### Módulo 4: Cierre de Caja (`modulo4.html`)
*   **Responsable:** Grupo 4
*   **Concepto de Computación Clave:** **Manejo de Archivos, Creación de Flujos de Datos Dinámicos (Blob) y Persistencia**
*   **Descripción:** Consolida la información recolectada por todos los módulos. Muestra estadísticas dinámicas (pedidos cobrados, mesas activas y número de ítems en el menú), renderiza una bitácora de pedidos y genera un reporte formateado en un área de texto de ancho fijo.
*   **Descarga Local:** Genera dinámicamente un archivo de texto (`.txt`) descargable al instante mediante la API del navegador:
    ```javascript
    function descargarArchivo() {
        let texto = document.getElementById('textoReporte').value;
        let blob = new Blob([texto], { type: "text/plain;charset=utf-8" });
        let url = URL.createObjectURL(blob);
        
        let enlace = document.createElement("a");
        enlace.href = url;
        enlace.download = "cierre_caja_uam_snacks.txt";
        enlace.click();
        
        URL.revokeObjectURL(url); // Liberar recursos del navegador
    }
    ```
*   **Acción Global:** Incluye un botón para **Limpiar todos los datos** que reinicia el `localStorage` mediante `localStorage.clear()` para simular el inicio de una nueva jornada diaria.

---

## 🚀 Flujo de Uso Recomendado (Paso a Paso)

Para experimentar la integración completa del sistema de forma coherente, te sugerimos seguir este camino lógico:

1.  **Paso 1: Configurar el Menú (Módulo 1)**
    *   Abre `modulo1.html`.
    *   Ingresa snacks personalizados (Ej. *Empanada, Jugo de Mora, Café, Pastel*) con sus respectivos precios y presiona **➕ Añadir**.
    *   Haz clic en **Ordenar (Burbuja)** para ver cómo los arreglos se reordenan visualmente según el precio.
2.  **Paso 2: Registrar Órdenes en las Mesas (Módulo 2)**
    *   Entra a `modulo2.html`.
    *   Verás una cuadrícula de 9 mesas en azul. Haz clic en cualquiera.
    *   Selecciona un snack del menú desplegable (que se cargó del paso anterior) y pulsa **Abrir Mesa**.
    *   Cuando el cliente termine, haz clic de nuevo en la mesa y presiona **Cobrar y Liberar Mesa** para registrar la venta en la base de datos temporal.
3.  **Paso 3: Realizar Búsquedas Rápidas (Módulo 3)**
    *   Ve a `modulo3.html`.
    *   Ingresa el nombre del snack que quieres buscar y haz clic en **🔎 Buscar**.
    *   Observa abajo en la traza paso a paso cómo funciona el algoritmo recursivo hasta llegar a la respuesta.
4.  **Paso 4: Realizar el Cierre de Caja (Módulo 4)**
    *   Abre `modulo4.html`.
    *   Visualiza los indicadores consolidados y el historial completo de pedidos cobrados.
    *   Haz clic en **Descargar Reporte (.txt)** para guardar en tu computadora el informe diario oficial de ventas.

---

## 🎨 Aspectos Destacados de Diseño e Interfaz

*   **Sin dependencias pesadas:** Todo se renderiza de forma óptima a través de enlaces CDN públicos de **Bootstrap 5.3.3** y fuentes estéticas del navegador.
*   **Responsividad Premium:** Adaptable a dispositivos móviles, tablets y computadoras de escritorio.
*   **Micro-animaciones:** Transiciones sutiles tipo `.card-modulo:hover { transform: translateY(-4px); }` que le dan vitalidad a la navegación del sistema.
*   **Aviso Didáctico:** Cada módulo contiene una sección final con fondo amigable que explica detalladamente a los estudiantes el concepto de código aplicado en esa sección.

---

## 👨‍💻 Créditos del Curso
*   **Curso:** Proyecto Final Integrador UAM 2026
*   **Institución:** Universidad Autónoma de Manizales (UAM)
*   **Tecnologías Utilizadas:** HTML5, CSS3, JavaScript (ES6+), Bootstrap 5.3.
