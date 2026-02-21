# Lección 11: Ejercicio Final Integrador — Ramas

## Objetivo

Simular un flujo de trabajo profesional con ramas: planificar funcionalidades,
desarrollarlas en paralelo, integrarlas y resolver conflictos. Todo en el
contexto de nuestro restaurante "El Buen Código".

## ⚠️ Reglas del ejercicio

1. **Nunca trabajes directamente en `main`**. Todos los cambios se hacen
   en ramas.
2. Usa `git status` y `git log --oneline --graph --all` frecuentemente
   para visualizar el estado.
3. Haz commits con mensajes descriptivos.
4. Limpia las ramas después de mergearlas.

---

## Escenario

El restaurante "El Buen Código" quiere hacer tres mejoras simultáneamente:

1. **Renovar el menú de entrantes** (nueva sección).
2. **Crear un menú de mediodía** económico.
3. **Reorganizar el inventario** por proveedor.

Tres tareas independientes que tres personas podrían hacer en paralelo.
Tú vas a simular las tres usando ramas.

---

## Tarea 1: Preparación

Verifica tu punto de partida:

```bash
git switch main
git status
git log --oneline -3
```

✅ Debes estar en `main` con el directorio limpio.

---

## Tarea 2: Rama para renovar entrantes

```bash
git switch -c feature/nuevos-entrantes
```

Edita `proyecto/menu.txt` y **reemplaza** la sección de entrantes actual
con esta versión mejorada. Abre el archivo en tu editor y sustituye la
sección "## Entrantes" por:

```
## Entrantes
- Croquetas de jamón ibérico (8 uds.) — 9.00€
- Gazpacho andaluz — 6.50€
- Patatas bravas con alioli — 6.00€
- Hummus con crudités — 7.00€
- Tabla de quesos artesanos — 12.00€
```

Confirma los cambios:

```bash
git diff
git add proyecto/menu.txt
git diff --staged
git commit -m "Renueva la sección de entrantes con opciones premium"
```

Verifica:

```bash
git log --oneline --graph --all -5
```

---

## Tarea 3: Rama para menú del mediodía

Vuelve a `main` y crea otra rama:

```bash
git switch main
git switch -c feature/menu-mediodia
```

Crea un archivo nuevo `proyecto/menu_mediodia.txt`:

```bash
cat > proyecto/menu_mediodia.txt << 'EOF'
# Menú del Mediodía — Restaurante "El Buen Código"
# Precio: 12.50€ (entrante + principal + bebida)
# Disponible de lunes a viernes, 12:00 - 16:00

## Entrantes (elige uno)
- Sopa del día
- Ensalada de la casa
- Croquetas (4 uds.)

## Principales (elige uno)
- Pollo al horno con patatas
- Pasta del día
- Merluza a la plancha con verduras

## Bebidas (incluida)
- Agua
- Refresco
- Copa de vino de la casa
EOF
```

También añade una referencia en `proyecto/menu.txt`. Abre el archivo y
añade al final:

```bash
echo "" >> proyecto/menu.txt
echo "---" >> proyecto/menu.txt
echo "Consulte nuestro menú del mediodía (12.50€) de lunes a viernes." >> proyecto/menu.txt
```

Confirma ambos cambios en un commit:

```bash
git status
git add proyecto/menu_mediodia.txt proyecto/menu.txt
git diff --staged
git commit -m "Crea menú del mediodía con referencia en el menú principal"
```

---

## Tarea 4: Rama para reorganizar inventario

```bash
git switch main
git switch -c feature/inventario-proveedores
```

Edita `proyecto/inventario.txt`. Reorganiza el contenido añadiendo al
final una sección de proveedores:

```bash
cat >> proyecto/inventario.txt << 'EOF'

## Proveedores
- "Frutas García" — Verduras y frutas frescas (entregas lunes y jueves)
- "Lácteos del Norte" — Productos lácteos (entregas martes y viernes)
- "Distribuciones Martínez" — Productos secos y enlatados (entregas miércoles)
- "Bodegas Ruiz" — Vinos y bebidas (entregas bajo pedido)
EOF
```

```bash
git add proyecto/inventario.txt
git commit -m "Añade sección de proveedores al inventario"
```

---

## Tarea 5: Visualiza las tres ramas

```bash
git log --oneline --graph --all
```

✅ **Resultado esperado**: Deberías ver tres ramas divergiendo desde `main`:

```
* aaaa (feature/inventario-proveedores) Añade sección de proveedores...
| * bbbb (feature/menu-mediodia) Crea menú del mediodía...
|/
| * cccc (feature/nuevos-entrantes) Renueva la sección de entrantes...
|/
* dddd (HEAD -> main) ...
```

💡 Tres líneas de trabajo independientes, ninguna afecta a las otras.

---

## Tarea 6: Integra la primera rama (fast-forward)

Mergea primero los nuevos entrantes (debería ser fast-forward):

```bash
git switch main
git merge feature/nuevos-entrantes
```

✅ Debería ser un fast-forward limpio.

```bash
git log --oneline --graph --all -5
git branch -d feature/nuevos-entrantes
```

---

## Tarea 7: Integra la segunda rama (three-way merge)

```bash
git merge feature/menu-mediodia
```

✅ Ahora `main` ha avanzado, así que será un three-way merge. Git puede
que combine los cambios automáticamente ya que tocan diferentes partes
del archivo, o puede generar un conflicto si ambas ramas modificaron las
mismas líneas.

**Si hay conflicto**: Resuélvelo usando lo aprendido en la lección 10:
1. `git status` para identificar archivos.
2. Abre el archivo, decide qué mantener, elimina marcadores.
3. `git add` + `git commit`.

**Si no hay conflicto**: Git crea el merge commit automáticamente.

```bash
git log --oneline --graph --all -6
git branch -d feature/menu-mediodia
```

---

## Tarea 8: Integra la tercera rama

```bash
git merge feature/inventario-proveedores
```

✅ Este merge toca un archivo diferente (`inventario.txt`), así que
no debería haber conflictos.

```bash
git log --oneline --graph --all -8
git branch -d feature/inventario-proveedores
```

---

## Tarea 9: Verifica el resultado final

```bash
# Solo debe quedar main
git branch

# Mira el historial completo
git log --oneline --graph

# Verifica los contenidos
echo "=== MENÚ PRINCIPAL ==="
cat proyecto/menu.txt

echo ""
echo "=== MENÚ DEL MEDIODÍA ==="
cat proyecto/menu_mediodia.txt

echo ""
echo "=== INVENTARIO ==="
cat proyecto/inventario.txt
```

✅ **Resultado esperado**:
- `proyecto/menu.txt` tiene los entrantes renovados y la referencia al
  menú del mediodía.
- `proyecto/menu_mediodia.txt` existe con el menú completo.
- `proyecto/inventario.txt` tiene la sección de proveedores.
- El historial muestra las bifurcaciones y uniones.

---

## Tarea 10: Desafío extra — Simula un conflicto real

Para este desafío, crea una situación donde dos ramas modifiquen la misma
sección:

```bash
# Rama A: el chef quiere cambiar el plato del menú del mediodía
git switch -c fix/cambiar-pollo
# Edita menu_mediodia.txt: cambia "Pollo al horno" por "Pollo a la brasa"
sed -i 's/Pollo al horno con patatas/Pollo a la brasa con patatas asadas/' proyecto/menu_mediodia.txt
git add proyecto/menu_mediodia.txt
git commit -m "Cambia pollo al horno por pollo a la brasa"

# Rama B: el nutricionista quiere cambiar el mismo plato
git switch main
git switch -c fix/opcion-saludable
# Edita menu_mediodia.txt: cambia "Pollo al horno" por "Pechuga a la plancha"
sed -i 's/Pollo al horno con patatas/Pechuga a la plancha con ensalada/' proyecto/menu_mediodia.txt
git add proyecto/menu_mediodia.txt
git commit -m "Sustituye pollo al horno por opción saludable"
```

Ahora integra ambas ramas en `main`:

```bash
git switch main
git merge fix/cambiar-pollo     # Este debería ser limpio
git merge fix/opcion-saludable  # Este debería dar CONFLICTO
```

**Tu tarea**: Resuelve el conflicto decidiendo cuál es la mejor opción
(o combinando ambas si tiene sentido). Luego limpia las ramas.

---

## Autoevaluación

### Pregunta 1: ¿Cuántas ramas tienes activas?

```bash
git branch
```

✅ Solo `main`.

### Pregunta 2: ¿Tu historial refleja las bifurcaciones?

```bash
git log --oneline --graph -15
```

✅ Deberías ver líneas divergentes y convergentes.

### Pregunta 3: ¿El directorio está limpio?

```bash
git status
```

✅ "nothing to commit, working tree clean"

### Pregunta 4: ¿Cuántos merge commits tienes?

```bash
git log --oneline --merges
```

✅ Lista solo los commits de merge.

### Pregunta 5: ¿Todos los archivos están correctos?

Revisa manualmente que `menu.txt`, `menu_mediodia.txt` e `inventario.txt`
tengan el contenido esperado y sin marcadores de conflicto.

---

## 🏆 ¡Felicidades!

Has completado el Módulo 2: Ramas. Ahora sabes:

```
┌───────────────────────────────────────────────────────────┐
│                                                           │
│  ✅ Crear ramas para cada tarea    (git branch / -c)      │
│  ✅ Moverte entre ramas            (git switch)           │
│  ✅ Integrar trabajo terminado     (git merge)            │
│  ✅ Resolver conflictos con calma  (editar + add + commit)│
│  ✅ Limpiar ramas después de usar  (git branch -d)        │
│                                                           │
│  Tu flujo profesional:                                    │
│                                                           │
│  main ──────────────────────────────── main               │
│        \                            /                     │
│         feature ── C1 ── C2 ── merge                      │
│                                                           │
│  1. Crear rama desde main                                 │
│  2. Trabajar y hacer commits en la rama                   │
│  3. Volver a main y mergear                               │
│  4. Eliminar la rama                                      │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

### Próximos pasos sugeridos:

1. **Remotos** (`git push`, `git pull`, `git fetch`) — Colabora con otros
   y sube tu trabajo a GitHub/GitLab.
2. **Rebase** (`git rebase`) — Una alternativa al merge para mantener un
   historial lineal.
3. **Stash** (`git stash`) — Guarda cambios temporalmente sin commitear.

---

> "Las ramas son la superpotencia de Git. Úsalas sin miedo,
> crea una para cada idea, y mergea cuando estés seguro."
