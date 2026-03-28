# 📝 Resumen FASE 1.3 — Estado y eventos (`useState`)

---

## 🎯 ¿Qué es el estado?

**Definición:** Datos que React vigila y que provocan re-renderizado cuando cambian.

**Diferencia con variables normales:**
- Variable normal: React NO re-renderiza cuando cambia
- Estado: React SÍ re-renderiza cuando cambia

---

## 🔑 Sintaxis de `useState`

```jsx
import { useState } from 'react';

function MiComponente() {
  const [variable, setVariable] = useState(valorInicial);
  //     ^^^^^^^^  ^^^^^^^^^^^           ^^^^^^^^^^^^^
  //        |           |                      |
  //    variable    función para          valor inicial
  //   de estado   cambiar estado
  
  return (
    <button onClick={() => setVariable(nuevoValor)}>
      {variable}
    </button>
  );
}
```

---

## 📖 Conceptos clave

### 1. **Importar `useState`**
```jsx
import { useState } from 'react';
```

### 2. **Declarar estado**
```jsx
const [numero, setNumero] = useState(0);
const [texto, setTexto] = useState('');
const [activo, setActivo] = useState(false);
const [lista, setLista] = useState([]);
```

### 3. **Leer el estado**
```jsx
<p>{numero}</p>
<p>{activo ? "Sí" : "No"}</p>
```

### 4. **Actualizar el estado**
```jsx
// Valor directo
setNumero(5);

// Función con estado anterior (más seguro)
setNumero(prevNumero => prevNumero + 1);
```

---

## 🎮 Eventos en React

### Eventos comunes

```jsx
// Click
<button onClick={() => alert('Click!')}>Click</button>

// Input
<input 
  value={texto}
  onChange={(e) => setTexto(e.target.value)}
/>

// Submit
<form onSubmit={(e) => {
  e.preventDefault();
  console.log('Enviado');
}}>
  <button type="submit">Enviar</button>
</form>

// Mouse
<div onMouseEnter={() => console.log('Mouse encima')}>
  Hover
</div>
```

### Diferencias con JS vanilla

| JavaScript vanilla | React |
|---|---|
| `onclick` | `onClick` |
| `onchange` | `onChange` |
| `onsubmit` | `onSubmit` |
| `"miFunc()"` (string) | `{miFunc}` (función) |

---

## 📝 Formularios controlados

**Definición:** Input cuyo valor está sincronizado con el estado.

```jsx
function Formulario() {
  const [nombre, setNombre] = useState('');
  
  return (
    <input
      value={nombre}                          // Muestra el estado
      onChange={(e) => setNombre(e.target.value)} // Actualiza el estado
    />
  );
}
```

**Ventaja:** React siempre sabe qué hay en el input (único punto de verdad).

---

## ✅ Reglas de `useState`

### 1. Solo en componentes funcionales
```jsx
✅ function MiComponente() {
     const [estado, setEstado] = useState(0);
   }

❌ const estado = useState(0); // Fuera de componente
```

### 2. Siempre al inicio del componente
```jsx
function MiComponente() {
  ✅ const [numero, setNumero] = useState(0);
  
  if (condicion) {
    ❌ const [otro, setOtro] = useState(1); // Dentro de if/for/while
  }
}
```

### 3. Usar la función `set`, nunca modificar directamente
```jsx
const [numero, setNumero] = useState(0);

❌ numero = 5;              // React no se entera
✅ setNumero(5);            // React re-renderiza
```

### 4. Función con estado anterior para actualizaciones
```jsx
// ⚠️ Puede dar problemas
setNumero(numero + 1);

// ✅ Más seguro
setNumero(prevNumero => prevNumero + 1);
```

---

## 💡 Ejemplos completos

### Contador
```jsx
import { useState } from 'react';

function Contador() {
  const [numero, setNumero] = useState(0);
  
  return (
    <div>
      <p>Contador: {numero}</p>
      <button onClick={() => setNumero(numero + 1)}>
        Incrementar
      </button>
    </div>
  );
}
```

### Toggle (alternar)
```jsx
function Toggle() {
  const [activo, setActivo] = useState(false);
  
  return (
    <button onClick={() => setActivo(!activo)}>
      {activo ? "Encendido" : "Apagado"}
    </button>
  );
}
```

### Input controlado
```jsx
function Formulario() {
  const [nombre, setNombre] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Nombre:', nombre);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={nombre}
        onChange={(e) => setNombre(e.target.value)}
        placeholder="Tu nombre"
      />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

### Filtro de lista
```jsx
function CatalogoRecetas() {
  const [soloRapidas, setSoloRapidas] = useState(false);
  
  const recetas = [
    { id: 1, nombre: "Carbonara", tiempo: 20 },
    { id: 2, nombre: "Brownie", tiempo: 45 }
  ];
  
  const recetasMostradas = soloRapidas 
    ? recetas.filter(r => r.tiempo < 30)
    : recetas;
  
  return (
    <div>
      <button onClick={() => setSoloRapidas(!soloRapidas)}>
        {soloRapidas ? "Todas" : "Solo rápidas"}
      </button>
      
      {recetasMostradas.map(r => (
        <div key={r.id}>{r.nombre}</div>
      ))}
    </div>
  );
}
```

---

## ✅ Lo esencial para recordar

1. **Estado = datos que React vigila** → cuando cambian, re-renderiza
2. **`useState(valorInicial)`** → devuelve `[variable, setVariable]`
3. **Actualizar con `setVariable()`**, nunca directamente
4. **Eventos en camelCase**: `onClick`, `onChange`, `onSubmit`
5. **Formularios controlados**: `value={estado}` + `onChange`
6. **`e.preventDefault()`** para evitar recargar en formularios

---

## 🔄 Flujo de actualización del estado

1. Usuario hace click → `onClick={() => setNumero(5)}`
2. React actualiza el estado interno
3. React **re-renderiza** el componente
4. El componente se ejecuta de nuevo con el nuevo valor
5. La interfaz se actualiza automáticamente

---

## 🚀 Siguiente paso

**FASE 1.4 — Efectos y ciclo de vida (`useEffect`)**

---

## 📚 Resúmenes anteriores

- [FASE 1.1 — ¿Qué es React?](RESUMEN_FASE_1.1.md)
- [FASE 1.2 — Tu primer componente](RESUMEN_FASE_1.2.md)

---

**Fecha:** Febrero 2025  
**Versión:** 1.0
