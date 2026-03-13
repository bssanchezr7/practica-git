# Lección 02: `git revert` — Deshacer commits de forma segura

## 📖 Teoría

### ¿Qué hace `git revert`?

`git revert` crea un **nuevo commit** que deshace los cambios de un commit anterior. Según [Pro Git (Capítulo 2.4)](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things#_reverting_commits), esta es la forma más segura de deshacer cambios en ramas públicas, ya que no borra nada del historial — añade un commit que "invierte" los cambios.

```
  ANTES:
  C1 ── C2 ── C3 ── C4(error)
                      ↑ main

  git revert C4

  DESPUÉS:
  C1 ── C2 ── C3 ── C4(error) ── C5(revert de C4)
                                   ↑ main

  C4 sigue existiendo en el historial, pero C5 deshace sus cambios.
```

### ¿Por qué no simplemente borrar el commit?

Porque **el historial es sagrado**, especialmente si trabajas en equipo:

| Acción | Qué hace | ¿Altera el historial? | ¿Seguro en equipo? |
|--------|---------|----------------------|---------------------|
| `git revert` | Crea un commit inverso | No | ✅ Sí |
| `git reset` | Borra commits | Sí | ⚠️ Peligroso |

Si ya pusheaste un commit, **siempre usa `revert`**. Borrar commits
pusheados causa problemas a todos los que tengan esos commits.

### ¿Qué hace exactamente el commit de revert?

Si el commit original añadió 3 líneas y borró 1, el revert:
- **Borra** las 3 líneas añadidas.
- **Restaura** la línea borrada.

Es una inversión exacta, línea por línea.

### Sintaxis

```bash
# Revertir un commit específico (por hash)
git revert abc1234

# Revertir el último commit
git revert HEAD

# Revertir sin hacer commit automático (para editar antes)
git revert --no-commit abc1234

# Revertir varios commits (de más reciente a más antiguo)
git revert HEAD~2..HEAD

# Abortar un revert en curso
git revert --abort
```

### Revert de un merge commit

Los merge commits tienen dos padres, así que `git revert` necesita saber
cuál padre mantener:

```bash
git revert -m 1 abc1234    # Mantiene el padre 1 (normalmente main)
```

### ⚠️ Importante

- `git revert` puede generar **conflictos** si los cambios que intenta
  deshacer fueron modificados por commits posteriores.
- Si hay conflicto, resuélvelo como cualquier merge conflict y completa
  con `git revert --continue`.
- El mensaje del commit de revert se genera automáticamente:
  `Revert "mensaje del commit original"`.

---

## 💻 Práctica

### Ejercicio 1: Crea un commit con error

```bash
cd ~/practica_git
echo "" >> proyecto/menu.txt
echo "## ESTA SECCIÓN ES UN ERROR" >> proyecto/menu.txt
echo "- Plato inexistente — 999.99€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega sección errónea al menú (a propósito)"
git log --oneline -3
```

✅ El commit con el error está en el historial.

---

### Ejercicio 2: Revierte el error

```bash
git revert HEAD
```

✅ Git abre tu editor con un mensaje como:
`Revert "Agrega sección errónea al menú (a propósito)"`

Guarda y cierra. Verifica:

```bash
git log --oneline -4
cat proyecto/menu.txt
```

✅ El historial tiene DOS commits nuevos:
1. El commit con el error.
2. El commit que lo revierte.

Y `menu.txt` ya no tiene la sección errónea.

---

### Ejercicio 3: Revierte un commit antiguo (no el último)

Haz dos commits:

```bash
echo "## Horario de verano" >> proyecto/menu.txt
echo "Abierto hasta las 00:00 en julio y agosto" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega horario de verano"

echo "- Sangría de la casa — 8.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega sangría al menú"
```

Ahora quieres revertir el horario de verano (penúltimo commit) pero
mantener la sangría:

```bash
git log --oneline -4
git revert HEAD~1
```

✅ Si no hay conflicto, Git crea un commit que solo deshace el horario
de verano. La sangría permanece.

Si hay conflicto (porque ambos commits tocaron las mismas líneas),
resuélvelo manualmente y:

```bash
git add proyecto/menu.txt
git revert --continue
```

---

### Ejercicio 4: Revert con `--no-commit`

```bash
echo "Línea temporal para revert test" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Cambio temporal en inventario"

git revert --no-commit HEAD
git status
```

✅ Los cambios inversos están en el Staging Area pero NO se ha creado
el commit todavía. Puedes revisar, ajustar y luego:

```bash
git commit -m "Revierte cambio temporal (revisado manualmente)"
```

💡 `--no-commit` te da control total antes de confirmar el revert.

---

### Ejercicio 5: Aborta un revert

```bash
echo "Otro cambio" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Cambio para probar abort"

git revert --no-commit HEAD
git status
```

Cambiaste de opinión. Aborta:

```bash
git revert --abort
git status
```

✅ Todo vuelve a como estaba antes del revert.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git revert HEAD` | Revierte el último commit |
| `git revert abc123` | Revierte un commit específico |
| `git revert --no-commit HEAD` | Revierte sin commitear |
| `git revert --abort` | Cancela un revert en curso |
| `git revert -m 1 abc123` | Revierte un merge commit |

**Regla de oro**: Si ya hiciste push, usa `git revert`. Nunca reescribas
historial que otros ya tienen.

---

> **Siguiente lección**: `03_git_reset.md` — Aprenderás la herramienta
> más potente (y peligrosa) para deshacer cambios.
