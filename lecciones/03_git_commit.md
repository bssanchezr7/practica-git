# Lección 03: `git commit` — Guardar instantáneas en el historial

## 📖 Teoría

### ¿Qué es un commit?

Un commit es una **instantánea** (snapshot) de tu proyecto en un momento dado.
No es un diff ni un parche: es una foto completa de todos los archivos
preparados en el Staging Area.

Piensa en los commits como los puntos de guardado de un videojuego. Cada uno
captura el estado exacto del juego en ese momento, y puedes volver a cualquiera
de ellos.

```
  Commit 1          Commit 2          Commit 3
  ┌──────────┐     ┌──────────┐     ┌──────────┐
  │ Foto del │     │ Foto del │     │ Foto del │
  │ proyecto │────▶│ proyecto │────▶│ proyecto │
  │ 10:00 AM │     │ 11:30 AM │     │ 02:15 PM │
  └──────────┘     └──────────┘     └──────────┘

  Cada commit apunta al anterior, formando una cadena (historial).
```

### Anatomía de un commit

Cada commit contiene:

```
┌─────────────────────────────────────────┐
│              COMMIT                      │
│                                         │
│  Hash:    a1b2c3d (identificador único) │
│  Autor:   Tu Nombre <tu@email.com>      │
│  Fecha:   2026-02-20 10:30:00           │
│  Mensaje: "Agrega menú del restaurante" │
│                                         │
│  Contenido:                             │
│    - README.md (versión en ese momento) │
│    - proyecto/menu.txt (ídem)           │
│    - proyecto/inventario.txt (ídem)     │
│                                         │
│  Padre:   f4e5d6a (commit anterior)     │
└─────────────────────────────────────────┘
```

- **Hash SHA-1**: Un identificador único de 40 caracteres (normalmente se
  muestran solo los primeros 7). Git lo genera automáticamente.
- **Autor y fecha**: Quién y cuándo.
- **Mensaje**: Una descripción legible de qué cambió y por qué.
- **Padre**: El commit anterior (así se forma la cadena del historial).

### Cómo usar `git commit`

```bash
# Commit con mensaje en línea (lo más común)
git commit -m "Descripción del cambio"

# Commit abriendo tu editor de texto para escribir el mensaje
git commit

# Commit preparando y confirmando archivos rastreados de una vez
git commit -a -m "Mensaje"
# (¡Ojo! -a solo prepara archivos que Git ya rastrea, no archivos nuevos)
```

### El arte de escribir buenos mensajes de commit

Los mensajes de commit son **documentación viva** de tu proyecto. Un buen
historial de commits te permite entender qué pasó, cuándo y por qué.

#### Estructura recomendada

```
<tipo>: <descripción corta en imperativo> (máx ~50 caracteres)

<cuerpo opcional: explica el "por qué", no el "qué"> (máx 72 char/línea)
```

#### Buenos mensajes vs malos mensajes

```
❌ MALOS                          ✅ BUENOS
─────────────────────────────     ─────────────────────────────
"cambios"                         "Agrega menú inicial del restaurante"
"fix"                             "Corrige precio incorrecto de la pasta"
"asdfg"                           "Elimina platos descontinuados del menú"
"WIP"                             "Actualiza inventario con productos de temporada"
"update"                          "Reorganiza categorías del menú por tipo"
```

#### Reglas prácticas

1. **Usa el imperativo**: "Agrega...", "Corrige...", "Elimina...", no
   "Agregado...", "Corrigiendo...", "Se eliminó..."
2. **Sé específico**: El mensaje debe responder: "¿Qué hace este commit?"
3. **Mantén la primera línea corta**: Máximo ~50 caracteres.
4. **Si necesitas explicar más**, deja una línea en blanco y añade un cuerpo.

### ⚠️ Importante

- Solo se incluye en el commit lo que está en el **Staging Area** (lo verde
  en `git status`).
- Si no hay nada en el staging, `git commit` no hace nada (y te lo dice).
- Cada commit es **inmutable**: una vez creado, no cambia. Puedes crear
  commits nuevos que reviertan cambios, pero el original siempre existirá
  en el historial.

### La opción `-a` (atajo con precaución)

```bash
git commit -a -m "Mensaje"
```

El flag `-a` ejecuta automáticamente `git add` para todos los archivos que
Git **ya conoce** (rastreados). Pero tiene una limitación importante:

- **SÍ** prepara archivos modificados que ya están rastreados.
- **NO** prepara archivos nuevos (untracked).

💡 Es útil para cambios rápidos, pero para commits precisos es mejor hacer
`git add` manualmente primero.

---

## 💻 Práctica

> **Prerrequisito**: Si seguiste la lección 02, ya tienes todos los archivos
> en el Staging Area. Si no, ejecuta `git add .` para prepararlos.

### Ejercicio 1: Tu primer commit

Verifica que hay algo preparado:

```bash
git status
```

Si todo está en verde, crea tu primer commit:

```bash
git commit -m "Inicializa el curso con estructura y archivos de práctica"
```

✅ **Resultado esperado**:

```
[master (root-commit) a1b2c3d] Inicializa el curso con estructura y archivos de práctica
 X files changed, Y insertions(+)
 create mode 100644 README.md
 create mode 100644 lecciones/01_git_status.md
 ...
```

Verifica que todo está limpio:

```bash
git status
```

✅ **Resultado esperado**: "nothing to commit, working tree clean"

💡 ¡Esa es la frase más satisfactoria de Git!

---

### Ejercicio 2: Modificar, preparar y commitear

Vamos a simular un día de trabajo real. Edita el archivo del menú:

```bash
echo "" >> proyecto/menu.txt
echo "## Platos nuevos" >> proyecto/menu.txt
echo "- Risotto de setas — 14.50€" >> proyecto/menu.txt
echo "- Tarta de queso — 7.00€" >> proyecto/menu.txt
```

Verifica el estado, prepara y confirma:

```bash
git status
git add proyecto/menu.txt
git commit -m "Agrega platos nuevos al menú"
```

✅ **Resultado esperado**: Un nuevo commit se crea solo con los cambios
de `menu.txt`.

---

### Ejercicio 3: Múltiples archivos en un commit

Ahora modifica dos archivos y confirma ambos juntos:

```bash
echo "" >> proyecto/menu.txt
echo "- Ensalada César — 9.50€" >> proyecto/menu.txt

echo "" >> proyecto/inventario.txt
echo "## Actualización" >> proyecto/inventario.txt
echo "- Lechuga romana: 20 unidades" >> proyecto/inventario.txt
echo "- Queso parmesano: 5 kg" >> proyecto/inventario.txt
```

Prepara ambos y confirma:

```bash
git add proyecto/menu.txt proyecto/inventario.txt
git commit -m "Agrega ensalada César al menú y actualiza inventario"
```

✅ **Resultado esperado**: Un commit que incluye cambios en ambos archivos.

💡 **Buena práctica**: Este commit agrupa cambios que están relacionados
entre sí (un nuevo plato y los ingredientes que necesita).

---

### Ejercicio 4: Usa `-a` para un commit rápido

Modifica un archivo ya rastreado:

```bash
echo "- Agua mineral: 50 botellas" >> proyecto/inventario.txt
```

Usa el atajo `-a`:

```bash
git commit -a -m "Añade agua mineral al inventario"
```

✅ **Resultado esperado**: El commit se crea sin necesidad de hacer `git add`
primero, porque `inventario.txt` ya era un archivo rastreado.

---

### Ejercicio 5: Intenta hacer commit sin nada preparado

```bash
git status
git commit -m "Este commit no debería funcionar"
```

✅ **Resultado esperado**: Git te dice que no hay nada que commitear:

```
nothing to commit, working tree clean
```

💡 Git protege contra commits vacíos accidentales.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git commit -m "msg"` | Crea un commit con el contenido del staging |
| `git commit` | Abre el editor para escribir el mensaje |
| `git commit -a -m "msg"` | Prepara archivos rastreados y commitea |

**Regla de oro**: Cada commit debe ser una **unidad lógica de cambio**.
Pregúntate: "Si alguien lee mi historial, ¿entenderá qué hice y por qué?"

---

> **Siguiente lección**: `lecciones/04_git_log.md` — Aprenderás a explorar
> el historial que acabas de crear.
