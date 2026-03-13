# Lección 02: `git switch` — Cambiar de rama

## 📖 Teoría

### ¿Qué hace `git switch`?

`git switch` mueve el puntero **HEAD** a otra rama. Según [Pro Git (Capítulo 3.1)](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell#_switching_branches), este comando fue introducido en Git 2.23 para proporcionar una alternativa más específica y segura a `git checkout` para cambiar entre ramas.

### `git switch` vs `git checkout`

Antes de Git 2.23 (2019), se usaba `git checkout` para todo: cambiar de
rama, restaurar archivos, crear ramas... era un comando sobrecargado.

Git 2.23 lo dividió en dos comandos más claros:

| Comando | Propósito |
|---------|-----------|
| `git switch` | Cambiar de rama |
| `git restore` | Restaurar archivos |

`git checkout` sigue funcionando para compatibilidad, pero `git switch` es
más intuitivo y seguro (no te deja perder cambios accidentalmente).

```
  ANTES (un comando para todo):
  git checkout = cambiar rama + restaurar archivos + crear ramas + ...

  AHORA (cada uno con su propósito):
  git switch   = cambiar de rama
  git restore  = restaurar archivos
```

### ¿Qué pasa cuando cambias de rama?

```
  ANTES de switch              DESPUÉS de git switch feature/carta-postres
  ──────────────               ───────────────────────────────────────────

  main ── C1 ── C2 ── C3      main ── C1 ── C2 ── C3
                  ↑                              ↑
                HEAD                           (main sigue aquí)

  feature/carta-postres        feature/carta-postres
  (apunta a C3 también)                  ↑
                                       HEAD  ← ahora estás aquí
```

1. HEAD se mueve a la nueva rama.
2. Tu Working Directory se actualiza: los archivos cambian para reflejar
   el estado de esa rama.
3. El Staging Area se limpia para coincidir con la nueva rama.

### Sintaxis

```bash
# Cambiar a una rama existente
git switch nombre-rama

# Crear una rama nueva Y cambiar a ella (atajo)
git switch -c nueva-rama

# Volver a la rama anterior (como cd -)
git switch -

# Entrar en estado de "Detached HEAD" (inspeccionar un commit)
git switch --detach <hash-del-commit>
```

### ⚠️ Importante: Cambios sin commitear

Si tienes cambios sin commitear en tu Working Directory y cambias de rama,
Git se comporta así:

| Situación | Qué hace Git |
|-----------|-------------|
| Los cambios no generan conflicto con la otra rama | Te deja cambiar, los cambios viajan contigo |
| Los cambios SÍ generan conflicto | **Te bloquea** y te pide que primero commitees o guardes los cambios (con `git stash`) |

💡 **Consejo**: Acostúmbrate a commitear (o hacer stash) antes de cambiar
de rama. Un Working Directory limpio evita sorpresas.

### El atajo `git switch -c`

Este es probablemente el comando que más usarás en tu día a día:

```bash
git switch -c feature/nueva-funcionalidad
```

Equivale a crear la rama y cambiar a ella en un solo paso. Según la documentación oficial, esto es más seguro que el antiguo `git checkout -b` ya que realiza comprobaciones adicionales para evitar sobrescribir cambios.

### ⚠️ El estado "Detached HEAD"

Si usas `git switch --detach <commit>` o `git switch <tag>`, entras en un estado llamado **Detached HEAD** (HEAD desprendido).

- **¿Qué es?**: HEAD apunta directamente a un commit, no a una rama.
- **Riesgo**: Si haces commits en este estado, no pertenecen a ninguna rama y podrían perderse ("quedar huérfanos") al cambiar de nuevo a una rama.
- **Cómo salir**: 
    - Para descartar: `git switch main`.
    - Para guardar lo que hiciste: `git switch -c nueva-rama`.

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado la lección 01 y tener la rama
> `feature/carta-postres` creada. Verifica con `git branch`.

### Ejercicio 1: Cambia a la rama de postres

```bash
git branch
git switch feature/carta-postres
```

✅ **Resultado esperado**:

```
Switched to branch 'feature/carta-postres'
```

Verifica:

```bash
git branch
```

```
* feature/carta-postres
  main
```

El asterisco ahora está en `feature/carta-postres`.

---

### Ejercicio 2: Haz cambios en la rama

Ahora que estás en la rama de postres, añade contenido al menú:

```bash
echo "" >> proyecto/menu.txt
echo "## Carta de postres" >> proyecto/menu.txt
echo "- Tiramisú — 6.00€" >> proyecto/menu.txt
echo "- Brownie con helado — 7.50€" >> proyecto/menu.txt
echo "- Fruta de temporada — 4.00€" >> proyecto/menu.txt
```

Haz un commit en esta rama:

```bash
git add proyecto/menu.txt
git commit -m "Agrega carta de postres al menú"
```

✅ **Resultado esperado**: El commit se crea en la rama
`feature/carta-postres`.

---

### Ejercicio 3: Vuelve a `main` y observa la magia

```bash
git switch main
```

Ahora mira el contenido de `proyecto/menu.txt`:

```bash
cat proyecto/menu.txt
```

✅ **Resultado esperado**: ¡La carta de postres NO está! El archivo
vuelve al estado de `main`, que no tiene esos cambios.

💡 **Concepto clave**: Cada rama tiene su propia "realidad". Los commits
de `feature/carta-postres` solo existen allí hasta que los mergees.

Vuelve a la rama de postres para verificar:

```bash
git switch feature/carta-postres
cat proyecto/menu.txt
```

✅ **Resultado esperado**: La carta de postres está de nuevo. Git
intercambia los archivos al cambiar de rama.

---

### Ejercicio 4: Crea una rama y cámbiate en un solo paso

Vuelve a `main` y usa el atajo `-c`:

```bash
git switch main
git switch -c feature/menu-vegano
```

✅ **Resultado esperado**:

```
Switched to a new branch 'feature/menu-vegano'
```

Verifica:

```bash
git branch
```

```
  feature/carta-postres
* feature/menu-vegano
  main
```

---

### Ejercicio 5: Trabaja en la nueva rama

Haz cambios en esta rama también:

```bash
echo "" >> proyecto/menu.txt
echo "## Opciones veganas" >> proyecto/menu.txt
echo "- Hamburguesa de lentejas — 11.00€" >> proyecto/menu.txt
echo "- Curry de verduras — 10.50€" >> proyecto/menu.txt
echo "- Bowl de quinoa — 9.00€" >> proyecto/menu.txt
```

```bash
git add proyecto/menu.txt
git commit -m "Agrega opciones veganas al menú"
```

---

### Ejercicio 6: Visualiza la divergencia

Ahora tienes tres ramas, dos de las cuales han divergido de `main`:

```bash
git log --oneline --graph --all
```

✅ **Resultado esperado**: Ves un gráfico donde las ramas se separan:

```
* abc1234 (HEAD -> feature/menu-vegano) Agrega opciones veganas al menú
| * def5678 (feature/carta-postres) Agrega carta de postres al menú
|/
* df2de15 (main) Inicializa curso...
```

💡 Este gráfico muestra que `feature/menu-vegano` y `feature/carta-postres`
partieron del mismo punto (`main`) pero cada una tiene su propio commit.

---

### Ejercicio 7: El atajo `git switch -`

Navega rápidamente entre las dos últimas ramas:

```bash
git switch main
git switch -
```

✅ **Resultado esperado**: `git switch -` te lleva de vuelta a
`feature/menu-vegano` (la rama anterior). Es como `cd -` en la terminal.

```bash
git switch -
```

Ahora estás de vuelta en `main`. Cada `git switch -` alterna entre las
dos últimas ramas.

---

### Ejercicio 8: ¿Qué pasa con cambios sin commitear?

Estando en `main`, haz un cambio SIN commitear:

```bash
echo "# Cambio sin guardar" >> proyecto/inventario.txt
git status
```

Intenta cambiar de rama:

```bash
git switch feature/carta-postres
```

✅ **Resultado esperado**: En este caso, Git probablemente te deja cambiar
y el cambio "viaja" contigo (porque no hay conflicto). Verifica:

```bash
git status
cat proyecto/inventario.txt
```

El cambio sigue ahí, pero ahora estás en `feature/carta-postres`.

⚠️ Esto puede ser confuso. Por eso la mejor práctica es **siempre commitear
o hacer stash antes de cambiar de rama**.

Limpia el cambio y vuelve a main:

```bash
git restore proyecto/inventario.txt
git switch main
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git switch rama` | Cambia a una rama existente |
| `git switch -c rama` | Crea y cambia a nueva rama |
| `git switch -` | Vuelve a la rama anterior |
| `git checkout rama` | Estilo antiguo (mismo efecto) |
| `git checkout -b rama` | Estilo antiguo: crear + cambiar |

**Regla de oro**: Usa `git switch -c` para crear ramas de trabajo y
`git switch main` para volver a la base. Siempre commitea antes de cambiar.

---

> **Siguiente lección**: `03_git_merge.md` — Aprenderás a unir
> el trabajo de diferentes ramas.
