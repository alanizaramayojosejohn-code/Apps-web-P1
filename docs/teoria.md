# Práctica N.º 1 - INF 3911 "A" II/2026
## Parte Teórica (20%)

### 1. HTML y CSS

**Pregunta:** ¿Cuál es la importancia de utilizar etiquetas semánticas en HTML5 (como `<header>`, `<nav>`, `<section>`, `<article>`) en lugar de usar únicamente contenedores genéricos `<div>`, y cómo se relaciona esto con CSS y la accesibilidad (SEO)?

**Respuesta:**

Las etiquetas semánticas de HTML5 describen el **significado y propósito** del contenido que envuelven, mientras que un `<div>` no aporta ninguna información sobre su contenido: es un simple contenedor genérico sin valor semántico. Esta diferencia tiene tres implicaciones importantes:

1. **Legibilidad y mantenibilidad del código:** al usar `<header>`, `<nav>`, `<section>`, `<article>`, `<aside>` o `<footer>`, cualquier desarrollador puede entender la estructura de la página con solo mirar el marcado, sin depender de nombres de clases (`class="div-menu"`, `class="contenedor-3"`) que pueden ser ambiguos o inconsistentes.

2. **Accesibilidad:** los lectores de pantalla y otras tecnologías asistivas usan las etiquetas semánticas para construir un "árbol de accesibilidad" de la página. Por ejemplo, un lector de pantalla puede anunciar "navegación principal" al llegar a un `<nav>` o permitir saltar directamente al `<main>`, algo que no es posible si todo el documento está compuesto por `<div>` sin atributos ARIA adicionales. Esto cumple con pautas como las WCAG (Web Content Accessibility Guidelines).

3. **SEO (posicionamiento en buscadores):** los motores de búsqueda analizan la estructura semántica para entender qué parte de la página es el contenido principal (`<article>`, `<main>`), cuál es la navegación (`<nav>`) y cuál es información secundaria (`<aside>`, `<footer>`). Esto ayuda a indexar mejor el contenido relevante y puede mejorar el ranking en los resultados de búsqueda.

**Relación con CSS:** desde el punto de vista puramente visual, CSS puede estilizar un `<div>` exactamente igual que un `<header>` o una `<section>` (ambos son elementos de bloque por defecto). La diferencia no está en lo que CSS puede hacer, sino en que usar etiquetas semánticas permite escribir selectores CSS más claros y predecibles (por ejemplo, `header nav a` en lugar de `.div1 .div2 a`), reduciendo la necesidad de añadir clases extra solo para diferenciar contenedores. Es decir, HTML semántico aporta significado estructural, y CSS se encarga de la presentación visual sobre esa estructura ya significativa.

---

### 2. JavaScript Moderno (ES6+)

**Pregunta:** Explica qué son las promesas en JavaScript y cómo la sintaxis de `async/await` simplifica el consumo de APIs asíncronas en comparación con el uso directo de `.then()` y `.catch()`.

**Respuesta:**

Una **promesa** (`Promise`) es un objeto que representa el resultado eventual (exitoso o fallido) de una operación asíncrona, como una petición a una API, una lectura de archivo o un temporizador. Una promesa siempre se encuentra en uno de tres estados:

- **Pending (pendiente):** la operación aún no ha terminado.
- **Fulfilled (cumplida):** la operación terminó con éxito y la promesa tiene un valor resuelto.
- **Rejected (rechazada):** la operación falló y la promesa tiene un motivo de error.

Antes de las promesas, el código asíncrono se manejaba con *callbacks* anidados, lo que producía el conocido "callback hell". Las promesas solucionan esto permitiendo encadenar operaciones con `.then()` (para el caso de éxito) y `.catch()` (para el caso de error):

```js
fetch('https://api.ejemplo.com/usuarios')
  .then(response => response.json())
  .then(data => {
    console.log(data);
    return fetch(`https://api.ejemplo.com/usuarios/${data[0].id}/pedidos`);
  })
  .then(response => response.json())
  .then(pedidos => console.log(pedidos))
  .catch(error => console.error('Error:', error));
```

Aunque esto ya es más legible que los callbacks anidados, cuando hay varias operaciones asíncronas dependientes entre sí, el encadenamiento de `.then()` puede volverse difícil de leer y de depurar (sobre todo al mezclar lógica condicional o manejo de errores en distintos pasos).

**`async/await`** es "azúcar sintáctico" construido sobre las promesas: permite escribir código asíncrono con una estructura que *parece* síncrona, mejorando mucho la legibilidad. Una función marcada como `async` siempre devuelve una promesa, y dentro de ella se puede usar `await` para "pausar" la ejecución hasta que una promesa se resuelva, sin bloquear el hilo principal del navegador.

El mismo ejemplo anterior con `async/await`:

```js
async function obtenerPedidos() {
  try {
    const response = await fetch('https://api.ejemplo.com/usuarios');
    const data = await response.json();

    const responsePedidos = await fetch(`https://api.ejemplo.com/usuarios/${data[0].id}/pedidos`);
    const pedidos = await responsePedidos.json();

    console.log(pedidos);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

**Ventajas de `async/await` frente a `.then()/.catch()`:**

1. **Legibilidad:** el código se lee de arriba hacia abajo como si fuera síncrono, sin anidar funciones callback dentro de cada `.then()`.
2. **Manejo de errores centralizado:** un solo bloque `try/catch` puede capturar errores de múltiples operaciones asíncronas seguidas, en lugar de depender de un `.catch()` al final de la cadena (que igual funciona, pero es menos explícito cuándo se mezclan validaciones intermedias).
3. **Depuración más sencilla:** los stack traces y el uso de breakpoints en herramientas de desarrollo son más claros con `async/await`, ya que el flujo de ejecución es más parecido al código síncrono tradicional.
4. **Combinación con lógica condicional:** es más natural usar `if`, `for` o `try/catch` alrededor de un `await` que dentro de una cadena de `.then()`.

En resumen, las promesas son la base del manejo moderno de asincronía en JavaScript, y `async/await` es una forma más clara y mantenible de consumirlas, especialmente cuando hay múltiples pasos asíncronos dependientes entre sí.

---

### 3. Bootstrap

**Pregunta:** ¿Cómo funciona el sistema de grillas (Grid System) de Bootstrap basado en Flexbox y para qué sirven los *breakpoints* (como `col-md-`, `col-lg-`) en el diseño responsivo?

**Respuesta:**

El sistema de grillas de Bootstrap organiza el contenido en una estructura de **tres niveles**:

1. **`.container` (o `.container-fluid`):** el contenedor más externo, que centra el contenido y le aplica un ancho máximo (o 100% del ancho en el caso fluido), además de un *padding* lateral.
2. **`.row`:** una fila que agrupa columnas. Internamente, Bootstrap convierte `.row` en un contenedor **Flexbox** (`display: flex; flex-wrap: wrap;`), lo que permite que las columnas se acomoden en una línea horizontal y salten de línea automáticamente cuando no caben.
3. **`.col`, `.col-*`:** las columnas propiamente dichas, que son los elementos flex (`flex items`) dentro de la fila.

Cada fila se divide conceptualmente en **12 columnas**. Por eso, una clase como `col-md-6` significa "ocupa 6 de las 12 columnas disponibles" (es decir, el 50% del ancho) a partir del breakpoint `md`. Al estar basado en Flexbox, Bootstrap aprovecha propiedades como `flex-grow`, `flex-shrink` y `flex-basis` para calcular automáticamente el ancho de cada columna, y permite además utilidades de alineación (`justify-content`, `align-items`) y de orden (`order-*`) directamente sobre las columnas, cosa que con el antiguo sistema basado en `float` era mucho más complicado.

**Los *breakpoints*** (`sm`, `md`, `lg`, `xl`, `xxl`) son anchos de pantalla de referencia a partir de los cuales cambia el comportamiento de las columnas. Cada breakpoint corresponde a un ancho mínimo de viewport:

| Clase    | Abreviatura | Ancho mínimo aproximado |
|----------|-------------|--------------------------|
| `col-`   | extra small | < 576px (por defecto, sin prefijo) |
| `col-sm-`| small       | ≥ 576px |
| `col-md-`| medium      | ≥ 768px |
| `col-lg-`| large       | ≥ 992px |
| `col-xl-`| extra large | ≥ 1200px |
| `col-xxl-`| extra extra large | ≥ 1400px |

La clave del diseño responsivo es que estas clases se pueden **combinar** en un mismo elemento para que su comportamiento cambie según el tamaño de pantalla. Por ejemplo:

```html
<div class="col-12 col-md-6 col-lg-4">
  Contenido
</div>
```

Esto significa: "ocupa el 100% del ancho en pantallas pequeñas (móvil), el 50% a partir de tablets (`md`), y el 33% a partir de pantallas grandes (`lg`, como laptops/escritorio)". Gracias a este enfoque *mobile-first* (las reglas se aplican desde el breakpoint más pequeño hacia arriba, usando `min-width` en los media queries), es posible construir layouts que se adaptan fluidamente a distintos dispositivos sin necesidad de escribir media queries personalizadas para cada caso, delegando esa complejidad al sistema de grillas de Bootstrap.

---

### 4. Introducción a Vue.js

**Pregunta:** ¿Qué es el DOM Virtual en Vue.js y por qué mejora el rendimiento de una aplicación web en comparación con la manipulación directa del DOM tradicional?

**Respuesta:**

El **DOM** (Document Object Model) es la representación en memoria de la estructura HTML de una página, sobre la cual el navegador dibuja lo que vemos en pantalla. Manipular el DOM directamente (por ejemplo, con `document.getElementById(...).innerHTML = ...` en JavaScript "vanilla") es una operación **costosa**, porque cada cambio puede forzar al navegador a recalcular estilos, volver a hacer *layout* (reflow) y repintar (*repaint*) partes de la página, incluso cuando el cambio real es mínimo.

El **DOM Virtual (Virtual DOM)** es una representación **ligera, en memoria**, del DOM real, construida con objetos JavaScript planos (no son nodos reales del navegador). Vue mantiene una copia virtual del árbol de la interfaz y, cuando el estado reactivo de la aplicación cambia, sigue este proceso:

1. **Genera un nuevo árbol virtual** que refleja cómo debería verse la interfaz con el estado actualizado.
2. **Compara (diffing)** ese nuevo árbol virtual con el árbol virtual anterior, usando un algoritmo eficiente de comparación (*diff algorithm*) para detectar exactamente qué nodos cambiaron, se agregaron o se eliminaron.
3. **Aplica (patching)** únicamente esas diferencias mínimas al DOM real, en lugar de volver a renderizar toda la página.

**¿Por qué mejora el rendimiento?**

- **Minimiza las operaciones costosas sobre el DOM real:** en lugar de que cada cambio de estado dispare manipulaciones directas y repetidas del DOM, Vue agrupa los cambios y actualiza el DOM real solo con el conjunto mínimo de operaciones necesarias.
- **Batching de actualizaciones:** Vue agrupa múltiples cambios de estado que ocurren en el mismo "tick" y los aplica juntos en una sola pasada de actualización del DOM, evitando *reflows* y *repaints* innecesarios y repetidos.
- **Cálculo eficiente en memoria:** comparar objetos JavaScript en memoria (el árbol virtual) es mucho más rápido que consultar y modificar directamente el DOM real, que es una API más pesada y con efectos colaterales (reflow/repaint) en el navegador.
- **Abstracción declarativa para el desarrollador:** el desarrollador simplemente declara "así debe verse la interfaz dado este estado" (con la plantilla y los datos reactivos), sin tener que calcular manualmente qué partes del DOM actualizar; Vue se encarga de optimizar esas actualizaciones por detrás.

En resumen, el DOM Virtual actúa como una capa intermedia que permite a Vue calcular de forma eficiente el mínimo conjunto de cambios necesarios antes de tocar el DOM real, reduciendo drásticamente el número de operaciones costosas de renderizado y mejorando el rendimiento general de la aplicación, especialmente en interfaces con actualizaciones frecuentes o complejas.

---

### 5. Reactividad en Vue

**Pregunta:** ¿Cuál es la diferencia fundamental entre el enlace de datos bidireccional (`v-model`) y el enlace unidireccional utilizando propiedades (`props`) y eventos (`emits`) en los componentes de Vue?

**Respuesta:**

La diferencia fundamental está en **la dirección en la que fluye la información** y **quién tiene la responsabilidad de modificar el estado**.

**`v-model` (enlace bidireccional):**

`v-model` es una directiva que sincroniza automáticamente un dato reactivo con un elemento del formulario (o con un componente hijo). El flujo va **en ambos sentidos**:

- Cuando el valor de la variable cambia en el script, el elemento (por ejemplo, un `<input>`) se actualiza automáticamente para reflejarlo.
- Cuando el usuario interactúa con el elemento (escribe en el input, marca un checkbox, etc.), la variable en el script se actualiza automáticamente.

```html
<input v-model="nombre" />
```

En realidad, `v-model` es "azúcar sintáctico": internamente combina un `:value` (o `:model-value` en componentes) con un listener de evento (`@input` o `@update:model-value`). Es decir, Vue genera por detrás algo equivalente a:

```html
<input :value="nombre" @input="nombre = $event.target.value" />
```

Esto es muy cómodo para formularios, pero oculta el flujo de datos: no es inmediatamente obvio, con solo mirar el template, que el valor puede cambiar "desde fuera" del componente padre.

**`props` y `emits` (enlace unidireccional / *one-way data flow*):**

En la arquitectura de componentes de Vue, los datos fluyen de forma **explícita y unidireccional**:

- El componente **padre** pasa datos hacia el **hijo** mediante **`props`**. El hijo recibe esos datos de solo lectura: **no debe modificarlos directamente** (Vue incluso emite una advertencia en consola si se intenta mutar una prop).
- Si el hijo necesita notificar un cambio o una acción al padre, lo hace **emitiendo un evento personalizado** con **`emits`** (`defineEmits` en la Composition API), y es el **padre** quien decide cómo reaccionar a ese evento (normalmente actualizando su propio estado).

```js
// Hijo (DetalleProducto.vue)
const props = defineProps({ producto: Object });
const emit = defineEmits(['cerrar']);

function cerrarDetalle() {
  emit('cerrar');
}
```

```html
<!-- Padre (Catalogo.vue) -->
<DetalleProducto :producto="productoSeleccionado" @cerrar="productoSeleccionado = null" />
```

Este patrón se resume habitualmente como **"props down, events up"** (las props bajan del padre al hijo, los eventos suben del hijo al padre).

**Diferencia fundamental:**

| Aspecto | `v-model` | `props` + `emits` |
|---|---|---|
| Dirección del flujo | Bidireccional (implícito) | Unidireccional explícito (padre → hijo con props, hijo → padre con emits) |
| Quién modifica el estado | El propio elemento/componente puede "escribir" de vuelta en el dato del padre automáticamente | El hijo **nunca** modifica la prop directamente; solo emite una señal (evento) para que el **padre** decida si y cómo cambiar su estado |
| Caso de uso típico | Formularios e inputs donde se quiere sincronización automática y simple | Comunicación entre componentes padre-hijo donde se busca mantener trazabilidad y previsibilidad del origen de cada cambio de estado |
| Transparencia del flujo de datos | Menos explícito (la "magia" ocurre detrás de la directiva) | Más explícito y predecible, ya que cada cambio de estado se origina claramente en un único lugar (el padre) |

En esencia, `v-model` prioriza la **conveniencia** para casos de sincronización simple (como formularios), mientras que el patrón `props`/`emits` prioriza la **previsibilidad y el control del flujo de datos**, un principio central en la arquitectura de componentes de Vue (y de otros frameworks como React), donde el estado debe tener una única fuente de verdad y fluir de manera clara y rastreable a través de la aplicación.

---
