# Lección 04: Resolver conflictos de merge

## 📖 Teoría

### ¿Qué es un conflicto?

Un conflicto ocurre cuando dos ramas modifican las **mismas líneas** del **mismo archivo** de formas diferentes. Según [Pro Git (Capítulo 3.2)](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merge_conflicts), Git es capaz de resolver la mayoría de las mezclas automáticamente, pero requiere intervención humana cuando los cambios se solapan.

### ¿Cuándo NO hay conflicto?

Git resuelve automáticamente la mayoría de situaciones:

| Situación | ¿Conflicto? |
|-----------|-------------|
| Rama A modifica archivo1, Rama B modifica archivo2 | No |
| Ambas ramas modifican el mismo archivo, pero líneas diferentes | No |
| Ambas ramas añaden contenido al final del mismo archivo | A veces |
| Ambas ramas modifican las **mismas líneas** | **Sí** |
| Una rama borra un archivo que la otra modifica | **Sí** |

### ¿Cuándo SÍ hay conflicto?

```
  main:                          feature:
  ┌──────────────────────┐      ┌──────────────────────┐
  │ - Spaghetti — 13.50€ │      │ - Spaghetti — 14.00€ │
  └──────────────────────┘      └──────────────────────┘
            ↑                              ↑
   Cambió el precio a 13.50     Cambió el precio a 14.00

  Git: "¿Cuál es el precio correcto? No puedo decidir. CONFLICTO."
```

### Anatomía de un conflicto

Cuando ocurre un conflicto, Git marca el archivo con **marcadores de
conflicto**:

```
## Platos principales
<<<<<<< HEAD
- Spaghetti carbonara — 13.50€
=======
- Spaghetti carbonara — 14.00€
>>>>>>> feature/nuevo-precio
- Pizza margarita — 11.00€
```

Desglosemos:

```
<<<<<<< HEAD
  (contenido de TU rama actual — la que recibe el merge)
=======
  (contenido de LA OTRA rama — la que estás mergeando)
>>>>>>> nombre-de-la-otra-rama
```

| Marcador | Significado |
|----------|------------|
| `<<<<<<< HEAD` | Inicio del conflicto: versión de tu rama actual |
| `=======` | Separador entre las dos versiones |
| `>>>>>>> rama` | Fin del conflicto: versión de la otra rama |

### Los tres pasos para resolver un conflicto

```
  1. IDENTIFICAR              2. EDITAR                 3. CONFIRMAR
  ┌──────────────┐           ┌──────────────┐          ┌──────────────┐
  │ git status   │           │ Abre el      │          │ git add      │
  │ muestra los  │──────────▶│ archivo y    │─────────▶│ git commit   │
  │ archivos con │           │ elige qué    │          │              │
  │ conflictos   │           │ mantener     │          │              │
  └──────────────┘           └──────────────┘          └──────────────┘
```

**Paso 1: Identificar** — `git status` muestra los archivos en conflicto
bajo "Unmerged paths".

**Paso 2: Editar** — Abre cada archivo con conflictos y decide:
- ¿Mantengo la versión de mi rama? (borra la otra y los marcadores)
- ¿Mantengo la versión de la otra rama? (borra la tuya y los marcadores)
- ¿Combino ambas versiones? (edita a mano y borra los marcadores)

**Paso 3: Confirmar** — Cuando hayas resuelto todos los conflictos:
```bash
git add archivo-resuelto.txt
git commit    # Git ya prepara un mensaje de merge
```

### ⚠️ Importante: las reglas de oro

1. **No entres en pánico**. Un conflicto no es un error — es Git pidiendo
   tu opinión.

2. **Elimina TODOS los marcadores**. Si dejas `<<<<<<<`, `=======` o
   `>>>>>>>` en el archivo, tu código estará roto.

3. **Puedes abortar**. Si te sientes perdido:
   ```bash
   git merge --abort
   ```
   Esto te devuelve al estado exacto de antes del merge. Sin consecuencias.

4. **Prueba después de resolver**. Antes de hacer commit, asegúrate de que
   el resultado tiene sentido (léelo, ejecútalo si es código).

### Herramientas para resolver conflictos

Puedes resolver conflictos con cualquier editor de texto, pero hay
herramientas que facilitan la tarea:

- **VS Code / Cursor**: Detecta los marcadores y muestra botones como
  "Accept Current Change", "Accept Incoming Change", "Accept Both Changes".
- **`git mergetool`**: Abre una herramienta visual de comparación (vimdiff,
  meld, kdiff3, etc.).

Para este curso, lo haremos a mano con un editor de texto para que entiendas exactamente qué ocurre.

💡 **Recomendación Pro**: Configura el estilo de conflicto `diff3` para ver también el contenido del ancestro común (la base), lo cual ayuda mucho a entender qué cambió en cada rama:
```bash
git config --global merge.conflictstyle diff3
```

---

## 💻 Práctica

### Preparación: Crear un conflicto a propósito

Vamos a crear un conflicto deliberadamente para aprender a resolverlo.

**Paso 1**: Asegúrate de estar en `main` y con el directorio limpio:

```bash
git switch main
git status
```

**Paso 2**: Modifica el precio del spaghetti en `main`:

Abre `proyecto/menu.txt` y cambia la línea del spaghetti. Puedes hacerlo
con `sed` o con tu editor:

```bash
sed -i 's/Spaghetti carbonara — 12.00€/Spaghetti carbonara — 13.50€/' proyecto/menu.txt
```

Confirma el cambio:

```bash
git add proyecto/menu.txt
git commit -m "Sube precio del spaghetti a 13.50€ en main"
```

**Paso 3**: Crea una rama que también cambie el precio del spaghetti,
pero partiendo del commit ANTERIOR:

```bash
git switch -c fix/precio-spaghetti HEAD~1
```

(Esto crea la rama desde el commit anterior al que acabas de hacer.)

```bash
sed -i 's/Spaghetti carbonara — 12.00€/Spaghetti carbonara — 14.00€/' proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Sube precio del spaghetti a 14.00€ en fix"
```

**Paso 4**: Verifica la divergencia:

```bash
git log --oneline --graph --all -6
```

✅ Deberías ver que `main` y `fix/precio-spaghetti` modificaron el mismo
archivo de formas diferentes.

---

### Ejercicio 1: Provoca el conflicto

```bash
git switch main
git merge fix/precio-spaghetti
```

✅ **Resultado esperado**:

```
Auto-merging proyecto/menu.txt
CONFLICT (content): Merge conflict in proyecto/menu.txt
Automatic merge failed; fix conflicts and then commit the result.
```

¡Tu primer conflicto! No pasa nada malo. Git te está pidiendo ayuda.

---

### Ejercicio 2: Examina el estado

```bash
git status
```

✅ **Resultado esperado**:

```
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   proyecto/menu.txt
```

💡 `both modified` confirma que ambas ramas tocaron ese archivo.

---

### Ejercicio 3: Mira los marcadores de conflicto

```bash
cat proyecto/menu.txt
```

✅ **Resultado esperado**: En algún lugar del archivo verás:

```
<<<<<<< HEAD
- Spaghetti carbonara — 13.50€
=======
- Spaghetti carbonara — 14.00€
>>>>>>> fix/precio-spaghetti
```

💡 `HEAD` (tu rama actual, `main`) quiere 13.50€. La otra rama quiere
14.00€. Git no sabe cuál elegir.

---

### Ejercicio 4: Resuelve el conflicto

Decide que el precio correcto es **14.00€**. Edita `proyecto/menu.txt` y
reemplaza todo el bloque de conflicto:

```bash
sed -i '/<<<<<<< HEAD/,/>>>>>>> fix\/precio-spaghetti/c\- Spaghetti carbonara — 14.00€' proyecto/menu.txt
```

O abre el archivo en tu editor y manualmente:
1. Borra la línea `<<<<<<< HEAD`
2. Borra la línea `- Spaghetti carbonara — 13.50€`
3. Borra la línea `=======`
4. Deja la línea `- Spaghetti carbonara — 14.00€`
5. Borra la línea `>>>>>>> fix/precio-spaghetti`

Verifica que quedó limpio:

```bash
cat proyecto/menu.txt
```

✅ No debe haber ningún marcador (`<<<<<<<`, `=======`, `>>>>>>>`).
Solo el contenido final que decidiste.

---

### Ejercicio 5: Completa el merge

```bash
git add proyecto/menu.txt
git status
```

✅ `proyecto/menu.txt` ya no aparece como "unmerged". Está en verde.

```bash
git commit -m "Resuelve conflicto de precio: spaghetti a 14.00€"
```

✅ **Resultado esperado**: El merge se completa con tu resolución.

Verifica el historial:

```bash
git log --oneline --graph -5
```

✅ Ves el commit de merge con las dos ramas convergiendo.

---

### Ejercicio 6: Limpia

```bash
git branch -d fix/precio-spaghetti
```

---

### Ejercicio 7: Practica abortar un merge

Vamos a crear otro conflicto pero esta vez lo abortamos:

```bash
# Crea el conflicto
git switch -c fix/precio-pizza HEAD~1
sed -i 's/Pizza margarita — 10.50€/Pizza margarita — 12.00€/' proyecto/menu.txt 2>/dev/null || sed -i 's/Pizza margarita — 11.00€/Pizza margarita — 12.00€/' proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Sube precio de pizza a 12.00€"

git switch main
sed -i 's/Pizza margarita — 10.50€/Pizza margarita — 11.50€/' proyecto/menu.txt 2>/dev/null || sed -i 's/Pizza margarita — 11.00€/Pizza margarita — 11.50€/' proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Ajusta precio de pizza a 11.50€"

# Intenta mergear
git merge fix/precio-pizza
```

Si hay conflicto, aborta:

```bash
git status
git merge --abort
git status
```

✅ **Resultado esperado**: Después del abort, `git status` muestra
"nothing to commit, working tree clean". Es como si nunca hubieras
intentado el merge.

💡 `git merge --abort` es tu red de seguridad. Úsalo siempre que
te sientas perdido durante un conflicto.

Limpia la rama:

```bash
git branch -D fix/precio-pizza
```

(Usamos `-D` en lugar de `-d` porque la rama no fue mergeada.)

---

### Ejercicio 8: Conflicto donde combinas ambas versiones

Crea un escenario donde la mejor solución es combinar ambos cambios:

```bash
# Rama A: añade un plato
git switch -c feature/plato-chef
echo "- Lomo a la Wellington — 22.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega lomo Wellington al menú"

# Vuelve a main y añade OTRO plato en el mismo lugar
git switch main
echo "- Lubina al horno — 18.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega lubina al horno al menú"

# Mergea
git merge feature/plato-chef
```

✅ Si hay conflicto al final del archivo, resuélvelo manteniendo
**AMBOS platos**:

```
- Lubina al horno — 18.00€
- Lomo a la Wellington — 22.00€
```

(Borra los marcadores y deja las dos líneas.)

```bash
git add proyecto/menu.txt
git commit -m "Merge: incorpora lomo Wellington junto a lubina"
git branch -d feature/plato-chef
```

💡 No siempre es "una versión u otra". A veces la solución correcta es
combinar el contenido de ambas ramas.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git merge rama` | Intenta mergear (puede dar conflicto) |
| `git status` | Muestra archivos en conflicto ("Unmerged paths") |
| `git diff` | Muestra los marcadores de conflicto |
| `git add archivo` | Marca el conflicto como resuelto |
| `git commit` | Completa el merge después de resolver |
| `git merge --abort` | Cancela el merge y vuelve al estado anterior |

### Marcadores de conflicto

```
<<<<<<< HEAD          ← Tu rama (la que recibe)
  tu versión
=======               ← Separador
  la otra versión
>>>>>>> otra-rama     ← La rama que mergeas
```

### Flujo de resolución

```
1. git merge rama         → CONFLICT
2. git status             → ver archivos afectados
3. Editar archivo         → quitar marcadores, elegir contenido
4. git add archivo        → marcar como resuelto
5. git commit             → completar el merge
```

**Regla de oro**: Los conflictos no son un error — son una pregunta.
`git merge --abort` siempre está ahí si necesitas empezar de nuevo.

---

> **Siguiente lección**: `05_ejercicio_final.md` — Un
> ejercicio integrador que pone a prueba todo lo aprendido sobre ramas.
