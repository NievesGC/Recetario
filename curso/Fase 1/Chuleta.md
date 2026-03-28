# 📚 React — Chuletas de referencia

# 1. Chuleta de eventos en React

En React los eventos se escriben en **camelCase** (no como en HTML) y reciben una función, no un string.

```jsx
// HTML estándar:
<button onclick="miFuncion()">Click</button>

// React:
<button onClick={miFuncion}>Click</button>
```

Todos los manejadores de eventos reciben un objeto **`event`** (o `e`) con información sobre lo que ocurrió. Puedes llamarlo como quieras.

```jsx
<input onChange={(e) => console.log(e.target.value)} />
```

---

## 1.1 Eventos de ratón

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onClick` | Al hacer clic | Botones, tarjetas, cualquier elemento clicable |
| `onDoubleClick` | Al hacer doble clic | Edición en línea, zoom |
| `onMouseEnter` | Al entrar el cursor en el elemento | Tooltips, hover effects |
| `onMouseLeave` | Al salir el cursor del elemento | Cerrar tooltips |
| `onMouseMove` | Al mover el cursor sobre el elemento | Seguimiento del cursor, efectos visuales |
| `onMouseDown` | Al presionar el botón del ratón (sin soltar) | Drag & drop, botones con efecto de pulsación |
| `onMouseUp` | Al soltar el botón del ratón | Fin de drag & drop |
| `onContextMenu` | Al hacer clic derecho | Menús contextuales personalizados |

**Sintaxis:**
```jsx
<div
  onClick={(e) => console.log('clic en', e.target)}
  onMouseEnter={() => setHover(true)}
  onMouseLeave={() => setHover(false)}
>
  Contenido
</div>
```

**Propiedades útiles del objeto `e`:**
- `e.target` → el elemento que recibió el evento
- `e.clientX` / `e.clientY` → posición del cursor en la pantalla
- `e.button` → qué botón del ratón se pulsó (0=izquierdo, 1=central, 2=derecho)

---

## 1.2 Eventos de teclado

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onKeyDown` | Al presionar una tecla (se repite si se mantiene) | Atajos de teclado, juegos |
| `onKeyUp` | Al soltar una tecla | Validación al terminar de escribir |
| `onKeyPress` | ⚠️ Obsoleto. Usar `onKeyDown` en su lugar | — |

**Sintaxis:**
```jsx
<input
  onKeyDown={(e) => {
    if (e.key === 'Enter') enviarFormulario();
    if (e.key === 'Escape') cerrarModal();
  }}
/>
```

**Propiedades útiles del objeto `e`:**
- `e.key` → nombre de la tecla pulsada (`'Enter'`, `'Escape'`, `'ArrowUp'`, `'a'`, `' '`...)
- `e.code` → código físico de la tecla (`'KeyA'`, `'Space'`, `'Enter'`...)
- `e.ctrlKey` / `e.shiftKey` / `e.altKey` / `e.metaKey` → si se pulsaron teclas modificadoras
- `e.preventDefault()` → evita el comportamiento por defecto del navegador

```jsx
// Detectar Ctrl + S
onKeyDown={(e) => {
  if (e.ctrlKey && e.key === 's') {
    e.preventDefault(); // evita que el navegador abra el diálogo de guardar
    guardar();
  }
}}
```

---

## 1.3 Eventos de formulario

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onChange` | Al cambiar el valor de un input, select o textarea | Componentes controlados, validación en tiempo real |
| `onInput` | Al escribir en un input (más inmediato que onChange) | Similar a onChange, menos común en React |
| `onSubmit` | Al enviar un formulario | Procesar datos del formulario |
| `onReset` | Al resetear un formulario | Limpiar el formulario |
| `onSelect` | Al seleccionar texto dentro de un input | Copiar texto, formatear selección |

**Sintaxis:**
```jsx
// Input controlado
<input
  value={nombre}
  onChange={(e) => setNombre(e.target.value)}
/>

// Select
<select value={opcion} onChange={(e) => setOpcion(e.target.value)}>
  <option value="a">Opción A</option>
  <option value="b">Opción B</option>
</select>

// Formulario
<form onSubmit={(e) => {
  e.preventDefault(); // MUY IMPORTANTE: evita que la página se recargue
  procesarDatos();
}}>
```

**Propiedades útiles del objeto `e`:**
- `e.target.value` → valor actual del input
- `e.target.checked` → para checkboxes (`true` o `false`)
- `e.target.files` → para inputs de tipo `file`
- `e.preventDefault()` → imprescindible en `onSubmit` para evitar recarga

---

## 1.4 Eventos de foco

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onFocus` | Al hacer foco en el elemento (clic o Tab) | Resaltar el campo activo, mostrar sugerencias |
| `onBlur` | Al perder el foco | Validar el campo al salir, ocultar sugerencias |

**Sintaxis:**
```jsx
<input
  onFocus={() => setActivo(true)}
  onBlur={(e) => {
    setActivo(false);
    validar(e.target.value);
  }}
/>
```

---

## 1.5 Eventos de scroll y rueda

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onScroll` | Al hacer scroll dentro de un elemento | Carga infinita, barra de progreso de lectura |
| `onWheel` | Al usar la rueda del ratón | Zoom personalizado, carruseles |

**Sintaxis:**
```jsx
<div onScroll={(e) => {
  const { scrollTop, scrollHeight, clientHeight } = e.target;
  const porcentaje = scrollTop / (scrollHeight - clientHeight);
  setProgreso(porcentaje);
}}>
  Contenido largo...
</div>
```

---

## 1.6 Eventos de arrastre (drag & drop)

| Evento | Cuándo se dispara | En qué elemento |
|--------|-------------------|-----------------|
| `onDragStart` | Al empezar a arrastrar | En el elemento que se arrastra |
| `onDrag` | Mientras se arrastra | En el elemento que se arrastra |
| `onDragEnd` | Al soltar (en cualquier sitio) | En el elemento que se arrastra |
| `onDragOver` | Al arrastrar sobre una zona de destino | En la zona de destino |
| `onDragEnter` | Al entrar en una zona de destino | En la zona de destino |
| `onDragLeave` | Al salir de una zona de destino | En la zona de destino |
| `onDrop` | Al soltar en una zona de destino | En la zona de destino |

**Sintaxis básica:**
```jsx
// Elemento arrastrable
<div
  draggable
  onDragStart={(e) => e.dataTransfer.setData('id', item.id)}
>
  Arrástrame
</div>

// Zona de destino
<div
  onDragOver={(e) => e.preventDefault()} // necesario para permitir el drop
  onDrop={(e) => {
    const id = e.dataTransfer.getData('id');
    moverItem(id);
  }}
>
  Suéltame aquí
</div>
```

---

## 1.7 Eventos de portapapeles

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onCopy` | Al copiar contenido | Registrar lo que se copia, modificarlo |
| `onCut` | Al cortar contenido | Similar a onCopy |
| `onPaste` | Al pegar contenido | Limpiar o transformar el contenido pegado |

**Sintaxis:**
```jsx
<input
  onPaste={(e) => {
    const texto = e.clipboardData.getData('text');
    console.log('El usuario pegó:', texto);
  }}
/>
```

---

## 1.8 Eventos táctiles (móvil)

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onTouchStart` | Al tocar la pantalla | Inicio de gesto |
| `onTouchMove` | Al mover el dedo sobre la pantalla | Swipe, pinch-to-zoom |
| `onTouchEnd` | Al levantar el dedo | Fin de gesto |

**Propiedades útiles:**
- `e.touches` → lista de todos los toques activos
- `e.touches[0].clientX` / `e.touches[0].clientY` → posición del primer toque

---

## 1.9 Eventos de imagen y multimedia

| Evento | Cuándo se dispara | Uso típico |
|--------|-------------------|------------|
| `onLoad` | Al cargar una imagen o recurso | Mostrar imagen cuando esté lista, ocultar spinner |
| `onError` | Al fallar la carga | Mostrar imagen de fallback |
| `onPlay` | Al iniciar reproducción de audio/video | Actualizar estado de reproductor |
| `onPause` | Al pausar reproducción | Actualizar estado de reproductor |
| `onEnded` | Al terminar reproducción | Pasar al siguiente elemento |
| `onTimeUpdate` | Al avanzar el tiempo de reproducción | Actualizar barra de progreso |

**Sintaxis:**
```jsx
<img
  src={url}
  onLoad={() => setImagenCargada(true)}
  onError={() => setSrc('/imagen-fallback.png')}
/>
```

---

---

# 2. Chuleta de métodos y utilidades JavaScript

Métodos nativos de JavaScript que usarás constantemente al trabajar con React. Organizados por el tipo de dato al que se aplican.

---

## 2.1 Métodos de String

| Método | Qué hace | Sintaxis | Ejemplo |
|--------|----------|----------|---------|
| `.padStart(n, char)` | Rellena por la izquierda hasta longitud n | `str.padStart(2, '0')` | `'5'.padStart(2,'0')` → `'05'` |
| `.padEnd(n, char)` | Rellena por la derecha hasta longitud n | `str.padEnd(4, '.')` | `'hi'.padEnd(4,'.')` → `'hi..'` |
| `.toUpperCase()` | Convierte a mayúsculas | `str.toUpperCase()` | `'hola'.toUpperCase()` → `'HOLA'` |
| `.toLowerCase()` | Convierte a minúsculas | `str.toLowerCase()` | `'HOLA'.toLowerCase()` → `'hola'` |
| `.trim()` | Elimina espacios al inicio y al final | `str.trim()` | `' hola '.trim()` → `'hola'` |
| `.trimStart()` | Elimina espacios solo al inicio | `str.trimStart()` | `' hola '.trimStart()` → `'hola '` |
| `.trimEnd()` | Elimina espacios solo al final | `str.trimEnd()` | `' hola '.trimEnd()` → `' hola'` |
| `.includes(sub)` | Comprueba si contiene una subcadena | `str.includes('texto')` | `'hola mundo'.includes('mundo')` → `true` |
| `.startsWith(sub)` | Comprueba si empieza por una subcadena | `str.startsWith('ho')` | `'hola'.startsWith('ho')` → `true` |
| `.endsWith(sub)` | Comprueba si termina por una subcadena | `str.endsWith('la')` | `'hola'.endsWith('la')` → `true` |
| `.replace(a, b)` | Reemplaza la primera coincidencia | `str.replace('a', 'b')` | `'banana'.replace('a','o')` → `'bonana'` |
| `.replaceAll(a, b)` | Reemplaza todas las coincidencias | `str.replaceAll('a', 'o')` | `'banana'.replaceAll('a','o')` → `'bonono'` |
| `.split(sep)` | Divide el string en array según separador | `str.split(',')` | `'a,b,c'.split(',')` → `['a','b','c']` |
| `.slice(start, end)` | Extrae una parte del string | `str.slice(0, 3)` | `'hola'.slice(0,2)` → `'ho'` |
| `.charAt(i)` | Devuelve el carácter en la posición i | `str.charAt(0)` | `'hola'.charAt(1)` → `'o'` |
| `.length` | Propiedad: número de caracteres | `str.length` | `'hola'.length` → `4` |

**Uso en React:**
```jsx
// Mostrar nombre con inicial en mayúscula
const nombreFormateado = nombre.charAt(0).toUpperCase() + nombre.slice(1).toLowerCase();

// Mostrar tiempo en formato MM:SS
<p>{String(minutos).padStart(2, '0')} : {String(segundos).padStart(2, '0')}</p>

// Validar que el email tiene formato básico
const esEmailValido = email.includes('@') && email.includes('.');
```

---

## 2.2 Métodos de Array

Los más importantes en React porque se usan constantemente para transformar y renderizar listas.

| Método | Qué hace | Devuelve | Mutación |
|--------|----------|----------|----------|
| `.map(fn)` | Transforma cada elemento | Nuevo array | ❌ No muta |
| `.filter(fn)` | Filtra elementos que cumplen condición | Nuevo array | ❌ No muta |
| `.find(fn)` | Devuelve el primer elemento que cumple condición | Un elemento o `undefined` | ❌ No muta |
| `.findIndex(fn)` | Devuelve el índice del primer elemento que cumple condición | Número o `-1` | ❌ No muta |
| `.includes(val)` | Comprueba si el array contiene un valor | `true` / `false` | ❌ No muta |
| `.some(fn)` | Comprueba si algún elemento cumple condición | `true` / `false` | ❌ No muta |
| `.every(fn)` | Comprueba si todos los elementos cumplen condición | `true` / `false` | ❌ No muta |
| `.reduce(fn, inicial)` | Reduce el array a un único valor | Cualquier valor | ❌ No muta |
| `.sort(fn)` | Ordena el array | El mismo array | ⚠️ Muta el original |
| `.reverse()` | Invierte el array | El mismo array | ⚠️ Muta el original |
| `.flat()` | Aplana arrays anidados | Nuevo array | ❌ No muta |
| `.flatMap(fn)` | Combina map + flat | Nuevo array | ❌ No muta |
| `.slice(start, end)` | Extrae una porción del array | Nuevo array | ❌ No muta |
| `.concat(arr)` | Une dos arrays | Nuevo array | ❌ No muta |
| `.join(sep)` | Une todos los elementos en un string | String | ❌ No muta |
| `.length` | Número de elementos | Número | — |

**⚠️ Importante en React:** nunca mutes el estado directamente. Para `sort` y `reverse`, copia primero el array:

```jsx
// ❌ MAL: mutación directa
setItems(items.sort());

// ✅ BIEN: copia y luego ordena
setItems([...items].sort());
```

**Uso en React:**
```jsx
// Renderizar una lista (map — el más usado en React)
{recetas.map(receta => (
  <RecetaCard key={receta.id} receta={receta} />
))}

// Filtrar resultados
const recetasVeganas = recetas.filter(r => r.tipo === 'vegana');

// Buscar un elemento concreto
const receta = recetas.find(r => r.id === id);

// Comprobar si algo existe
const yaGuardada = favoritos.includes(recetaId);

// Sumar valores
const totalCalorias = ingredientes.reduce((suma, ing) => suma + ing.calorias, 0);

// Añadir elemento sin mutar (patrón React)
setItems(prev => [...prev, nuevoItem]);

// Eliminar elemento sin mutar (patrón React)
setItems(prev => prev.filter(item => item.id !== idAEliminar));

// Actualizar elemento sin mutar (patrón React)
setItems(prev => prev.map(item =>
  item.id === id ? { ...item, nombre: nuevoNombre } : item
));
```

---

## 2.3 Métodos de Number

| Método / Función | Qué hace | Sintaxis | Ejemplo |
|-----------------|----------|----------|---------|
| `Math.floor(n)` | Redondea hacia abajo | `Math.floor(4.9)` | → `4` |
| `Math.ceil(n)` | Redondea hacia arriba | `Math.ceil(4.1)` | → `5` |
| `Math.round(n)` | Redondea al más cercano | `Math.round(4.5)` | → `5` |
| `Math.abs(n)` | Valor absoluto (sin signo) | `Math.abs(-5)` | → `5` |
| `Math.max(a, b...)` | El mayor de los valores | `Math.max(3, 7, 2)` | → `7` |
| `Math.min(a, b...)` | El menor de los valores | `Math.min(3, 7, 2)` | → `2` |
| `Math.random()` | Número aleatorio entre 0 y 1 | `Math.random()` | → `0.472...` |
| `Math.pow(base, exp)` | Potencia | `Math.pow(2, 3)` | → `8` |
| `Math.sqrt(n)` | Raíz cuadrada | `Math.sqrt(9)` | → `3` |
| `Number.isNaN(n)` | Comprueba si es NaN | `Number.isNaN(NaN)` | → `true` |
| `Number.isInteger(n)` | Comprueba si es entero | `Number.isInteger(4.0)` | → `true` |
| `parseInt(str)` | Convierte string a entero | `parseInt('42px')` | → `42` |
| `parseFloat(str)` | Convierte string a decimal | `parseFloat('3.14')` | → `3.14` |
| `.toFixed(n)` | Fija el número de decimales | `(3.14159).toFixed(2)` | → `'3.14'` |
| `%` (módulo) | Resto de una división | `10 % 3` | → `1` |

**Uso en React:**
```jsx
// Calcular minutos y segundos desde tiempo total
const minutos = Math.floor(tiempo / 60);
const segundos = tiempo % 60;

// Limitar un valor entre un mínimo y un máximo
const valorLimitado = Math.min(Math.max(valor, 0), 100);

// Mostrar precio con dos decimales
<p>{precio.toFixed(2)} €</p>

// Número aleatorio entre 0 y 9
const aleatorio = Math.floor(Math.random() * 10);
```

---

## 2.4 Métodos de Object

| Método | Qué hace | Sintaxis | Ejemplo |
|--------|----------|----------|---------|
| `Object.keys(obj)` | Array con las claves del objeto | `Object.keys(obj)` | `{a:1,b:2}` → `['a','b']` |
| `Object.values(obj)` | Array con los valores del objeto | `Object.values(obj)` | `{a:1,b:2}` → `[1,2]` |
| `Object.entries(obj)` | Array de pares `[clave, valor]` | `Object.entries(obj)` | `{a:1}` → `[['a',1]]` |
| `Object.assign(dest, src)` | Copia propiedades de src a dest | `Object.assign({}, obj, extra)` | Crea copia con cambios |
| `{ ...obj }` (spread) | Copia superficial de un objeto | `{ ...obj, nombre: 'nuevo' }` | Copia con propiedad modificada |
| `Object.freeze(obj)` | Hace el objeto inmutable | `Object.freeze(obj)` | No se pueden modificar sus propiedades |

**Uso en React:**
```jsx
// Actualizar una propiedad del estado sin mutar (patrón React)
setUsuario(prev => ({ ...prev, nombre: nuevoNombre }));

// Recorrer las propiedades de un objeto
Object.entries(filtros).map(([clave, valor]) => (
  <p key={clave}>{clave}: {valor}</p>
))

// Comprobar si un objeto tiene propiedades
Object.keys(errores).length > 0 // true si hay errores
```

---

## 2.5 Métodos de Date

| Método | Qué hace | Ejemplo |
|--------|----------|---------|
| `new Date()` | Fecha y hora actual | `new Date()` |
| `new Date(string)` | Fecha desde un string | `new Date('2026-01-15')` |
| `.getFullYear()` | Año (4 dígitos) | `fecha.getFullYear()` → `2026` |
| `.getMonth()` | Mes (0=enero, 11=diciembre) | `fecha.getMonth()` → `0` |
| `.getDate()` | Día del mes (1-31) | `fecha.getDate()` → `15` |
| `.getDay()` | Día de la semana (0=domingo, 6=sábado) | `fecha.getDay()` → `3` |
| `.getHours()` | Hora (0-23) | `fecha.getHours()` → `18` |
| `.getMinutes()` | Minutos (0-59) | `fecha.getMinutes()` → `30` |
| `.getSeconds()` | Segundos (0-59) | `fecha.getSeconds()` → `45` |
| `.getTime()` | Milisegundos desde el 1 enero 1970 | `fecha.getTime()` → `1736956245000` |
| `.toLocaleDateString()` | Fecha en formato local | `'15/1/2026'` |
| `.toLocaleTimeString()` | Hora en formato local | `'18:30:45'` |
| `.toLocaleString()` | Fecha y hora en formato local | `'15/1/2026, 18:30:45'` |
| `.toISOString()` | Formato ISO 8601 | `'2026-01-15T18:30:45.000Z'` |

**Uso en React:**
```jsx
// Mostrar fecha de publicación
const fecha = new Date(receta.created_at);
<p>Publicada el {fecha.toLocaleDateString()}</p>

// Calcular tiempo transcurrido
const ahora = new Date().getTime();
const publicacion = new Date(receta.created_at).getTime();
const diasTranscurridos = Math.floor((ahora - publicacion) / (1000 * 60 * 60 * 24));
```

---

## 2.6 Operadores y sintaxis moderna de JavaScript

Estos no son métodos sino operadores y características del lenguaje que usarás constantemente en React.

| Operador | Nombre | Qué hace | Ejemplo |
|----------|--------|----------|---------|
| `...` | Spread | Expande un array u objeto | `[...arr, nuevoItem]` |
| `?.` | Optional chaining | Accede a una propiedad solo si existe | `usuario?.direccion?.ciudad` |
| `??` | Nullish coalescing | Valor por defecto si es null o undefined | `nombre ?? 'Anónimo'` |
| `\|\|` | OR lógico | Valor por defecto si es falsy (0, '', false, null...) | `nombre \|\| 'Anónimo'` |
| `&&` | AND lógico | Renderizado condicional | `{cargando && <Spinner />}` |
| `? :` | Ternario | Condición en línea | `{activo ? 'Sí' : 'No'}` |
| `===` | Igualdad estricta | Compara valor y tipo | `valor === 0` |
| `!==` | Desigualdad estricta | Distinto en valor o tipo | `valor !== null` |

**Uso en React:**
```jsx
// Spread para copiar sin mutar
const nuevoEstado = { ...estado, nombre: 'nuevo' };
const nuevaLista = [...lista, nuevoElemento];

// Optional chaining: evita errores si el dato aún no ha llegado
<p>{usuario?.perfil?.bio ?? 'Sin descripción'}</p>

// Renderizado condicional con &&
{error && <MensajeError mensaje={error} />}

// Renderizado condicional con ternario
{cargando ? <Spinner /> : <Contenido />}

// Diferencia entre ?? y ||
// || considera falsy también a 0, '', false
0 || 'defecto'   // → 'defecto' (puede ser problema si 0 es válido)
// ?? solo considera null y undefined
0 ?? 'defecto'   // → 0 (correcto si 0 es un valor válido)
```

---

## 2.7 JSON

Muy usado para comunicarse con APIs y para guardar datos.

| Método | Qué hace | Ejemplo |
|--------|----------|---------|
| `JSON.stringify(obj)` | Convierte objeto a string JSON | `JSON.stringify({a:1})` → `'{"a":1}'` |
| `JSON.parse(str)` | Convierte string JSON a objeto | `JSON.parse('{"a":1}')` → `{a:1}` |

**Uso en React:**
```jsx
// Guardar en localStorage
localStorage.setItem('usuario', JSON.stringify(usuario));

// Leer de localStorage
const usuario = JSON.parse(localStorage.getItem('usuario'));

// Hacer una copia profunda de un objeto (truco sencillo)
const copia = JSON.parse(JSON.stringify(objeto));
```

---

## 2.8 Promesas y async/await

Para trabajar con operaciones asíncronas, especialmente peticiones a APIs.

| Concepto | Qué hace | Cuándo usarlo |
|----------|----------|---------------|
| `Promise` | Representa una operación que terminará en el futuro | Base de todo lo asíncrono |
| `.then(fn)` | Ejecuta fn cuando la promesa se resuelve | Encadenado de operaciones asíncronas |
| `.catch(fn)` | Ejecuta fn si la promesa falla | Gestión de errores |
| `.finally(fn)` | Ejecuta fn siempre, haya éxito o error | Ocultar spinner de carga |
| `async/await` | Sintaxis para trabajar con promesas de forma más legible | En funciones que hacen peticiones |
| `Promise.all([...])` | Espera a que todas las promesas terminen | Cuando necesitas varios datos a la vez |

**Uso en React:**
```jsx
// Petición a API dentro de useEffect
useEffect(() => {
  const cargarRecetas = async () => {
    try {
      const respuesta = await fetch('https://api.ejemplo.com/recetas');
      const datos = await respuesta.json();
      setRecetas(datos);
    } catch (error) {
      setError('No se pudieron cargar las recetas');
    } finally {
      setCargando(false);
    }
  };

  cargarRecetas();
}, []);
```

---

*Documento generado como referencia de aprendizaje autónomo. Versión 1.0.*