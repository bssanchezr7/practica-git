# Lección 02: `git add` — Preparar cambios para el commit

## 📖 Teoría

### ¿Qué hace `git add`?

`git add` mueve cambios desde el **Working Directory** al **Staging Area**.
Es como poner cosas en una caja antes de enviarlas por correo: decides
exactamente qué va dentro antes de cerrar y enviar (hacer commit).

```
  Working Directory          Staging Area            Repository
  ┌────────────────┐       ┌────────────────┐      ┌────────────────┐
  │                │       │                │      │                │
  │  menu.txt  ●───────▶  │  menu.txt ✓    │      │                │
  │                │ add   │                │      │                │
  │  inventario.txt│       │                │      │                │
  │                │       │                │      │                │
  └────────────────┘       └────────────────┘      └────────────────┘
                                    │
          Solo menu.txt está        │
          preparado. inventario.txt │
          se queda fuera del        │
          próximo commit.           │
```

### ¿Por qué existe el Staging Area?

Podrías pensar: "¿Por qué no guardar directamente todos los cambios?"

El Staging Area existe porque te da **control fino** sobre qué cambios
incluyes en cada commit. Imagina que modificaste 5 archivos pero solo 2
cambios están relacionados. Con `git add` seleccionas exactamente esos 2
y creas un commit limpio y enfocado.

**Analogía**: Estás preparando una maleta para un viaje. No metes todo lo que
hay en tu armario — eliges lo que necesitas para ESE viaje específico.

### Formas de usar `git add`

```bash
# Añadir UN archivo específico
git add archivo.txt

# Añadir VARIOS archivos específicos
git add archivo1.txt archivo2.txt archivo3.txt

# Añadir todos los archivos de una carpeta
git add carpeta/

# Añadir TODOS los cambios del proyecto
git add .

# Añadir todos los archivos con cierta extensión
git add *.txt

# Añadir partes específicas de un archivo (modo interactivo)
git add -p archivo.txt
```

### `git add .` vs `git add archivo.txt`

| Comando | Qué hace | Cuándo usarlo |
|---------|---------|---------------|
| `git add archivo.txt` | Prepara solo ese archivo | Cuando quieres commits precisos |
| `git add .` | Prepara TODO lo modificado/nuevo | Cuando todos los cambios van juntos |
| `git add *.txt` | Prepara archivos que coincidan con el patrón | Cambios en un tipo de archivo |
| `git add carpeta/` | Prepara todo dentro de esa carpeta | Cambios agrupados por carpeta |

### ⚠️ Importante: `git add` captura una instantánea

Cuando ejecutas `git add archivo.txt`, Git guarda el contenido del archivo
**en ese preciso momento**. Si modificas el archivo DESPUÉS del `git add`,
esos cambios nuevos NO estarán en el Staging Area. Tendrías que ejecutar
`git add` otra vez.

```
  1. Editas archivo.txt        → Working Directory cambia
  2. git add archivo.txt       → Staging Area tiene versión "A"
  3. Editas archivo.txt otra vez → Working Directory tiene versión "B"
  4. git commit                → Se guarda versión "A" (¡no la "B"!)
```

### Quitar archivos del Staging Area

Si preparaste algo por error:

```bash
# Quitar UN archivo del staging (vuelve al Working Directory)
git restore --staged archivo.txt

# Quitar TODO del staging
git restore --staged .
```

El archivo no se borra ni se pierde — solo vuelve al estado "modificado
pero no preparado".

---

## 💻 Práctica

### Ejercicio 1: Preparar archivos uno a uno

Primero, veamos qué archivos tenemos sin rastrear:

```bash
git status
```

Ahora, añade SOLO el archivo `README.md`:

```bash
git add README.md
git status
```

✅ **Resultado esperado**:
- `README.md` aparece en **verde** bajo "Changes to be committed".
- El resto de archivos siguen en **rojo** como "Untracked files".

💡 **Observa**: Has sido selectivo. Solo `README.md` irá en el próximo commit.

---

### Ejercicio 2: Preparar una carpeta completa

Añade toda la carpeta `lecciones/`:

```bash
git add lecciones/
git status
```

✅ **Resultado esperado**: Todos los archivos dentro de `lecciones/`
aparecen en verde. Los archivos de `proyecto/` siguen en rojo.

---

### Ejercicio 3: Preparar todo lo restante

Ahora añade todo lo que queda:

```bash
git add .
git status
```

✅ **Resultado esperado**: Todo aparece en verde bajo "Changes to be
committed". No queda nada en rojo.

💡 **`git add .`** usa el punto (`.`) que significa "el directorio actual
y todo lo que contiene".

---

### Ejercicio 4: Quitar algo del Staging Area

Imagina que no quieres incluir `proyecto/inventario.txt` en este commit.
Quítalo del staging:

```bash
git restore --staged proyecto/inventario.txt
git status
```

✅ **Resultado esperado**: `proyecto/inventario.txt` vuelve a aparecer en
rojo como "Untracked". El resto sigue en verde.

Ahora vuelve a añadirlo para dejarlo todo preparado:

```bash
git add proyecto/inventario.txt
git status
```

✅ **Resultado esperado**: Todo en verde de nuevo.

---

### Ejercicio 5: Entender que `git add` captura una instantánea

Este ejercicio demuestra un concepto crucial. Vamos a modificar un archivo
DESPUÉS de haberlo preparado:

```bash
echo "Línea añadida después del git add" >> proyecto/menu.txt
git status
```

✅ **Resultado esperado**: `proyecto/menu.txt` aparece **dos veces**:
- En verde: la versión que ya estaba preparada.
- En rojo: los cambios nuevos que acabas de hacer.

Esto confirma que el Staging Area tiene una **foto** del momento del
`git add`, no un enlace al archivo.

Prepara también los cambios nuevos:

```bash
git add proyecto/menu.txt
git status
```

✅ **Resultado esperado**: `proyecto/menu.txt` aparece solo una vez, en
verde. La nueva versión reemplazó a la anterior en el Staging Area.

---

### Ejercicio 6: Usa la salida corta para verificar

```bash
git status -s
```

✅ **Resultado esperado**: Todos los archivos muestran `A` (added) en la
primera columna, en verde:

```
A  README.md
A  lecciones/01_git_status.md
A  lecciones/02_git_add.md
...
A  proyecto/menu.txt
A  proyecto/inventario.txt
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git add archivo.txt` | Prepara un archivo específico |
| `git add .` | Prepara todos los cambios |
| `git add carpeta/` | Prepara toda una carpeta |
| `git add *.txt` | Prepara archivos por patrón |
| `git restore --staged archivo.txt` | Quita un archivo del staging |
| `git restore --staged .` | Quita todo del staging |

**Regla de oro**: `git add` es tu herramienta de **selección**. Úsala para
construir commits limpios que contengan cambios relacionados entre sí.

---

> **Siguiente lección**: `03_git_commit.md` — Es hora de guardar
> tu trabajo en el historial permanente.
