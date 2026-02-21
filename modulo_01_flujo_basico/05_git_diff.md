# Lección 05: `git diff` — Comparar versiones y ver cambios exactos

## 📖 Teoría

### ¿Qué hace `git diff`?

`git diff` te muestra las **diferencias exactas** entre dos estados de tus
archivos. Línea por línea, te dice qué se añadió, qué se eliminó y qué se
modificó.

Si `git status` es el **mapa** (te dice dónde estás), `git diff` es la
**lupa** (te muestra el detalle de qué cambió).

### Los tres tipos de comparación principales

```
  Working Directory        Staging Area          Último Commit
  ┌───────────────┐      ┌───────────────┐     ┌───────────────┐
  │ Tus cambios   │      │ Lo preparado  │     │ Lo guardado   │
  │ actuales      │      │ con git add   │     │ en el historial│
  └───────┬───────┘      └───────┬───────┘     └───────┬───────┘
          │                      │                      │
          │◀── git diff ────────▶│                      │
          │    (sin argumentos)  │                      │
          │                      │                      │
          │                      │◀── git diff ────────▶│
          │                      │    --staged          │
          │                      │    (o --cached)      │
          │                      │                      │
          │◀── git diff HEAD ──────────────────────────▶│
          │    (working dir vs último commit)           │
```

1. **`git diff`** (sin argumentos): Compara el Working Directory contra el
   Staging Area. Te muestra cambios que **aún no has preparado**.

2. **`git diff --staged`**: Compara el Staging Area contra el último commit.
   Te muestra exactamente lo que se incluirá en el **próximo commit**.

3. **`git diff HEAD`**: Compara el Working Directory contra el último commit.
   Te muestra **todos los cambios** desde el último commit (preparados o no).

### Cómo leer la salida de `git diff`

La salida usa el formato **unified diff**. Veamos un ejemplo:

```diff
diff --git a/proyecto/menu.txt b/proyecto/menu.txt
index 3a4b5c6..7d8e9f0 100644
--- a/proyecto/menu.txt
+++ b/proyecto/menu.txt
@@ -5,6 +5,8 @@ Restaurante "El Buen Código"
 - Spaghetti carbonara — 12.00€
 - Pizza margarita — 10.50€
 - Ensalada mixta — 8.00€
+- Risotto de setas — 14.50€
+- Tarta de queso — 7.00€

 ## Bebidas
 - Agua mineral — 2.50€
```

Desglosemos cada parte:

| Línea | Significado |
|-------|------------|
| `diff --git a/... b/...` | Qué archivo se compara |
| `--- a/proyecto/menu.txt` | Versión anterior (a) |
| `+++ b/proyecto/menu.txt` | Versión nueva (b) |
| `@@ -5,6 +5,8 @@` | Ubicación: desde línea 5, contexto de 6→8 líneas |
| Líneas sin prefijo | Contexto (no cambiaron) |
| `+` líneas en verde | Líneas **añadidas** |
| `-` líneas en rojo | Líneas **eliminadas** |

### ⚠️ Importante: "Modificar" = eliminar + añadir

Git no entiende "modificaciones" directamente. Si cambias una palabra en una
línea, Git lo muestra como: se eliminó la línea vieja (`-`) y se añadió la
nueva (`+`).

```diff
-- Pizza margarita — 10.50€
+- Pizza margarita — 11.00€
```

Esto no significa que se borró el plato — solo cambió el precio.

### Comparar commits entre sí

```bash
# Comparar dos commits específicos (usando sus hashes)
git diff abc1234 def5678

# Comparar un commit con el actual
git diff abc1234 HEAD

# Comparar los últimos 2 commits
git diff HEAD~1 HEAD
```

`HEAD~1` significa "el commit anterior a HEAD". `HEAD~2` es dos commits atrás,
y así sucesivamente.

### Comparar un archivo específico

Puedes añadir el nombre del archivo al final de cualquier `git diff`:

```bash
git diff proyecto/menu.txt              # Cambios sin preparar de ese archivo
git diff --staged proyecto/menu.txt     # Cambios preparados de ese archivo
git diff HEAD~1 HEAD proyecto/menu.txt  # Cambios de ese archivo entre commits
```

### Ver solo los nombres de archivos cambiados

```bash
git diff --name-only              # Solo nombres de archivos modificados
git diff --name-status            # Nombres + tipo de cambio (M/A/D)
```

La columna de tipo:
- `M` = Modified (modificado)
- `A` = Added (añadido)
- `D` = Deleted (eliminado)
- `R` = Renamed (renombrado)

### Ver estadísticas resumidas

```bash
git diff --stat
```

Muestra un resumen compacto con barras de progreso:

```
 proyecto/menu.txt       | 3 +++
 proyecto/inventario.txt | 2 +-
 2 files changed, 4 insertions(+), 1 deletion(-)
```

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado los ejercicios de la lección 03
> (tener commits en el historial).

### Ejercicio 1: Diferencias en el Working Directory

Primero, verifica que todo está limpio:

```bash
git status
```

Ahora haz cambios en `proyecto/menu.txt`:

```bash
echo "" >> proyecto/menu.txt
echo "## Postres de temporada" >> proyecto/menu.txt
echo "- Crema catalana — 6.50€" >> proyecto/menu.txt
echo "- Flan casero — 5.00€" >> proyecto/menu.txt
```

Compara lo que cambió:

```bash
git diff
```

✅ **Resultado esperado**: Ves las líneas añadidas marcadas con `+` en verde.
Estas son las diferencias entre tu Working Directory y el Staging Area.

---

### Ejercicio 2: Diferencias después de `git add`

Prepara los cambios:

```bash
git add proyecto/menu.txt
```

Ahora intenta:

```bash
git diff
```

✅ **Resultado esperado**: ¡No aparece nada! Porque `git diff` (sin opciones)
compara Working Directory vs Staging Area, y ambos son iguales ahora.

Ahora usa `--staged`:

```bash
git diff --staged
```

✅ **Resultado esperado**: Ahora SÍ ves los cambios — son los que están
preparados para el próximo commit. Estás comparando el Staging Area contra
el último commit.

💡 **Regla mental**:
- `git diff` = "¿Qué he cambiado que NO he preparado?"
- `git diff --staged` = "¿Qué he preparado que NO he commiteado?"

---

### Ejercicio 3: Ver todos los cambios con `HEAD`

Haz un cambio adicional SIN prepararlo:

```bash
echo "- Helado artesanal — 4.50€" >> proyecto/menu.txt
```

Ahora tienes cambios preparados Y cambios sin preparar. Compara:

```bash
echo "=== Cambios SIN preparar ==="
git diff

echo ""
echo "=== Cambios PREPARADOS ==="
git diff --staged

echo ""
echo "=== TODOS los cambios desde el último commit ==="
git diff HEAD
```

✅ **Resultado esperado**:
- `git diff` muestra solo la línea del helado (sin preparar).
- `git diff --staged` muestra los postres de temporada (preparados).
- `git diff HEAD` muestra TODO: postres + helado.

---

### Ejercicio 4: Confirma todo y compara commits

Prepara y confirma todo:

```bash
git add .
git commit -m "Agrega sección de postres al menú"
```

Ahora compara el último commit con el anterior:

```bash
git diff HEAD~1 HEAD
```

✅ **Resultado esperado**: Ves exactamente lo que cambió entre el penúltimo
y el último commit.

---

### Ejercicio 5: Ver solo nombres de archivos

```bash
git diff --name-only HEAD~1 HEAD
```

✅ **Resultado esperado**: Solo muestra `proyecto/menu.txt` (el único archivo
que cambió en el último commit).

Prueba con más contexto:

```bash
git diff --name-status HEAD~3 HEAD
```

✅ **Resultado esperado**: Muestra los archivos que cambiaron en los
últimos 3 commits con su tipo de cambio (M = Modified, A = Added).

---

### Ejercicio 6: Estadísticas resumidas

```bash
git diff --stat HEAD~3 HEAD
```

✅ **Resultado esperado**: Un resumen compacto con barras que muestran
la proporción de líneas añadidas vs eliminadas por archivo.

---

### Ejercicio 7: Comparar un archivo específico entre commits

```bash
git diff HEAD~2 HEAD -- proyecto/menu.txt
```

✅ **Resultado esperado**: Solo ves los cambios de `menu.txt` entre
esos dos commits, ignorando otros archivos.

💡 El `--` separa las opciones de git diff de los nombres de archivo.
No siempre es obligatorio, pero es buena práctica para evitar ambigüedades.

---

## 🧠 Resumen

| Comando | Compara |
|---------|---------|
| `git diff` | Working Directory ↔ Staging Area |
| `git diff --staged` | Staging Area ↔ Último commit |
| `git diff HEAD` | Working Directory ↔ Último commit |
| `git diff abc def` | Commit abc ↔ Commit def |
| `git diff HEAD~1 HEAD` | Penúltimo commit ↔ Último commit |
| `git diff --name-only` | Solo nombres de archivos |
| `git diff --stat` | Resumen estadístico |
| `git diff -- archivo` | Diferencias de un archivo específico |

**Regla de oro**: Usa `git diff --staged` **siempre antes de hacer commit**
para verificar que vas a guardar exactamente lo que quieres.

---

> **Siguiente lección**: `06_ejercicio_final.md` — Un ejercicio
> integrador que combina todo lo aprendido.
