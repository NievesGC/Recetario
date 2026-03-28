# 📘 Lección 1.4: Efectos y ciclo de vida (useEffect)

## 🎯 Conceptos clave

### ¿Qué es un efecto secundario?

Un **efecto secundario** (side effect) es cualquier código que afecta al mundo exterior del componente, fuera del flujo normal de renderizado de React.

**Flujo normal de React:**
1. React ejecuta tu función componente
2. Calcula qué JSX devuelves
3. Actualiza el DOM si es necesario
4. El navegador pinta la pantalla

**Con efectos secundarios:**
- Hacer peticiones HTTP a una API
- Suscribirse a eventos del navegador (scroll, resize, etc.)
- Actualizar el título de la página
- Guardar datos en `localStorage`
- Iniciar temporizadores

---

## 🪝 El hook useEffect

`useEffect` es el hook que nos permite ejecutar código **después** de que React haya renderizado el componente.

### Sintaxis básica

```javascript
import { useEffect } from 'react';

function MiComponente() {
  useEffect(() => {
    // Código que se ejecuta después del render
    console.log('El componente se ha renderizado');
  });

  return <div>Hola</div>;
}
```

**⚠️ Problema:** Sin array de dependencias, este efecto se ejecuta en **cada render**.

---

## 🎯 Array de dependencias

El segundo parámetro de `useEffect` es un array que controla **cuándo** se ejecuta el efecto.

### 1. Efecto que se ejecuta solo una vez (al montar)

```javascript
useEffect(() => {
  console.log('Esto se ejecuta solo cuando el componente aparece');
}, []); // Array vacío = solo al montar
```

**Casos de uso:**
- Hacer una petición HTTP al cargar el componente
- Suscribirse a eventos globales
- Inicializar librerías de terceros

---

### 2. Efecto que se ejecuta cuando cambia una variable específica

```javascript
function Buscador() {
  const [termino, setTermino] = useState('');

  useEffect(() => {
    console.log('El usuario buscó:', termino);
    // Aquí haríamos una petición HTTP con el término
  }, [termino]); // Se ejecuta cada vez que 'termino' cambia

  return (
    <input 
      value={termino}
      onChange={(e) => setTermino(e.target.value)}
    />
  );
}
```

**Regla importante:** Si usas una variable dentro de `useEffect` y esa variable puede cambiar, debe estar en el array de dependencias.

---

### 3. Efecto sin dependencias (se ejecuta en cada render)

```javascript
useEffect(() => {
  console.log('Esto se ejecuta en cada render');
});
// Sin array de dependencias = se ejecuta siempre
```

**⚠️ Usar con cuidado:** Normalmente quieres controlar cuándo se ejecutan los efectos.

---

## 🧹 Limpieza de efectos (cleanup)

Algunos efectos necesitan "limpiarse" cuando el componente se desmonta o antes de volver a ejecutarse.

### ¿Cuándo es necesario limpiar?

- Si te suscribes a un evento → debes desuscribirte
- Si inicias un temporizador → debes cancelarlo
- Si abres una conexión → debes cerrarla

### Sintaxis de limpieza

```javascript
useEffect(() => {
  // 1. Configurar el efecto
  const interval = setInterval(() => {
    console.log('Tick');
  }, 1000);

  // 2. Devolver una función de limpieza
  return () => {
    clearInterval(interval);
    console.log('Limpieza: temporizador cancelado');
  };
}, []);
```

**Cuándo se ejecuta la limpieza:**
- Antes de que el efecto se ejecute de nuevo
- Cuando el componente se desmonta

---

## 🔄 Ciclo de vida de un componente

React tiene tres fases en la vida de un componente:

### 1. Montaje (mounting)
El componente aparece en pantalla por primera vez.

```javascript
useEffect(() => {
  console.log('Componente montado');
}, []); // Solo al montar
```

---

### 2. Actualización (updating)
El componente se vuelve a renderizar porque cambió su estado o sus props.

```javascript
useEffect(() => {
  console.log('Componente actualizado');
}, [contador]); // Cada vez que 'contador' cambia
```

---

### 3. Desmontaje (unmounting)
El componente desaparece de la pantalla.

```javascript
useEffect(() => {
  return () => {
    console.log('Componente desmontado');
  };
}, []);
```

---

## 💻 Funciones de JavaScript usadas en el ejemplo

### `new Date()`

**¿Qué es?**
- Función **nativa de JavaScript** (NO es de React)
- Crea un objeto con la fecha y hora actual del sistema

**Ejemplo:**
```javascript
const ahora = new Date();
console.log(ahora); 
// Resultado: Tue Feb 25 2026 18:30:45 GMT+0100
```

Cada vez que ejecutas `new Date()`, obtienes la hora **en ese preciso momento**.

---

### `setInterval(función, milisegundos)`

**¿Qué es?**
- Función **nativa de JavaScript** (NO es de React)
- Ejecuta una función **repetidamente** cada X milisegundos

**Sintaxis:**
```javascript
setInterval(función_a_ejecutar, milisegundos)
```

**Ejemplo:**
```javascript
setInterval(() => {
  console.log('Hola');
}, 1000);
// Imprime "Hola" cada 1 segundo (1000 ms = 1 segundo)
```

**¿Por qué 1000?**
- JavaScript usa **milisegundos** como unidad estándar
- 1000 ms = 1 segundo
- 500 ms = 0.5 segundos
- 2000 ms = 2 segundos

---

### El ID del temporizador

`setInterval` devuelve un **ID único** (un número) que identifica ese temporizador.

```javascript
const intervalo = setInterval(() => console.log("hola"), 1000);
console.log(intervalo); // Ej: 123
```

**¿Qué es ese número?**
- Es un **identificador único** (como un DNI o ticket)
- **NO es un contador** de ejecuciones
- Sirve para **cancelar** ese temporizador específico más tarde

**Analogía:**
- Como cuando pides comida y te dan el número de pedido #123
- Ese número no indica cuántas veces han cocinado
- Es solo tu número para identificar **tu pedido específico**

**El ID se mantiene constante:**
```
Tiempo:  0s    1s    2s    3s    4s
         |     |     |     |     |
ID:      123   123   123   123   123  ← Siempre el mismo
Ejecuta: hola  hola  hola  hola  hola
```

Un solo temporizador con ID fijo, ejecutándose repetidamente.

---

### `clearInterval(id)`

**¿Qué es?**
- Función **nativa de JavaScript** (NO es de React)
- **Cancela** un temporizador creado con `setInterval`

**Sintaxis:**
```javascript
const intervalo = setInterval(() => console.log("tick"), 1000);
// Crear temporizador

clearInterval(intervalo);
// Cancelar temporizador
```

**¿Por qué es necesario?**
- Si no cancelas el temporizador, sigue ejecutándose aunque el componente desaparezca
- Esto causa **fugas de memoria** (memory leaks)
- React se queda ejecutando código que ya no necesitas

**Ejemplo de problema:**
```javascript
// ❌ MAL: Sin limpieza
useEffect(() => {
  setInterval(() => console.log("tick"), 1000);
}, []);
// El temporizador sigue corriendo aunque el componente desaparezca

// ✅ BIEN: Con limpieza
useEffect(() => {
  const timer = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(timer);
}, []);
// El temporizador se cancela cuando el componente desaparece
```

---

### `.toLocaleTimeString()`

**¿Qué es?**
- Método **nativo de JavaScript** (NO es de React)
- Convierte un objeto `Date` en un string con formato de hora legible

**Ejemplo:**
```javascript
const fecha = new Date();
console.log(fecha); 
// Tue Feb 25 2026 18:30:45 GMT+0100

console.log(fecha.toLocaleTimeString());
// 18:30:45
```

Convierte la fecha completa en solo la parte de la hora en formato local.

---

## 📊 Ejemplo completo explicado: Reloj interactivo

```javascript
import { useState, useEffect } from 'react';

function RelojInteractivo() {
  // ESTADO: hora actual y si está pausado
  const [hora, setHora] = useState(new Date());
  const [pausado, setPausado] = useState(false);

  // EFECTO: Se ejecuta después del render y cuando cambia 'pausado'
  useEffect(() => {
    console.log('🔵 useEffect se ejecutó');
    
    // Solo crear el intervalo si NO está pausado
    if (!pausado) {
      console.log('✅ Creando intervalo (reloj activo)');
      
      // Crear temporizador que actualiza la hora cada segundo
      const intervalo = setInterval(() => {
        console.log('⏰ Tick - actualizando hora');
        setHora(new Date()); // Actualiza 'hora' con la hora actual
      }, 1000);

      // LIMPIEZA: Cancelar temporizador cuando sea necesario
      return () => {
        console.log('🧹 Limpiando intervalo');
        clearInterval(intervalo);
      };
    } else {
      console.log('⏸️ Reloj pausado, no se crea intervalo');
    }
  }, [pausado]); // ← Se ejecuta cuando cambia 'pausado'

  // EVENTOS: Funciones que se ejecutan al hacer clic
  const togglePausa = () => {
    setPausado(!pausado); // Cambia pausado de true a false o viceversa
  };

  const resetear = () => {
    setHora(new Date()); // Actualiza la hora a la hora actual
  };

  console.log('🎨 RENDER - El componente se está renderizando');

  // JSX: Lo que se muestra en pantalla
  return (
    <div>
      <h2>Reloj interactivo</h2>
      <p>{hora.toLocaleTimeString()}</p>
      <button onClick={togglePausa}>
        {pausado ? 'Reanudar' : 'Pausar'}
      </button>
      <button onClick={resetear}>Reset</button>
    </div>
  );
}
```

---

## 🔄 Flujo de ejecución paso a paso

### 1. Carga inicial
```
1. React ejecuta App()
   └─> useState crea 'hora' con new Date() y 'pausado' con false
   
2. React renderiza el JSX
   └─> console.log('🎨 RENDER')
   └─> Muestra la hora con hora.toLocaleTimeString()
   
3. React ejecuta useEffect (DESPUÉS del render)
   └─> console.log('🔵 useEffect se ejecutó')
   └─> pausado es false → crea setInterval
       └─> Cada 1 segundo: setHora(new Date())
           └─> Cambia 'hora' → React re-renderiza
```

---

### 2. Usuario hace clic en "Pausar"
```
1. onClick ejecuta togglePausa()
   └─> setPausado(true) cambia 'pausado' a true
   
2. React detecta cambio de estado → re-renderiza
   └─> console.log('🎨 RENDER')
   
3. React ejecuta useEffect porque cambió 'pausado'
   └─> Primero ejecuta la LIMPIEZA del efecto anterior
       └─> console.log('🧹 Limpiando intervalo')
       └─> clearInterval(intervalo) cancela el temporizador
   └─> Luego ejecuta el nuevo efecto
       └─> pausado es true → NO crea setInterval
       └─> console.log('⏸️ Reloj pausado')
```

---

### 3. Usuario hace clic en "Reanudar"
```
1. onClick ejecuta togglePausa()
   └─> setPausado(false) cambia 'pausado' a false
   
2. React detecta cambio → re-renderiza
   └─> console.log('🎨 RENDER')
   
3. React ejecuta useEffect porque cambió 'pausado'
   └─> pausado es false → crea setInterval de nuevo
   └─> Vuelven los 'Tick' cada segundo
```

---

### 4. Usuario hace clic en "Reset"
```
1. onClick ejecuta resetear()
   └─> setHora(new Date()) actualiza 'hora'
   
2. React detecta cambio → re-renderiza
   └─> console.log('🎨 RENDER')
   
3. ¿Se ejecuta useEffect?
   └─> NO, porque 'pausado' NO cambió
   └─> El array de dependencias es [pausado]
   └─> Como 'pausado' sigue igual, el efecto no se ejecuta
```

---

## ⚠️ Errores comunes

### ❌ Error 1: Olvidar el array de dependencias

```javascript
// MAL: Se ejecuta en cada render
useEffect(() => {
  hacerPeticionHTTP();
});
```

```javascript
// BIEN: Se ejecuta solo una vez
useEffect(() => {
  hacerPeticionHTTP();
}, []);
```

---

### ❌ Error 2: No limpiar efectos

```javascript
// MAL: El temporizador sigue activo aunque el componente desaparezca
useEffect(() => {
  setInterval(() => console.log('tick'), 1000);
}, []);
```

```javascript
// BIEN: Limpiamos el temporizador
useEffect(() => {
  const timer = setInterval(() => console.log('tick'), 1000);
  return () => clearInterval(timer);
}, []);
```

---

### ❌ Error 3: Dependencias faltantes

```javascript
// MAL: 'contador' se usa dentro pero no está en las dependencias
useEffect(() => {
  console.log(contador);
}, []); // ⚠️ React te avisará con una advertencia
```

```javascript
// BIEN: Todas las variables usadas están en las dependencias
useEffect(() => {
  console.log(contador);
}, [contador]);
```

---

## 🎯 Cuándo usar useEffect

### ✅ SÍ usar useEffect para:

- **Llamadas HTTP/APIs**
  ```javascript
  useEffect(() => {
    fetch('https://api.example.com/datos')
      .then(res => res.json())
      .then(datos => setDatos(datos));
  }, []);
  ```

- **Suscripciones a eventos del navegador**
  ```javascript
  useEffect(() => {
    const manejar = () => setAncho(window.innerWidth);
    window.addEventListener('resize', manejar);
    return () => window.removeEventListener('resize', manejar);
  }, []);
  ```

- **Temporizadores**
  ```javascript
  useEffect(() => {
    const timer = setInterval(() => console.log('tick'), 1000);
    return () => clearInterval(timer);
  }, []);
  ```

- **Actualizar document.title**
  ```javascript
  useEffect(() => {
    document.title = `Contador: ${count}`;
  }, [count]);
  ```

- **localStorage/sessionStorage**
  ```javascript
  useEffect(() => {
    localStorage.setItem('usuario', nombre);
  }, [nombre]);
  ```

---

### ❌ NO usar useEffect para:

- **Cálculos simples**
  ```javascript
  // ❌ MAL
  useEffect(() => {
    setPrecioFinal(precio * 0.9);
  }, [precio]);

  // ✅ BIEN
  const precioFinal = precio * 0.9;
  ```

- **Manejar eventos de usuario**
  ```javascript
  // ❌ MAL
  useEffect(() => {
    const btn = document.getElementById('btn');
    btn.addEventListener('click', manejar);
  }, []);

  // ✅ BIEN
  <button onClick={manejar}>Click</button>
  ```

---

## 📋 Reglas de oro

> **Si usas una variable dentro de `useEffect` y esa variable puede cambiar, debe estar en el array de dependencias.**

> **useEffect es para código que necesita ejecutarse DESPUÉS de que React haya actualizado la pantalla.**

> **Si un efecto crea algo (temporizador, suscripción, conexión), debe limpiarlo.**

---

## 🔑 Conceptos clave para recordar

| Concepto | Qué es | Ejemplo |
|----------|--------|---------|
| **Efecto secundario** | Código que afecta al mundo exterior | HTTP, eventos, timers |
| **useEffect** | Hook para ejecutar código después del render | `useEffect(() => {...}, [])` |
| **Array de dependencias** | Controla cuándo se ejecuta el efecto | `[]`, `[var]`, sin array |
| **Función de limpieza** | Se ejecuta antes de re-ejecutar o al desmontar | `return () => {...}` |
| **Ciclo de vida** | Montaje → Actualización → Desmontaje | 3 fases del componente |
| **setInterval** | Ejecuta función repetidamente cada X ms | `setInterval(fn, 1000)` |
| **clearInterval** | Cancela un temporizador | `clearInterval(id)` |
| **new Date()** | Crea objeto con fecha/hora actual | JavaScript nativo |
| **.toLocaleTimeString()** | Convierte Date a string de hora | `"18:30:45"` |

---

## 📊 Resumen visual: Orden de ejecución

```
┌─────────────────────────────────────┐
│  1. React ejecuta el componente     │
│     └─> useState inicializa estados │
├─────────────────────────────────────┤
│  2. React renderiza el JSX          │
│     └─> Calcula qué mostrar         │
│     └─> Actualiza el DOM            │
├─────────────────────────────────────┤
│  3. Navegador pinta la pantalla     │
├─────────────────────────────────────┤
│  4. React ejecuta useEffect         │
│     └─> DESPUÉS de que todo         │
│         esté pintado                │
└─────────────────────────────────────┘
```

---

## ✅ Checklist de comprensión

Después de esta lección, deberías poder responder:

- [ ] ¿Qué es un efecto secundario?
- [ ] ¿Cuándo se ejecuta `useEffect`?
- [ ] ¿Para qué sirve el array de dependencias?
- [ ] ¿Qué hace `return () => {...}` dentro de `useEffect`?
- [ ] ¿Cuál es la diferencia entre `[]`, `[var]` y sin array?
- [ ] ¿Qué devuelve `setInterval`?
- [ ] ¿Para qué sirve `clearInterval`?
- [ ] ¿Cuándo es necesario limpiar un efecto?

---

**Fin de la Lección 1.4** 🎉

---

---

# 📘 Lección 1.4 — Ampliación: Conceptos avanzados trabajados en la práctica

> Esta sección amplía la Lección 1.4 con conceptos descubiertos durante los ejercicios.
> Todo lo anterior sigue siendo válido. Esto se añade encima.

---

## 🧠 Render vs Efecto: son dos cosas distintas

Una confusión muy habitual es pensar que `useEffect` y el render hacen lo mismo. No es así. Son dos procesos separados que ocurren en momentos distintos.

### El render

El render ocurre **cada vez que cambia cualquier estado** (`useState`). React lee el `return` del componente y actualiza lo que se ve en pantalla. Es automático e inevitable: si cambia el estado, React renderiza.

### El useEffect

El `useEffect` es un cajón aparte donde metes código que **no es pintar**. Cosas como crear intervalos, llamar a APIs, añadir event listeners. El efecto no pinta nada por sí solo. Solo se re-ejecuta cuando cambia algo de su array de dependencias.

### El flujo completo

```
1. Cambia un estado (setTime, setActive...)
2. React renderiza → lee el return del componente y pinta la pantalla  ← SIEMPRE ocurre
3. React mira el array de dependencias del useEffect
4. ¿Cambió algo? → ejecuta el efecto                                   ← SOLO si cambiaron las dependencias
5. ¿No cambió nada? → no toca el efecto
```

**En una frase:**
> El render pinta. El `useEffect` ejecuta lógica. Son dos procesos distintos.

---

## ⏱️ setTimeout vs setInterval

Ambas son funciones nativas de JavaScript, no de React. Pero se comportan de forma distinta.

### `setInterval`
Ejecuta una función **repetidamente** cada X milisegundos, indefinidamente, hasta que se cancela con `clearInterval`.

```javascript
// Se ejecuta cada segundo para siempre (hasta que se cancele)
const id = setInterval(() => {
  console.log('tick');
}, 1000);
```

**Cuándo usarlo:** cuando necesitas que algo ocurra de forma continua y repetida (un reloj, un contador regresivo).

---

### `setTimeout`
Ejecuta una función **una sola vez** después de X milisegundos.

```javascript
// Se ejecuta una vez, después de 3 segundos
const id = setTimeout(() => {
  console.log('han pasado 3 segundos');
}, 3000);
```

**Cuándo usarlo:** cuando necesitas que algo ocurra una vez después de una espera (cambiar de fase, lanzar una animación, encadenar acciones con tiempos distintos).

Se cancela con `clearTimeout(id)`, igual que `clearInterval`.

---

### Diferencia clave

| | `setInterval` | `setTimeout` |
|---|---|---|
| ¿Cuántas veces se ejecuta? | Indefinidamente | Una sola vez |
| ¿Cómo se cancela? | `clearInterval(id)` | `clearTimeout(id)` |
| Uso típico | Relojes, contadores | Transiciones, encadenado de fases |

---

## 🔒 El closure desactualizado (stale closure)

Este es uno de los errores más habituales al usar `setInterval` o `setTimeout` dentro de `useEffect`.

### ¿Qué es?

Cuando se crea un `setInterval`, este hace una **"foto"** de todos los valores que hay en ese momento en el componente. Esa foto no se actualiza sola. Si dentro del intervalo usas una variable de estado directamente, estarás usando el valor congelado de cuando se creó el intervalo, no el valor actual.

```javascript
// ❌ MAL: 'segundos' está congelado en el valor inicial
const timer = setInterval(() => {
  setSegundos(segundos - 1); // 'segundos' siempre vale lo mismo
}, 1000);
```

### ¿Cómo detectarlo?

Si tu contador se queda parado después del primer tick, o siempre resta desde el mismo valor, probablemente tengas un closure desactualizado.

### ¿Cómo solucionarlo?

Con la **forma funcional** de los setters de `useState`.

---

## 🔄 La forma funcional de los setters

Cuando el nuevo valor de un estado **depende del valor anterior**, usa la forma funcional en lugar de leer el estado directamente.

### Sintaxis

```javascript
// Forma directa (puede dar problemas con closures)
setSegundos(segundos - 1);

// Forma funcional (siempre correcta)
setSegundos(s => s - 1);
```

### ¿Qué hace la forma funcional?

En lugar de leer el valor de la variable (que puede estar congelado), le dices a React: *"tú dame el valor real que tienes ahora mismo, y yo te digo qué hacer con él"*. React te entrega el valor actual como parámetro, tú aplicas la transformación, y React guarda el resultado.

El nombre del parámetro (`s`, `t`, `prev`...) es completamente arbitrario. Por convención se usa una letra corta o `prev` (de *previous*, anterior).

### ¿Cuándo usarla?

**Siempre que el nuevo valor dependa del valor anterior**, especialmente dentro de `setInterval` o `setTimeout`.

```javascript
// ✅ BIEN: forma funcional dentro de un intervalo
const timer = setInterval(() => {
  setSegundos(s => s - 1);
}, 1000);
```

```javascript
// ✅ También válida fuera de intervalos
setContador(c => c + 1);
setCorriendo(v => !v);
```

```javascript
// Cuando el nuevo valor NO depende del anterior, no es necesaria
setCorriendo(false); // esto está bien tal cual
```

---

## 🔁 setInterval como bucle independiente de React

`setInterval` funciona como un **bucle independiente del ciclo de React**. Una vez creado, el navegador lo gestiona por su cuenta. React no necesita hacer nada para que siga ejecutándose.

```
1. useEffect se ejecuta → crea el setInterval
2. El efecto termina
3. El setInterval vive solo en el navegador
4. Cada segundo llama a la función que le diste
5. Esa función cambia el estado (setTime, setSegundos...)
6. El cambio de estado provoca un render → React actualiza la pantalla
7. React no toca el efecto (a menos que cambien sus dependencias)
```

React solo aparece al principio (creando el intervalo) y al final de cada tick (repintando la pantalla). En medio, el intervalo funciona completamente solo.

---

## 🚪 Cuándo se ejecuta el `return` de limpieza

El `return` de limpieza de un `useEffect` se ejecuta en dos momentos:

**Momento 1: justo antes de que el efecto se vuelva a ejecutar**

Cada vez que algo del array de dependencias cambia, React primero ejecuta el `return` del efecto anterior y luego ejecuta el efecto de nuevo.

**Momento 2: cuando el componente desaparece de la pantalla**

Si el componente se desmonta, React ejecuta el `return` para limpiar todo lo que quedó vivo.

```
Con time en las dependencias → el efecto se re-ejecuta cada segundo:

Segundo 1:  intervalo creado → tick
Segundo 2:  return ejecutado → intervalo cancelado → nuevo intervalo → tick
Segundo 3:  return ejecutado → intervalo cancelado → nuevo intervalo → tick
```

```
Con solo activeTimer en las dependencias → el efecto se ejecuta una vez:

Al pulsar Start:  intervalo creado → tick, tick, tick... (solo React pinta)
Al pulsar Stop:   return ejecutado → intervalo cancelado
```

---

## 🧩 Fuente de verdad única

Cuando tienes varios estados que representan lo mismo (por ejemplo, `minutos`, `segundos` y `tiempo` referidos al mismo contador), es mejor tener **un único estado como fuente de verdad** y calcular el resto a partir de él.

### ¿Por qué?

- Menos estados que gestionar
- No puedes tener estados inconsistentes entre sí
- El código es más simple y predecible

### Ejemplo

```javascript
// ❌ MAL: tres estados para la misma información
const [tiempo, setTiempo] = useState(1500);
const [minutos, setMinutos] = useState(25);
const [segundos, setSegundos] = useState(0);

// ✅ BIEN: un solo estado, el resto son cálculos
const [tiempo, setTiempo] = useState(1500);

const minutos = Math.floor(tiempo / 60);
const segundos = tiempo % 60;
```

Los operadores que necesitas:
- `Math.floor(n)` → división entera, descarta los decimales
- `%` (módulo) → el resto de una división

```javascript
// Ejemplos con tiempo = 90 segundos
Math.floor(90 / 60) // → 1 (minuto completo)
90 % 60             // → 30 (segundos restantes)
// Resultado: 01:30
```

---

## 🎨 Formatear números con `padStart`

Para mostrar el tiempo en formato `MM:SS` (con ceros a la izquierda cuando el número tiene un solo dígito), se usa el método `padStart` de los strings de JavaScript.

```javascript
String(5).padStart(2, "0")  // → "05"
String(12).padStart(2, "0") // → "12"
```

**`padStart(longitud, carácter)`** rellena el string por la izquierda hasta que tenga la longitud indicada, usando el carácter que le pases.

```javascript
// Ejemplo completo en el JSX
<p>{String(minutos).padStart(2, "0")} : {String(segundos).padStart(2, "0")}</p>
// Muestra: "04 : 07" en lugar de "4 : 7"
```

---

## ⚠️ Lógica condicional dentro del setter

Cuando necesitas parar un intervalo basándote en el valor del estado (por ejemplo, detener cuando llega a 0), no puedes usar la variable directamente porque estará congelada. Debes hacerlo dentro de la forma funcional:

```javascript
// ❌ MAL: 'tiempo' está congelado, nunca llega a 0 dentro del intervalo
setInterval(() => {
  if (tiempo === 0) return;
  setTiempo(tiempo - 1);
}, 1000);

// ✅ BIEN: usamos 't' que es el valor real en ese momento
setInterval(() => {
  setTiempo(t => {
    if (t === 0) return t;   // devolvemos el mismo valor, React no cambia nada
    return t - 1;
  });
}, 1000);
```

**Importante:** cuando no quieres cambiar el estado, devuelve el mismo valor que recibiste (`return t`). Si haces `return` sin valor, JavaScript devuelve `undefined` y React puede comportarse de forma inesperada.

---

## 📋 Reglas de oro ampliadas

> **Usa `===` en lugar de `==` para comparar en JavaScript.** El `==` hace conversiones de tipo automáticas que pueden darte sorpresas. El `===` compara valor y tipo, y es siempre más seguro.

> **Si el nuevo valor de un estado depende del valor anterior, usa la forma funcional del setter.**

> **Ten una única fuente de verdad. No guardes en estado lo que puedes calcular.**

> **Lo que defines dentro de un `useEffect` no existe fuera de él.** Si necesitas usar un valor en el JSX, decláralolo fuera del efecto.

> **Los nombres de las variables deben coincidir de arriba abajo.** Si creas un intervalo como `const timer` y luego intentas cancelar `clearInterval(id)`, JavaScript no sabe de qué le hablas.

---

## 🔑 Conceptos nuevos para recordar

| Concepto | Qué es | Cuándo usarlo |
|----------|--------|---------------|
| **Render** | React pinta la pantalla | Cada vez que cambia un estado |
| **useEffect** | Lógica que no es pintar | Timers, APIs, eventos |
| **setTimeout** | Ejecuta algo una vez tras una espera | Transiciones, encadenado de fases |
| **Closure desactualizado** | Variable de estado congelada dentro de un intervalo | Cuando el contador no avanza bien |
| **Forma funcional del setter** | `setState(v => v - 1)` | Cuando el nuevo valor depende del anterior |
| **Fuente de verdad única** | Un solo estado, el resto se calcula | Cuando varios estados representan lo mismo |
| **padStart** | Rellena un número con ceros a la izquierda | Formato MM:SS |
| **Math.floor** | División entera | Calcular minutos desde segundos totales |
| **% (módulo)** | Resto de una división | Calcular segundos restantes |

---

## ✅ Checklist de comprensión ampliada

- [ ] ¿Cuál es la diferencia entre render y efecto?
- [ ] ¿Cuándo usarías `setTimeout` en lugar de `setInterval`?
- [ ] ¿Qué es un closure desactualizado y cómo lo detectas?
- [ ] ¿Cuándo usas la forma funcional de un setter?
- [ ] ¿Qué diferencia hay entre `setState(valor - 1)` y `setState(v => v - 1)`?
- [ ] ¿Qué significa tener una fuente de verdad única?
- [ ] ¿Qué devuelve `return` sin valor dentro de un setter? ¿Por qué es un problema?
- [ ] ¿En qué dos momentos se ejecuta el `return` de limpieza de un `useEffect`?
- [ ] ¿Qué hace `padStart` y para qué se usa en un timer?
