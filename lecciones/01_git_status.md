# Lección 01: `git status` — Inspeccionar el estado del proyecto

## 📖 Teoría

### ¿Qué hace `git status`?

`git status` es tu brújula en Git. Te dice **exactamente** en qué estado se
encuentra cada archivo de tu proyecto. Es el comando que más vas a usar — antes
de añadir, antes de hacer commit, antes de casi cualquier cosa.

### Los tres espacios de Git

Para entender `git status`, primero necesitas conocer los tres espacios donde
viven tus archivos:

```
┌─────────────────────────────────────────────────────────────┐
│                    TU PROYECTO EN GIT                        │
│                                                             │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │  WORKING     │    │  STAGING     │    │  REPOSITORY  │  │
│  │  DIRECTORY   │───▶│  AREA        │───▶│  (.git)      │  │
│  │              │    │  (Index)     │    │              │  │
│  │ Donde editas │    │ Lo que vas   │    │ El historial │  │
│  │ tus archivos │    │ a guardar    │    │ permanente   │  │
│  └──────────────┘    └──────────────┘    └──────────────┘  │
│                                                             │
│       git add ──────▶          git commit ──────▶           │
└─────────────────────────────────────────────────────────────┘
```

1. **Working Directory** (Directorio de trabajo): La carpeta real en tu disco.
   Aquí creas, editas y borras archivos normalmente.

2. **Staging Area** (Área de preparación): Una zona intermedia donde colocas
   los cambios que quieres incluir en tu próximo commit. Piensa en ella como
   una "bandeja de salida".

3. **Repository** (Repositorio): La base de datos de Git (dentro de `.git/`)
   que guarda todo el historial de tu proyecto.

### Los estados de un archivo

`git status` clasifica tus archivos en estas categorías:

| Estado | Significado | Color en terminal |
|--------|------------|-------------------|
| **Untracked** (sin seguimiento) | Git no conoce este archivo | Rojo |
| **Modified** (modificado) | Archivo conocido por Git, con cambios sin preparar | Rojo |
| **Staged** (preparado) | Cambios listos para el próximo commit | Verde |
| **Unmodified** (sin cambios) | No aparece en `git status` (todo limpio) | — |

### El ciclo de vida de un archivo

```
  Sin seguimiento    Sin modificar     Modificado      Preparado
  (Untracked)        (Unmodified)      (Modified)      (Staged)
       │                  │                 │               │
       │   git add        │    Editar       │   git add     │
       │─────────────────────────────────────────────────▶  │
       │                  │────────────▶    │──────────▶    │
       │                  │                 │               │
       │                  │                 │  git commit   │
       │                  │◀────────────────────────────────│
       │                  │                 │               │
```

### Sintaxis

```bash
git status           # Salida completa
git status -s        # Salida corta (resumida)
```

La salida corta (`-s`) usa dos columnas de letras:
- **Primera columna**: estado en el Staging Area
- **Segunda columna**: estado en el Working Directory

```
 M archivo.txt     ← Modificado en Working Directory (no preparado)
M  archivo.txt     ← Modificado y preparado (staged)
MM archivo.txt     ← Preparado Y con cambios adicionales sin preparar
?? archivo.txt     ← Sin seguimiento (untracked)
A  archivo.txt     ← Archivo nuevo, añadido al staging
```

### ⚠️ Importante

`git status` **nunca modifica nada**. Es 100% seguro ejecutarlo en cualquier
momento. Si tienes duda de en qué estado está tu proyecto, ejecútalo.
Es como mirar un mapa: no te mueve, solo te muestra dónde estás.

---

## 💻 Práctica

### Ejercicio 1: Tu primer `git status`

Desde la terminal, en la carpeta `~/practica_git`, ejecuta:

```bash
git status
```

✅ **Resultado esperado**: Verás algo como esto:

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
        lecciones/
        proyecto/

nothing added to commit but untracked files present (use "git add" to track)
```

💡 **Interpreta el resultado**: Git te dice que existen archivos pero que
no los está rastreando todavía. Son **untracked**.

---

### Ejercicio 2: Observa cómo cambia el estado

Vamos a crear un archivo nuevo y observar qué pasa:

```bash
echo "Hola, soy un archivo de prueba" > prueba_temporal.txt
git status
```

✅ **Resultado esperado**: `prueba_temporal.txt` aparece en rojo bajo
"Untracked files".

Ahora prepáralo (lo aprenderás a fondo en la lección 02):

```bash
git add prueba_temporal.txt
git status
```

✅ **Resultado esperado**: `prueba_temporal.txt` aparece en verde bajo
"Changes to be committed" con la etiqueta `new file`.

Ahora modifica el archivo que ya preparaste:

```bash
echo "Esta línea es nueva" >> prueba_temporal.txt
git status
```

✅ **Resultado esperado**: El archivo aparece **dos veces**:
- En verde: la versión que preparaste.
- En rojo: los cambios nuevos que aún no has preparado.

💡 **Concepto clave**: El Staging Area guarda una *foto* del archivo en el
momento en que hiciste `git add`. Si luego lo modificas, esos cambios nuevos
están solo en el Working Directory.

---

### Ejercicio 3: Prueba la salida corta

```bash
git status -s
```

✅ **Resultado esperado**: Verás algo como:

```
AM prueba_temporal.txt
?? README.md
?? lecciones/
?? proyecto/
```

`AM` significa: **A**ñadido al staging + **M**odificado en el working directory.

---

### Ejercicio 4: Limpieza

Vamos a dejar todo limpio para las siguientes lecciones. Quita el archivo
del staging y elimínalo:

```bash
git rm --cached prueba_temporal.txt
rm prueba_temporal.txt
git status
```

✅ **Resultado esperado**: `prueba_temporal.txt` ya no aparece. El estado
vuelve a mostrar solo los archivos originales como untracked.

---

## 🧠 Resumen

| Concepto | Detalle |
|----------|---------|
| `git status` | Muestra el estado actual de todos los archivos |
| `git status -s` | Versión compacta del estado |
| Working Directory | Donde editas archivos (tu carpeta real) |
| Staging Area | Zona de preparación para el próximo commit |
| Repository | Historial permanente de commits |
| Untracked | Archivo que Git aún no rastrea |
| Modified | Archivo rastreado con cambios sin preparar |
| Staged | Cambios listos para el próximo commit |

---

> **Siguiente lección**: `lecciones/02_git_add.md` — Aprenderás a preparar
> cambios con precisión quirúrgica.
