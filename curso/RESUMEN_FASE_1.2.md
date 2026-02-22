# 📝 Resumen FASE 1.2 — Tu primer componente

---

## 🎯 Estructura básica de un componente

```jsx
function NombreDelComponente(props) {
  // 1. Lógica (opcional)
  
  // 2. Return con JSX
  return (
    <div>
      {/* HTML aquí */}
    </div>
  );
}
```

---

## 🔑 Conceptos clave

### 1. **Props (propiedades)**

**Definición:** Datos que se pasan de un componente padre a un hijo.

```jsx
// Pasar props
<RecipeCard receta={miReceta} nombre="Carbonara" />

// Recibir props (destructuring)
function RecipeCard({ receta, nombre }) {
  return <h3>{nombre}</h3>;
}

// Recibir props (sin destructuring)
function RecipeCard(props) {
  return <h3>{props.nombre}</h3>;
}
```

---

### 2. **Renderizar componentes**

```jsx
// Componente individual
<RecipeCard receta={miReceta} />

// Múltiples componentes con .map()
{recetas.map(receta => (
  <RecipeCard key={receta.id} receta={receta} />
))}
```

---

### 3. **JavaScript en JSX**

```jsx
<div>
  {variable}                    {/* Variable */}
  {2 + 2}                       {/* Expresión */}
  {edad > 18 ? "Mayor" : "Menor"} {/* Ternario */}
  {activo && <span>✓</span>}    {/* Renderizado condicional */}
</div>
```

---

## ✅ Reglas importantes

### Nombres en PascalCase
```jsx
✅ function RecipeCard() {}
❌ function recipeCard() {}
```

### Un solo elemento raíz
```jsx
// ❌ MAL
return (
  <h3>Título</h3>
  <p>Texto</p>
);

// ✅ BIEN
return (
  <div>
    <h3>Título</h3>
    <p>Texto</p>
  </div>
);

// ✅ BIEN (Fragment)
return (
  <>
    <h3>Título</h3>
    <p>Texto</p>
  </>
);
```

### Atributos en camelCase
```jsx
✅ className="card"
❌ class="card"

✅ onClick={handleClick}
❌ onclick={handleClick}
```

### Props son inmutables
```jsx
function RecipeCard({ receta }) {
  // ❌ NUNCA
  receta.nombre = "Cambio";
  
  // ✅ Crear copia
  const nueva = { ...receta, nombre: "Cambio" };
}
```

---

## 🧩 Anatomía completa

```jsx
function RecipeCard({ receta }) {
  // Lógica antes del return
  const esMuyRapida = receta.tiempo < 15;
  
  return (
    <div className="recipe-card">
      <h3>{receta.nombre}</h3>
      <p>{receta.descripcion}</p>
      <span>⏱️ {receta.tiempo} min</span>
      {esMuyRapida && <span>Rápida</span>}
    </div>
  );
}
```

---

## 💡 Ejemplo completo

```jsx
// Componente hijo
function RecipeCard({ receta }) {
  return (
    <div className="recipe-card">
      <h3>{receta.nombre}</h3>
      <p>{receta.descripcion}</p>
      <span>⏱️ {receta.tiempo} min</span>
    </div>
  );
}

// Componente padre
function App() {
  const recetas = [
    { id: 1, nombre: "Carbonara", descripcion: "Italiana", tiempo: 20 },
    { id: 2, nombre: "Ensalada", descripcion: "Fresca", tiempo: 10 }
  ];

  return (
    <div>
      {recetas.map(receta => (
        <RecipeCard key={receta.id} receta={receta} />
      ))}
    </div>
  );
}
```

---

## ✅ Lo esencial para recordar

1. **Componente = función que devuelve JSX**
2. **Props = datos que se pasan entre componentes** (son inmutables)
3. **JavaScript en JSX = entre llaves `{}`**
4. **`.map()` para renderizar listas** (no olvides `key`)
5. **Un componente = un elemento raíz**

---

## 🚀 Siguiente paso

**FASE 1.3 — Estado y eventos (`useState`)**

---

**Fecha:** Febrero 2025  
**Versión:** 1.0
