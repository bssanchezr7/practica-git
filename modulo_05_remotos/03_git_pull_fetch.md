# Lección 03: `git pull` y `git fetch` — Traer cambios del remoto

## 📖 Teoría

### El problema: tu repo local se queda atrás

Mientras trabajas en tu copia local, otras personas (o tú mismo desde otra
máquina) pueden pushear cambios al remoto. Según [Pro Git (Capítulo 2.5)](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes#_fetching_and_pulling_your_remotes), tu repo local no se actualiza solo — necesitas pedirle explícitamente que traiga los cambios.

```
  Tu local:       C1 ── C2 ── C3         (te quedaste aquí)
  El remoto:      C1 ── C2 ── C3 ── C4 ── C5   (avanzó)

  Necesitas traer C4 y C5 a tu local.
```

### Dos formas de traer cambios: `fetch` vs `pull`

```
                   git fetch                    git pull
               ┌──────────────┐          ┌──────────────────┐
               │ Descarga los │          │ Descarga los     │
               │ cambios del  │          │ cambios del      │
               │ remoto       │          │ remoto           │
               │              │          │        +         │
               │ NO modifica  │          │ Los integra      │
               │ tu Working   │          │ (merge) en tu    │
               │ Directory    │          │ rama actual      │
               └──────────────┘          └──────────────────┘
```

| Aspecto | `git fetch` | `git pull` |
|---------|------------|-----------|
| Descarga cambios | ✅ Sí | ✅ Sí |
| Modifica tu rama local | ❌ No | ✅ Sí (merge) |
| Riesgo de conflictos | Ninguno | Posible |
| Seguridad | Máxima | Media |
| Uso típico | Revisar antes de integrar | Actualizar rápido |

### `git fetch` en detalle

`git fetch` descarga los commits nuevos del remoto y actualiza las
**ramas remotas** (como `origin/main`), pero NO toca tu rama local
ni tu Working Directory.

```
  ANTES de fetch:
  Local:  C1 ── C2 ── C3  (main)
  origin/main: C1 ── C2 ── C3

  Remoto: C1 ── C2 ── C3 ── C4 ── C5

  DESPUÉS de fetch:
  Local:  C1 ── C2 ── C3  (main)          ← NO cambió
  origin/main: C1 ── C2 ── C3 ── C4 ── C5  ← Actualizado

  Tu rama "main" sigue en C3. Pero origin/main ya tiene C4 y C5.
  Puedes inspeccionar los cambios antes de integrarlos.
```

Después del fetch, puedes:

```bash
git log main..origin/main    # Ver qué commits nuevos hay
git diff main origin/main    # Ver qué cambió exactamente
git merge origin/main        # Integrar cuando estés listo
```

### `git pull` en detalle

`git pull` es equivalente a `git fetch` + `git merge`:

```bash
git pull origin main
# Es lo mismo que:
git fetch origin
git merge origin/main
```

```
  ANTES de pull:
  Local:  C1 ── C2 ── C3  (main)
  Remoto: C1 ── C2 ── C3 ── C4 ── C5

  DESPUÉS de pull:
  Local:  C1 ── C2 ── C3 ── C4 ── C5  (main)
  (Si fue fast-forward)

  O si había divergencia:
  Local:  C1 ── C2 ── C3 ── C6 ── M  (main)
                        \         /
                         C4 ── C5
  (Three-way merge con merge commit M)
```

### Sintaxis

```bash
# Fetch
git fetch                    # Trae cambios de todos los remotos
git fetch origin             # Solo de origin
git fetch origin main        # Solo la rama main de origin

# Pull
git pull                     # Pull de la rama upstream configurada
git pull origin main         # Pull explícito de origin/main

# Pull con rebase (alternativa al merge)
git pull --rebase origin main
```

### `git pull --rebase` vs `git pull` (merge)

Cuando tu rama local y el remoto han divergido:

```
  git pull (merge):                 git pull --rebase:

  C1 ── C2 ── C3 ── M              C1 ── C2 ── C4 ── C5 ── C3'
          \        /                (tu commit C3 se "replanta"
           C4 ── C5                  encima de los cambios remotos)
  (crea merge commit)              (historial lineal)
```

| Opción | Historial | Cuándo usarla |
|--------|-----------|---------------|
| `git pull` (merge) | Muestra la divergencia | Por defecto, más seguro |
| `git pull --rebase` | Lineal, más limpio | Cuando quieres historial limpio |

💡 **Configuración recomendada**: Muchos profesionales prefieren que `pull` haga rebase por defecto para evitar "commits de merge" innecesarios que ensucian el historial:
```bash
git config --global pull.rebase true
```

### Ramas de seguimiento (tracking branches)

Cuando haces `git push -u` o `git clone`, Git crea una relación entre
tu rama local y la remota:

```
  main (local) ──tracks──▶ origin/main (remota)
```

Gracias a esto, `git pull` y `git push` saben de dónde traer y a dónde
enviar sin que lo especifiques.

```bash
git branch -vv    # Muestra qué rama remota rastrea cada rama local
```

### ⚠️ Importante

- `git fetch` es **siempre seguro**: nunca modifica tu código.
- `git pull` puede causar conflictos si tú y el remoto modificaron las
  mismas líneas.
- Si un `git pull` genera conflictos, resuélvelos igual que en un merge
  normal (Módulo 4, lección 04).

---

## 💻 Práctica

> **Prerrequisito**: Debes tener `origin` configurado y haber pusheado
> al menos una vez (lección 02 de este módulo).

### Ejercicio 1: Simula cambios en el remoto

Crea cambios "de otro desarrollador" en el remoto:

```bash
git clone /tmp/remoto_simulado.git /tmp/otro_dev
cd /tmp/otro_dev
echo "" >> proyecto/inventario.txt
echo "## Pedido urgente" >> proyecto/inventario.txt
echo "- Limones: 10 kg (para el fin de semana)" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega pedido urgente de limones"
git push origin main
cd ~/practica_git
```

---

### Ejercicio 2: Usa `git fetch` para ver qué cambió

```bash
git fetch origin
```

✅ Git descarga los cambios, pero tu Working Directory NO cambia.

```bash
git status
git log --oneline -3
```

✅ Tu rama `main` sigue igual.

Ahora inspecciona qué trajo el fetch:

```bash
git log main..origin/main --oneline
```

✅ Ves el commit del "otro desarrollador".

```bash
git diff main origin/main
```

✅ Ves exactamente qué líneas cambiaron.

---

### Ejercicio 3: Integra los cambios con merge

Después de revisar que todo está bien:

```bash
git merge origin/main
```

✅ Tu rama `main` ahora incluye el commit del pedido de limones.

```bash
cat proyecto/inventario.txt
```

✅ El pedido de limones está ahí.

💡 Este flujo (fetch → revisar → merge) es el más seguro porque
puedes inspeccionar los cambios antes de integrarlos.

---

### Ejercicio 4: Usa `git pull` (el atajo)

Simula otro cambio remoto:

```bash
cd /tmp/otro_dev
git pull
echo "- Naranjas: 8 kg" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega naranjas al pedido urgente"
git push origin main
cd ~/practica_git
```

Ahora usa pull (fetch + merge en un paso):

```bash
git pull
```

✅ **Resultado esperado**: Git descarga e integra los cambios en un solo
comando:

```
Updating abc1234..def5678
Fast-forward
 proyecto/inventario.txt | 1 +
 1 file changed, 1 insertion(+)
```

---

### Ejercicio 5: Pull con divergencia

Simula una divergencia — cambios en ambos lados:

```bash
# Cambio remoto
cd /tmp/otro_dev
git pull
echo "## Nota del proveedor" >> proyecto/inventario.txt
echo "Entrega estimada: jueves a las 10:00" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega nota del proveedor"
git push origin main

# Cambio local (sin pullear primero)
cd ~/practica_git
echo "" >> proyecto/menu.txt
echo "## Información adicional" >> proyecto/menu.txt
echo "Wi-Fi gratuito para clientes" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega información de Wi-Fi al menú"
```

Ahora haz pull:

```bash
git pull
```

✅ Git hace un three-way merge porque ambos lados avanzaron. Como
tocaron archivos diferentes, no hay conflicto.

```bash
git log --oneline --graph -5
```

✅ Ves el merge commit uniendo ambas líneas.

---

### Ejercicio 6: Verifica las ramas de seguimiento

```bash
git branch -vv
```

✅ Ves que `main` rastrea `origin/main` y si estás adelante, atrás o
sincronizado.

Sincroniza:

```bash
git push
```

Limpieza:

```bash
rm -rf /tmp/otro_dev
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git fetch` | Descarga cambios sin integrar |
| `git fetch origin` | Descarga solo de origin |
| `git pull` | Descarga + merge (fetch + merge) |
| `git pull --rebase` | Descarga + rebase (historial lineal) |
| `git log main..origin/main` | Ver commits nuevos del remoto |
| `git branch -vv` | Ver relaciones de seguimiento |

**Flujo seguro**: `git fetch` → revisar → `git merge origin/main`.
**Flujo rápido**: `git pull`.

---

> **Siguiente lección**: `04_ejercicio_final.md` — Ejercicio integrador
> que simula un flujo completo de colaboración con remotos.
