# Lección 03: `git merge` — Unir ramas

## 📖 Teoría

### ¿Qué hace `git merge`?

`git merge` toma los cambios de una rama y los integra en otra. Es la forma
de decir: "el trabajo en esta rama está listo, quiero incorporarlo a la
rama principal".

### La regla fundamental

**Siempre te posicionas en la rama que RECIBE los cambios** y mergeas la
rama que APORTA los cambios.

```bash
# Quieres traer los cambios de "feature/postres" hacia "main"
git switch main                    # 1. Te posicionas en la que RECIBE
git merge feature/carta-postres    # 2. Mergeas la que APORTA
```

```
  ANTES del merge:
                                feature/carta-postres
                               /
  main ── C1 ── C2 ── C3 ── C4
                                \
                                 feature/menu-vegano

  DESPUÉS de: git switch main && git merge feature/carta-postres

  main ── C1 ── C2 ── C3 ── C4 ── C5(postres)
                                \
                                 feature/menu-vegano
```

### Los tres tipos de merge

#### 1. Fast-Forward Merge (avance rápido)

Se produce cuando la rama destino NO tiene commits nuevos desde que se
creó la rama origen. Git simplemente mueve el puntero hacia adelante.

```
  ANTES:
  main ── C1 ── C2
                  \
                   feature ── C3 ── C4

  DESPUÉS (fast-forward):
  main ── C1 ── C2 ── C3 ── C4
                               ↑
                           main (avanzó)
```

- No se crea un commit de merge.
- El historial queda lineal (como si hubieras trabajado directamente en main).
- Es el merge más limpio y simple.

#### 2. Three-Way Merge (merge de tres vías)

Se produce cuando AMBAS ramas tienen commits nuevos. Git encuentra el
**ancestro común**, compara los cambios de ambas ramas y los combina en
un nuevo **commit de merge**.

```
  ANTES:
  main ── C1 ── C2 ── C5 ── C6
                  \
                   feature ── C3 ── C4

  DESPUÉS (three-way merge):
  main ── C1 ── C2 ── C5 ── C6 ── M (merge commit)
                  \               /
                   feature ── C3 ── C4
```

- Se crea un commit de merge (`M`) con **dos padres**.
- El historial muestra la bifurcación y la unión.
- Git resuelve automáticamente los cambios si no hay conflictos.

#### 3. Merge con conflictos

Se produce cuando ambas ramas modificaron las **mismas líneas** del
**mismo archivo**. Git no puede decidir cuál versión mantener y te
pide que lo resuelvas tú. (Esto lo veremos en detalle en la lección 04.)

### El commit de merge

En un three-way merge, Git crea un commit especial:

```
┌─────────────────────────────────────────────────┐
│              MERGE COMMIT                        │
│                                                 │
│  Hash:    m1e2r3g                               │
│  Mensaje: "Merge branch 'feature/carta-postres'"│
│                                                 │
│  Padre 1: C6 (último commit de main)            │
│  Padre 2: C4 (último commit de feature)         │
│                                                 │
│  ↑ Tiene DOS padres, a diferencia de un commit  │
│    normal que solo tiene uno.                    │
└─────────────────────────────────────────────────┘
```

### Opciones de `git merge`

```bash
# Merge estándar (fast-forward si es posible, three-way si no)
git merge nombre-rama

# Forzar un commit de merge aunque sea fast-forward
git merge --no-ff nombre-rama

# Abortar un merge en curso (si hay conflictos)
git merge --abort

# Merge con mensaje personalizado
git merge nombre-rama -m "Mensaje personalizado"
```

### ¿`--no-ff` o fast-forward?

| Opción | Historial | Cuándo usarla |
|--------|-----------|---------------|
| Fast-forward (default) | Lineal, limpio | Ramas pequeñas con pocos commits |
| `--no-ff` | Muestra la bifurcación | Quieres preservar que hubo una rama |

```
  Fast-forward:                    --no-ff:
  C1 ── C2 ── C3 ── C4            C1 ── C2 ────────── M
                                          \          /
                                           C3 ── C4
  (historial lineal)              (se ve que hubo una rama)
```

### Después del merge: ¿qué pasa con la rama?

La rama mergeada sigue existiendo. Git no la borra automáticamente. Buena
práctica:

```bash
git merge feature/carta-postres       # Mergea
git branch -d feature/carta-postres   # Limpia la rama
```

### ⚠️ Importante

- Siempre haz `git status` antes de un merge para confirmar que tu
  Working Directory está limpio.
- El merge modifica la rama en la que estás (`HEAD`), nunca la otra.
- Si algo sale mal, `git merge --abort` te devuelve al estado anterior.

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado la lección 02 y tener las ramas
> `feature/carta-postres` (con un commit de postres) y `feature/menu-vegano`
> (con un commit de opciones veganas).

Verifica tu punto de partida:

```bash
git log --oneline --graph --all
```

Deberías ver algo como:

```
* abc1234 (feature/menu-vegano) Agrega opciones veganas al menú
| * def5678 (feature/carta-postres) Agrega carta de postres al menú
|/
* df2de15 (HEAD -> main) Inicializa curso...
```

### Ejercicio 1: Fast-Forward Merge

Estando en `main`, mergea la rama de postres:

```bash
git switch main
git status
git merge feature/carta-postres
```

✅ **Resultado esperado**: Como `main` no tiene commits nuevos desde que
se creó `feature/carta-postres`, Git hace un fast-forward:

```
Updating df2de15..def5678
Fast-forward
 proyecto/menu.txt | 6 ++++++
 1 file changed, 6 insertions(+)
```

Verifica el contenido:

```bash
cat proyecto/menu.txt
```

✅ La carta de postres ahora está en `main`.

Verifica el historial:

```bash
git log --oneline --graph --all
```

```
* abc1234 (feature/menu-vegano) Agrega opciones veganas al menú
| * def5678 (HEAD -> main, feature/carta-postres) Agrega carta de postres...
|/
* df2de15 Inicializa curso...
```

💡 Observa que `main` y `feature/carta-postres` ahora apuntan al mismo
commit. El puntero de `main` simplemente avanzó.

---

### Ejercicio 2: Limpia la rama mergeada

Ya no necesitas la rama de postres:

```bash
git branch -d feature/carta-postres
```

✅ **Resultado esperado**: Se elimina sin problemas porque ya está mergeada.

---

### Ejercicio 3: Three-Way Merge

Ahora `main` tiene el commit de postres, pero `feature/menu-vegano` partió
de un punto anterior. Primero, creemos un commit en `main` para forzar un
three-way merge:

```bash
echo "" >> proyecto/inventario.txt
echo "## Productos orgánicos" >> proyecto/inventario.txt
echo "- Tofu: 5 kg" >> proyecto/inventario.txt
echo "- Leche de avena: 12 litros" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega productos orgánicos al inventario"
```

Ahora mira el gráfico:

```bash
git log --oneline --graph --all
```

```
* xyz9999 (HEAD -> main) Agrega productos orgánicos al inventario
* def5678 Agrega carta de postres al menú
| * abc1234 (feature/menu-vegano) Agrega opciones veganas al menú
|/
* df2de15 Inicializa curso...
```

`main` y `feature/menu-vegano` han **divergido**: ambas tienen commits
que la otra no tiene. Ahora el merge será three-way.

```bash
git merge feature/menu-vegano
```

✅ **Resultado esperado**: Git abre tu editor para escribir un mensaje
de merge (o lo genera automáticamente). Si se abre un editor, guarda y
cierra. Verás algo como:

```
Merge made by the 'ort' strategy.
 proyecto/menu.txt | 6 ++++++
 1 file changed, 6 insertions(+)
```

💡 **Estrategia ORT**: "Ostensibly Recursive's Twin" es la estrategia por defecto desde Git 2.34. Diseñada para ser mucho más rápida y manejar mejor las detecciones de renombres que la antigua estrategia `recursive`. En repositorios grandes, la mejora de rendimiento es masiva.

---

### Ejercicio 4: Observa el commit de merge

```bash
git log --oneline --graph --all
```

✅ **Resultado esperado**: Ahora ves la bifurcación Y la unión:

```
*   mmmnnnn (HEAD -> main) Merge branch 'feature/menu-vegano'
|\
| * abc1234 (feature/menu-vegano) Agrega opciones veganas al menú
* | xyz9999 Agrega productos orgánicos al inventario
* | def5678 Agrega carta de postres al menú
|/
* df2de15 Inicializa curso...
```

💡 El commit de merge tiene dos líneas entrando (dos padres). Esto es
lo que diferencia un merge commit de un commit normal.

Verifica el contenido de `menu.txt`:

```bash
cat proyecto/menu.txt
```

✅ Verás tanto la carta de postres como las opciones veganas. Git combinó
automáticamente los cambios de ambas ramas.

---

### Ejercicio 5: Limpia la rama mergeada

```bash
git branch -d feature/menu-vegano
```

Verifica:

```bash
git branch
git log --oneline --graph
```

✅ Solo queda `main`, con todo el historial integrado.

---

### Ejercicio 6: Merge con `--no-ff`

Crea una rama rápida para probar `--no-ff`:

```bash
git switch -c feature/bebidas-premium
echo "" >> proyecto/menu.txt
echo "## Bebidas premium" >> proyecto/menu.txt
echo "- Vino reserva — 8.00€" >> proyecto/menu.txt
echo "- Cóctel del chef — 9.50€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega sección de bebidas premium"
```

Ahora mergea con `--no-ff`:

```bash
git switch main
git merge --no-ff feature/bebidas-premium -m "Incorpora carta de bebidas premium"
```

```bash
git log --oneline --graph -5
```

✅ **Resultado esperado**: Aunque podría haber sido fast-forward, `--no-ff`
fuerza un commit de merge:

```
*   nnn1234 (HEAD -> main) Incorpora carta de bebidas premium
|\
| * bbb5678 (feature/bebidas-premium) Agrega sección de bebidas premium
|/
* mmmnnnn Merge branch 'feature/menu-vegano'
...
```

💡 Con `--no-ff` siempre queda claro que hubo una rama, incluso si los
cambios eran lineales.

Limpia:

```bash
git branch -d feature/bebidas-premium
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git merge rama` | Merge estándar (ff si posible) |
| `git merge --no-ff rama` | Fuerza commit de merge |
| `git merge --abort` | Aborta un merge con conflictos |
| `git branch -d rama` | Limpia la rama después del merge |

| Tipo de merge | Cuándo ocurre | Resultado |
|---------------|--------------|-----------|
| Fast-forward | La rama destino no avanzó | Historial lineal |
| Three-way | Ambas ramas avanzaron | Commit de merge con dos padres |
| Con conflictos | Mismas líneas modificadas | Requiere resolución manual |

**Regla de oro**: Siempre posiciónate en la rama que RECIBE (`git switch main`)
antes de mergear. Y limpia las ramas que ya no necesites.

---

> **Siguiente lección**: `04_conflictos.md` — Aprenderás a resolver
> el temido "CONFLICT" sin miedo.
