# Lección 03: `git reset` — Reescribir el historial (con cuidado)

## 📖 Teoría

### ¿Qué hace `git reset`?

`git reset` es un comando potente que suele malinterpretarse. Según [Pro Git (Capítulo 7.7): Reset Demystified](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified), este comando manipula los tres árboles de Git (HEAD, Index y Working Directory) para deshacer cambios. Es la forma más directa de "borrar" o mover commits del historial local.

### Los tres modos de reset

```
                                  ¿Modifica    ¿Modifica     ¿Modifica
  Modo         Comando            Repository?  Staging?      Working Dir?
  ──────────── ────────────────── ──────────── ──────────── ────────────
  --soft       git reset --soft   ✅ Sí        ❌ No        ❌ No
  --mixed      git reset          ✅ Sí        ✅ Sí        ❌ No
  (default)
  --hard       git reset --hard   ✅ Sí        ✅ Sí        ✅ Sí
```

### `--soft`: Solo mueve HEAD

```
  ANTES:  C1 ── C2 ── C3       Working Dir: tiene cambios de C3
                        ↑ HEAD  Staging: vacío

  git reset --soft HEAD~1

  DESPUÉS: C1 ── C2            Working Dir: tiene cambios de C3
                  ↑ HEAD        Staging: cambios de C3 están aquí
```

Los cambios del commit "borrado" quedan **en el Staging Area**, listos
para hacer un nuevo commit. Útil para rehacer un commit con diferente
mensaje o combinarlo con otros cambios.

### `--mixed` (default): Mueve HEAD y limpia el staging

```
  ANTES:  C1 ── C2 ── C3       Working Dir: tiene cambios de C3
                        ↑ HEAD  Staging: vacío

  git reset HEAD~1   (o git reset --mixed HEAD~1)

  DESPUÉS: C1 ── C2            Working Dir: tiene cambios de C3
                  ↑ HEAD        Staging: vacío
```

Los cambios quedan en el **Working Directory** pero no en el staging.
Tendrías que hacer `git add` otra vez antes de commitear.

### `--hard`: Borra todo

```
  ANTES:  C1 ── C2 ── C3       Working Dir: tiene cambios de C3
                        ↑ HEAD  Staging: algo preparado

  git reset --hard HEAD~1

  DESPUÉS: C1 ── C2            Working Dir: vuelve al estado de C2
                  ↑ HEAD        Staging: vacío

  ⚠️ Los cambios de C3 se PIERDEN COMPLETAMENTE
```

`--hard` es **destructivo e irreversible** (a menos que uses `git reflog`,
que es tu último recurso).

### Comparación visual

```
  git reset --soft HEAD~1:
  ┌─────────────────────────────────────────┐
  │ HEAD se mueve, el resto queda igual     │
  │ → Ideal para rehacer un commit          │
  └─────────────────────────────────────────┘

  git reset --mixed HEAD~1 (default):
  ┌─────────────────────────────────────────┐
  │ HEAD se mueve, staging se limpia        │
  │ → Ideal para reorganizar antes de commit│
  └─────────────────────────────────────────┘

  git reset --hard HEAD~1:
  ┌─────────────────────────────────────────┐
  │ HEAD se mueve, staging Y disco se borran│
  │ → ⚠️ Solo si estás seguro de perder todo│
  └─────────────────────────────────────────┘
```

### `git reset` vs `git revert`

| Aspecto | `git reset` | `git revert` |
|---------|------------|-------------|
| Modifica el historial | Sí (borra commits) | No (añade un commit nuevo) |
| Seguro en equipo | ⚠️ No, si ya hiciste push | ✅ Siempre seguro |
| Reversible | Difícil (reflog) | Fácil (revert del revert) |
| Cuándo usarlo | Commits locales, aún no pusheados | Commits ya pusheados |

### El salvavidas: `git reflog`

Si haces un `reset --hard` por error, `git reflog` muestra TODOS los
movimientos de HEAD, incluidos los commits "borrados":

```bash
git reflog
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: El commit que "borraste"

git reset --hard def5678    # ¡Lo recuperas!
```

⚠️ `reflog` solo funciona localmente y los registros expiran después
de ~90 días.

### ⚠️ Regla de oro para reset

```
  ¿Ya hiciste push del commit?
       │
       ├── SÍ → Usa git revert (NUNCA reset)
       │
       └── NO → Puedes usar git reset
                    │
                    ├── ¿Quieres rehacer el commit? → --soft
                    ├── ¿Quieres re-seleccionar qué preparar? → --mixed
                    └── ¿Quieres borrar todo rastro? → --hard
```

---

## 💻 Práctica

### Ejercicio 1: Reset `--soft` — Rehacer un commit

Crea un commit que quieras rehacer:

```bash
cd ~/practica_git
echo "Texto con errror de ortografía" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega tetxo con typos"
git log --oneline -3
```

Ups, el mensaje y el contenido tienen errores. Deshaz el commit pero
mantén los cambios preparados:

```bash
git reset --soft HEAD~1
git status
```

✅ El commit desapareció pero los cambios están en el Staging Area (verde).

Corrige el contenido y haz un nuevo commit:

```bash
git restore --staged proyecto/menu.txt
git restore proyecto/menu.txt
echo "Texto corregido sin errores" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega texto corregido al menú"
git log --oneline -3
```

✅ Un commit limpio reemplaza al que tenía errores.

---

### Ejercicio 2: Reset `--mixed` — Deshacer el staging

```bash
echo "Cambio A" >> proyecto/menu.txt
echo "Cambio B" >> proyecto/inventario.txt
git add .
git commit -m "Dos cambios que deberían ir separados"
```

Querías hacer dos commits separados. Deshaz:

```bash
git reset HEAD~1
git status
```

✅ Ambos archivos están modificados (rojo) pero no preparados. Ahora
puedes hacer commits separados:

```bash
git add proyecto/menu.txt
git commit -m "Aplica cambio A al menú"

git add proyecto/inventario.txt
git commit -m "Aplica cambio B al inventario"

git log --oneline -4
```

✅ Dos commits limpios en vez de uno mezclado.

---

### Ejercicio 3: Reset `--hard` — El botón nuclear

```bash
echo "Cambio que voy a borrar completamente" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Commit que quiero eliminar por completo"
git log --oneline -3
cat proyecto/menu.txt
```

Ahora bórralo completamente:

```bash
git reset --hard HEAD~1
git log --oneline -3
cat proyecto/menu.txt
```

✅ El commit desapareció del historial Y el archivo volvió al estado
anterior. Es como si el commit nunca hubiera existido.

---

### Ejercicio 4: Rescata un commit "borrado" con reflog

```bash
echo "Dato MUY importante" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega dato crucial"
git log --oneline -2
```

Ahora "bórralo" accidentalmente:

```bash
git reset --hard HEAD~1
git log --oneline -2
```

✅ El commit desapareció. Pero no todo está perdido:

```bash
git reflog -5
```

✅ Ves el commit "borrado" con su hash. Recupéralo:

```bash
git reset --hard HEAD@{1}
git log --oneline -3
cat proyecto/inventario.txt
```

✅ ¡El dato crucial está de vuelta!

💡 `git reflog` es tu red de seguridad. Mientras no hayan pasado ~90
días, casi todo es recuperable.

Limpia este cambio de prueba:

```bash
git reset --hard HEAD~1
```

---

### Ejercicio 5: Reset de archivos específicos del staging

`git reset` también puede quitar archivos individuales del staging
(equivalente a `git restore --staged`):

```bash
echo "Cambio 1" >> proyecto/menu.txt
echo "Cambio 2" >> proyecto/inventario.txt
git add .
git status
```

Quita solo uno del staging:

```bash
git reset proyecto/inventario.txt
git status
```

✅ `menu.txt` sigue en verde, `inventario.txt` volvió a rojo.

Limpia:

```bash
git restore .
git status
```

---

## 🧠 Resumen

| Comando | HEAD | Staging | Working Dir | Uso típico |
|---------|------|---------|-------------|------------|
| `reset --soft HEAD~1` | Mueve | Intacto | Intacto | Rehacer un commit |
| `reset HEAD~1` | Mueve | Limpia | Intacto | Reorganizar cambios |
| `reset --hard HEAD~1` | Mueve | Limpia | Limpia | Borrar todo rastro |
| `reflog` | — | — | — | Recuperar commits perdidos |

```
  ¿Pusheaste?  SÍ → git revert
               NO → git reset (soft/mixed/hard según necesidad)
```

---

> **Siguiente lección**: `04_ejercicio_final.md` — Ejercicio integrador
> donde practicarás todas las formas de deshacer cambios.
