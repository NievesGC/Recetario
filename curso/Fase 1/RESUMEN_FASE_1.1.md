# 📝 Resumen FASE 1.1 — Fundamentos de React

---

## 🎯 ¿Qué es React?

**Librería de JavaScript para construir interfaces de usuario de forma declarativa.**

---

## 🔑 Conceptos clave

### 1. **Declarativo vs Imperativo**

- **Imperativo (JS vanilla):** Le dices al navegador CÓMO hacer las cosas paso a paso
- **Declarativo (React):** Le dices QUÉ quieres mostrar y React se encarga del resto

---

### 2. **Componente**

**Definición:** Pieza reutilizable de interfaz. Una función que devuelve JSX.

```jsx
function MiComponente() {
  return <div>Hola</div>;
}
```

**Uso:** Renderizar (ejecutar) el componente

```jsx
<MiComponente />
```

---

### 3. **JSX**

**Definición:** Sintaxis que parece HTML pero es JavaScript.

```jsx
const elemento = <h1>Hola mundo</h1>;
```

**Reglas importantes:**
- Un solo elemento raíz por componente
- Atributos en camelCase: `className` (no `class`), `onClick` (no `onclick`)
- JavaScript entre llaves: `{variable}`

---

### 4. **Props**

**Definición:** Datos que se pasan de un componente padre a uno hijo.

```jsx
// Pasar props
<RecipeCard receta={miReceta} />

// Recibir props
function RecipeCard({ receta }) {
  return <h3>{receta.name}</h3>;
}
```

---

### 5. **Virtual DOM**

**Definición:** Copia en memoria del DOM. React compara versiones y actualiza solo lo que cambió.

**Ventaja:** Más rápido que borrar y recrear todo el DOM real.

---

## 💡 Aclaraciones importantes

### `.map()` en React

```jsx
recetas.map(receta => (
  <RecipeCard key={receta.id} receta={receta} />
))
```

- `receta` → parámetro del map (como el `i` de un for)
- `<RecipeCard />` → renderiza (ejecuta) el componente
- `key={receta.id}` → prop especial de React para identificar elementos en listas
- `receta={receta}` → prop que pasa datos al componente hijo

**Izquierda vs derecha:**
```jsx
receta={receta}
^^^^^^  ^^^^^^
  |       |
nombre   valor que
de la    le pasas
prop
```

---

## ✅ Lo esencial para recordar

1. **React actualiza automáticamente** la interfaz cuando cambian los datos
2. **Componentes = funciones que devuelven JSX**
3. **JSX = HTML dentro de JavaScript**
4. **Props = datos que se pasan entre componentes**
5. **`<Componente />` = renderizar/ejecutar ese componente**

---

## 🚀 Siguiente paso

**FASE 1.2 — Tu primer componente**

---

**Fecha:** Febrero 2025  
**Versión:** 1.0
