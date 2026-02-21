# Lección 02: `git push` — Enviar cambios al remoto

## 📖 Teoría

### ¿Qué hace `git push`?

`git push` envía tus commits locales al repositorio remoto. Es como
publicar tu trabajo: lo que solo existía en tu máquina ahora está
disponible para otros (o guardado en el servidor como respaldo).

```
  Repo Local                          Repo Remoto (origin)
  ┌──────────────────┐               ┌──────────────────┐
  │ C1 ── C2 ── C3   │               │ C1 ── C2         │
  │              ↑    │  git push     │           ↑      │
  │            main   │──────────────▶│         main     │
  └──────────────────┘               └──────────────────┘

  DESPUÉS del push:
  ┌──────────────────┐               ┌──────────────────┐
  │ C1 ── C2 ── C3   │               │ C1 ── C2 ── C3   │
  │              ↑    │               │              ↑    │
  │            main   │               │            main   │
  └──────────────────┘               └──────────────────┘
```

### Sintaxis

```bash
# Push de la rama actual al remoto
git push origin main

# Push con seguimiento (configura la relación upstream)
git push -u origin main

# Push de la rama actual (si ya tiene upstream)
git push

# Push de todas las ramas
git push --all origin

# Push de tags
git push --tags
```

### ¿Qué es upstream (`-u`)?

Cuando haces `git push -u origin main`, le dices a Git: "esta rama local
`main` está vinculada a la rama `main` en `origin`". Después de eso,
puedes hacer solo `git push` sin especificar remoto ni rama.

```bash
# Primera vez: estableces la relación
git push -u origin main

# Todas las veces siguientes: Git ya sabe dónde enviar
git push
```

### ¿Cuándo falla un push?

El push puede ser **rechazado** si el remoto tiene commits que tú no
tienes:

```
  Tu local:    C1 ── C2 ── C3
  El remoto:   C1 ── C2 ── C4   ← Alguien más pusheó C4

  git push → RECHAZADO
  "Updates were rejected because the remote contains work that you
   do not have locally."
```

**Solución**: Primero haz `git pull` para traer los cambios del remoto,
resuélvelos si hay conflictos, y luego intenta `git push` de nuevo.

```
  1. git pull        → Traes C4 y lo mergeas con tu C3
  2. Tu local:  C1 ── C2 ── C3 ── C4 ── M (merge)
  3. git push        → Ahora sí funciona
```

### ⚠️ Importante

- **Nunca uses `git push --force`** a menos que sepas exactamente lo que
  haces. Sobrescribe el historial remoto y puede borrar el trabajo de
  otros.
- `git push` solo envía **commits**. Los cambios en tu Working Directory
  o Staging Area no se envían.
- Si no has hecho commit, no hay nada que pushear.

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado la lección 01 y tener el
> remoto `origin` configurado (apuntando a `/tmp/remoto_simulado.git`
> o a un repositorio en GitHub). Verifica con `git remote -v`.

### Ejercicio 1: Tu primer push

```bash
cd ~/practica_git
git push -u origin main
```

✅ **Resultado esperado**:

```
Enumerating objects: ..., done.
Counting objects: ..., done.
...
To /tmp/remoto_simulado.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

💡 El `-u` estableció el upstream. De aquí en adelante puedes usar solo
`git push`.

---

### Ejercicio 2: Haz cambios y vuelve a pushear

```bash
echo "## Promoción de la semana" >> proyecto/menu.txt
echo "- 2x1 en pizzas los martes" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega promoción semanal al menú"

git push
```

✅ **Resultado esperado**: Git envía el nuevo commit al remoto sin
necesidad de especificar `origin main` (gracias al `-u` anterior).

---

### Ejercicio 3: Verifica que el remoto recibió los cambios

```bash
git log --oneline -3
git log --oneline --remotes -3
```

✅ `origin/main` debería estar en el mismo commit que `main`.

También puedes verificar clonando el remoto en otra carpeta:

```bash
git clone /tmp/remoto_simulado.git /tmp/verificacion
cd /tmp/verificacion
git log --oneline -5
cat proyecto/menu.txt
```

✅ La promoción está ahí. El remoto tiene tus cambios.

```bash
cd ~/practica_git
rm -rf /tmp/verificacion
```

---

### Ejercicio 4: Push de una rama diferente

Crea una rama, trabaja en ella y púsheala:

```bash
git switch -c feature/horario
echo "" >> proyecto/menu.txt
echo "## Horario" >> proyecto/menu.txt
echo "Lunes a Sábado: 12:00 - 23:00" >> proyecto/menu.txt
echo "Domingo: 12:00 - 16:00" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega horario del restaurante"

git push -u origin feature/horario
```

✅ **Resultado esperado**: La rama se crea en el remoto:

```
To /tmp/remoto_simulado.git
 * [new branch]      feature/horario -> feature/horario
```

Verifica las ramas remotas:

```bash
git branch -a
```

✅ Ves `remotes/origin/feature/horario` y `remotes/origin/main`.

Vuelve a main y limpia:

```bash
git switch main
git merge feature/horario
git push
git branch -d feature/horario
git push origin --delete feature/horario
```

💡 `git push origin --delete rama` elimina la rama del remoto.

---

### Ejercicio 5: Simula un push rechazado

Vamos a crear una situación donde alguien más modificó el remoto:

```bash
# Simula que otro desarrollador clona y pushea
git clone /tmp/remoto_simulado.git /tmp/otro_dev
cd /tmp/otro_dev
echo "Cambio del otro dev" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Otro dev actualiza inventario"
git push origin main
cd ~/practica_git
```

Ahora haz un cambio local y pushea:

```bash
echo "Mi cambio local" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Actualiza inventario localmente"

git push
```

✅ **Resultado esperado**: El push es **rechazado** porque el remoto
tiene un commit que tú no tienes.

La solución:

```bash
git pull
```

Si hay conflicto, resuélvelo como aprendiste en el Módulo 4.
Si no, Git hace un merge automático. Luego:

```bash
git push
```

✅ Ahora funciona. Tu historial incluye ambos cambios.

```bash
rm -rf /tmp/otro_dev
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git push -u origin main` | Envía y configura upstream |
| `git push` | Envía al upstream configurado |
| `git push origin rama` | Envía una rama específica |
| `git push --all` | Envía todas las ramas |
| `git push origin --delete rama` | Elimina rama del remoto |

**Regla de oro**: Siempre haz `git pull` antes de `git push` si trabajas
en equipo. Esto evita rechazos y mantiene tu rama actualizada.

---

> **Siguiente lección**: `03_git_pull_fetch.md` — Aprenderás a traer
> cambios del remoto a tu repositorio local.
