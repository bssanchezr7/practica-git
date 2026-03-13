# Lección 01: `git restore` — Descartar cambios

## 📖 Teoría

### ¿Qué hace `git restore`?

`git restore` deshace cambios en tus archivos. Según [Pro Git (Capítulo 2.4)](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things#_unstaging_a_staged_file), este comando fue introducido en Git 2.23 para separar las responsabilidades de deshacer cambios de las de cambiar de ramas. Tiene dos modos principales:

1. **Descartar cambios del Working Directory** (volver al estado del último
   commit o del staging).
2. **Quitar archivos del Staging Area** (sin borrar los cambios del disco).

```
  Repository       Staging Area       Working Directory
  ┌──────────┐    ┌──────────┐       ┌──────────┐
  │ Último   │    │ Lo       │       │ Tus      │
  │ commit   │    │ preparado│       │ cambios  │
  └────┬─────┘    └────┬─────┘       └────┬─────┘
       │               │                  │
       │  restore      │  restore         │
       │  --staged     │  (sin flags)     │
       │  ◀────────────│  ◀───────────────│
       │               │                  │
       │  Quita del    │  Descarta        │
       │  staging      │  cambios del     │
       │               │  working dir     │
```

### `git restore` vs el viejo `git checkout`

Antes de Git 2.23, se usaba `git checkout` para todo. Ahora:

| Tarea | Comando moderno | Comando viejo |
|-------|----------------|---------------|
| Descartar cambios en un archivo | `git restore archivo` | `git checkout -- archivo` |
| Quitar del staging | `git restore --staged archivo` | `git reset HEAD archivo` |
| Cambiar de rama | `git switch rama` | `git checkout rama` |

`git restore` es más seguro e intuitivo.

### Modo 1: Descartar cambios del Working Directory

```bash
git restore archivo.txt
```

Esto **sobrescribe** el archivo con la versión del Staging Area (o del
último commit si no hay nada en staging). Los cambios que hiciste en
el archivo **se pierden permanentemente**.

⚠️ **No hay vuelta atrás.** Si no hiciste commit ni stash, los cambios
se pierden para siempre.

### Modo 2: Quitar del Staging Area

```bash
git restore --staged archivo.txt
```

Esto quita el archivo del Staging Area pero **NO modifica el archivo en
disco**. Los cambios siguen en tu Working Directory — simplemente ya no
están preparados para el commit.

### Restaurar desde un commit específico

```bash
git restore --source=HEAD~2 archivo.txt
```

Esto reemplaza el archivo en tu Working Directory con la versión de 2
commits atrás. Útil para recuperar versiones antiguas de un archivo.

### Resumen visual

```
  Acción                          Comando                    ¿Pierdo cambios?
  ─────────────────────────────── ────────────────────────── ────────────────
  Descartar cambios del disco     git restore archivo        SÍ (irreversible)
  Quitar del staging              git restore --staged arch  NO (siguen en disco)
  Restaurar versión antigua       git restore --source=hash  Sobrescribe disco
  Descartar TODO del disco        git restore .              SÍ (irreversible)
  Quitar TODO del staging         git restore --staged .     NO
```

### ⚠️ Importante

- `git restore` sin `--staged` **destruye cambios**. Úsalo con cuidado.
- Siempre haz `git status` y `git diff` antes de restaurar para
  asegurarte de qué estás descartando.
- Si no estás seguro, primero haz `git stash` para guardar los cambios
  temporalmente (lo verás en el Módulo 7).

---

## 💻 Práctica

### Ejercicio 1: Descartar cambios del Working Directory

Haz un cambio en un archivo:

```bash
cd ~/practica_git
echo "ESTE CAMBIO ES UN ERROR" >> proyecto/menu.txt
git status
git diff proyecto/menu.txt
```

✅ Ves el cambio en rojo. Ahora descártalo:

```bash
git restore proyecto/menu.txt
git status
cat proyecto/menu.txt
```

✅ El archivo volvió a su estado anterior. "ESTE CAMBIO ES UN ERROR"
desapareció.

---

### Ejercicio 2: Quitar del Staging Area

Haz un cambio y prepáralo:

```bash
echo "Cambio que no quiero commitear todavía" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git status
```

✅ El archivo está en verde (staged). Ahora quítalo del staging:

```bash
git restore --staged proyecto/inventario.txt
git status
```

✅ El archivo vuelve a rojo (modified pero no staged). El cambio sigue
en el archivo — solo lo sacaste de la "bandeja de salida".

Ahora descarta también el cambio del disco:

```bash
git restore proyecto/inventario.txt
git status
```

✅ Todo limpio.

---

### Ejercicio 3: Restaurar un archivo desde un commit antiguo

Veamos cómo era `menu.txt` hace unos commits:

```bash
git log --oneline -5
```

Restaura la versión de 3 commits atrás:

```bash
git restore --source=HEAD~3 proyecto/menu.txt
git diff proyecto/menu.txt
```

✅ Ves las diferencias entre la versión antigua y la actual.

Vuelve a la versión actual:

```bash
git restore proyecto/menu.txt
```

✅ Todo restaurado.

---

### Ejercicio 4: Descartar todos los cambios de golpe

Haz varios cambios:

```bash
echo "Error 1" >> proyecto/menu.txt
echo "Error 2" >> proyecto/inventario.txt
git status
```

✅ Dos archivos modificados. Descarta todo:

```bash
git restore .
git status
```

✅ Todo limpio en un solo comando.

💡 `git restore .` usa el punto (directorio actual) para restaurar
todos los archivos modificados.

---

### Ejercicio 5: La diferencia crítica entre los dos modos

```bash
echo "Dato importante" >> proyecto/menu.txt
git add proyecto/menu.txt
git status
```

Opción A — `git restore --staged` (SEGURO):

```bash
git restore --staged proyecto/menu.txt
git status
cat proyecto/menu.txt
```

✅ El cambio sigue en el archivo, solo se quitó del staging.

Opción B — `git restore` (DESTRUCTIVO):

```bash
git restore proyecto/menu.txt
cat proyecto/menu.txt
```

✅ El cambio desapareció del archivo por completo.

💡 **Recuerda**: `--staged` solo mueve, sin `--staged` destruye.

---

## 🧠 Resumen

| Comando | Efecto | ¿Reversible? |
|---------|--------|--------------|
| `git restore archivo` | Descarta cambios del disco | ❌ No |
| `git restore --staged archivo` | Quita del staging | ✅ Sí |
| `git restore .` | Descarta todos los cambios | ❌ No |
| `git restore --staged .` | Quita todo del staging | ✅ Sí |
| `git restore --source=hash archivo` | Restaura versión antigua | Sobrescribe |

---

> **Siguiente lección**: `02_git_revert.md` — Aprenderás a deshacer
> commits de forma segura sin alterar el historial.
