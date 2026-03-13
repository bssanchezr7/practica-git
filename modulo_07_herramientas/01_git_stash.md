# Lección 01: `git stash` — Guardar cambios temporalmente

## 📖 Teoría

### ¿Qué hace `git stash`?

`git stash` guarda tus cambios sin commitear en un **almacén temporal** y deja tu Working Directory limpio. Según [Pro Git (Capítulo 7.3)](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning), este comando toma el estado "sucio" de tu directorio de trabajo y lo guarda en una pila de cambios inacabados que puedes volver a aplicar en cualquier momento.

```
  ANTES de stash:                    DESPUÉS de stash:
  Working Directory tiene cambios    Working Directory limpio
  ┌──────────────────┐              ┌──────────────────┐
  │ menu.txt (mod.)  │   stash     │ menu.txt (limpio)│
  │ inv.txt (mod.)   │──────────▶  │ inv.txt (limpio) │
  └──────────────────┘              └──────────────────┘
         │                                    │
         ▼                                    │
  ┌──────────────────┐              Puedes cambiar de rama,
  │ Stash (cajón)    │              hacer pull, etc.
  │ guardado         │              
  │ temporalmente    │              Cuando termines:
  └──────────────────┘              git stash pop → recuperas todo
```

### ¿Cuándo usar stash?

- Estás a mitad de un cambio y necesitas **cambiar de rama urgentemente**.
- Quieres hacer `git pull` pero tienes cambios sin commitear.
- Quieres probar algo limpio sin perder tu trabajo en progreso.
- No quieres hacer un commit "WIP" (trabajo en progreso) sucio.

### Comandos principales

```bash
# Guardar cambios en el stash
git stash
git stash push -m "Descripción de qué estaba haciendo"

# Listar stashes guardados
git stash list

# Recuperar el último stash (y eliminarlo del stash)
git stash pop

# Recuperar sin eliminar del stash (por si algo sale mal)
git stash apply

# Ver qué hay en un stash específico
git stash show            # Resumen
git stash show -p         # Diff completo

# Eliminar un stash sin aplicarlo
git stash drop            # Elimina el más reciente
git stash drop stash@{2}  # Elimina uno específico

# Eliminar todos los stashes
git stash clear

# Guardar incluyendo archivos sin rastrear (untracked)
git stash -u
```

### La pila de stashes

Los stashes se guardan en una **pila** (stack). El más reciente es
`stash@{0}`, el anterior `stash@{1}`, etc.

```
  git stash list:

  stash@{0}: WIP on main: Cambio más reciente
  stash@{1}: WIP on main: Cambio anterior
  stash@{2}: On feature/x: Primer stash

  git stash pop  → aplica y borra stash@{0}
```

### `pop` vs `apply`

| Comando | Recupera cambios | Borra del stash |
|---------|-----------------|-----------------|
| `git stash pop` | ✅ | ✅ (si no hay conflictos) |
| `git stash apply` | ✅ | ❌ (lo mantiene) |

💡 Usa `apply` si no estás seguro. Puedes aplicar el mismo stash
varias veces y borrarlo manualmente después.

### ⚠️ Importante

- `git stash` solo guarda archivos **rastreados** y modificados. Para
  incluir archivos nuevos (untracked), usa `git stash -u`.
- Si hay conflictos al hacer `pop` o `apply`, resuélvelos como un merge.
- Los stashes son **locales** — no se pushean ni se comparten.

---

## 💻 Práctica

### Ejercicio 1: Tu primer stash

Haz cambios sin commitear:

```bash
cd ~/practica_git
echo "Trabajo en progreso..." >> proyecto/menu.txt
echo "Datos pendientes..." >> proyecto/inventario.txt
git status
```

Guárdalos en el stash:

```bash
git stash push -m "Trabajo en progreso: actualizando menú e inventario"
git status
```

✅ Working Directory limpio. Los cambios están guardados.

```bash
git stash list
```

✅ Ves tu stash en la lista.

---

### Ejercicio 2: Recupera el stash

```bash
git stash pop
git status
```

✅ Los cambios volvieron al Working Directory.

Limpia:

```bash
git restore .
```

---

### Ejercicio 3: Stash para cambiar de rama

Simula que estás trabajando y necesitas cambiar de rama urgentemente:

```bash
echo "Plato nuevo a medio definir..." >> proyecto/menu.txt
git status

# Necesitas cambiar de rama urgentemente
git stash push -m "Plato nuevo a medio hacer"
git switch -c hotfix/emergencia
echo "Corrección urgente aplicada" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Aplica corrección urgente"
git switch main
git merge hotfix/emergencia
git branch -d hotfix/emergencia

# Ahora recuperas tu trabajo en progreso
git stash pop
git status
```

✅ El cambio del plato nuevo está de vuelta, y la corrección urgente
también se aplicó.

```bash
git restore .
```

---

### Ejercicio 4: Múltiples stashes

```bash
echo "Stash 1" >> proyecto/menu.txt
git stash push -m "Primer trabajo guardado"

echo "Stash 2" >> proyecto/inventario.txt
git stash push -m "Segundo trabajo guardado"

echo "Stash 3" >> proyecto/menu.txt
git stash push -m "Tercer trabajo guardado"

git stash list
```

✅ Ves tres stashes. Aplica uno específico:

```bash
git stash apply stash@{1}
git status
```

✅ Aplicaste el segundo stash (índice 1) sin borrarlo.

Limpia:

```bash
git restore .
git stash clear
git stash list
```

✅ Todo limpio, todos los stashes borrados.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git stash` | Guarda cambios en el cajón temporal |
| `git stash push -m "msg"` | Guarda con descripción |
| `git stash -u` | Guarda incluyendo archivos nuevos |
| `git stash list` | Lista todos los stashes |
| `git stash pop` | Recupera y borra el último stash |
| `git stash apply` | Recupera sin borrar |
| `git stash drop` | Borra un stash sin aplicar |
| `git stash clear` | Borra todos los stashes |

---

> **Siguiente lección**: `02_git_tag.md` — Aprenderás a marcar versiones
> importantes de tu proyecto.
