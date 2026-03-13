# Lección 02: Flujos de trabajo en equipo

## 📖 Teoría

### ¿Qué es un flujo de trabajo (workflow)?

Un flujo de trabajo define **cómo un equipo usa las ramas** para organizar el desarrollo. Según [Pro Git (Capítulo 5.1)](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows), no hay uno perfecto — depende del tamaño del equipo y de la confianza entre sus miembros.

### Los tres flujos más populares

---

### 1. Feature Branch Workflow (el más común)

Cada funcionalidad se desarrolla en su propia rama. Cuando está lista,
se integra en `main`.

```
  main ──────────────────────────────── main
         \                           /
          feature/menu ── C1 ── C2 ──
         \                               /
          feature/inventario ── C3 ── C4 ──

  Regla: main SIEMPRE funciona. Todo cambio va en una rama.
```

**Flujo**:
1. `git switch -c feature/mi-cambio` — Crea rama desde main.
2. Trabaja y haz commits en la rama.
3. `git push -u origin feature/mi-cambio` — Pushea la rama.
4. Abre un **Pull Request** (PR) para revisión.
5. Tras aprobación, mergea a main.
6. Borra la rama.

**Ideal para**: Equipos de cualquier tamaño. Simple y efectivo.

---

### 2. Git Flow

Un flujo más estructurado con ramas dedicadas:

```
  main ────────────────────────────────── main
    │                                      ↑
    │   develop ── ── ── ── ── ── ── merge│
    │      \                        /      │
    │       feature/x ── C1 ── C2 ──      │
    │      \                        /      │
    │       feature/y ── C3 ──────        │
    │                                      │
    │   release/1.0 ── fix ── fix ────────│
    │                                      │
    │   hotfix/urgente ──────────────────│
```

| Rama | Propósito | Vida |
|------|-----------|------|
| `main` | Código en producción | Permanente |
| `develop` | Integración de features | Permanente |
| `feature/*` | Nuevas funcionalidades | Temporal |
| `release/*` | Preparar un release | Temporal |
| `hotfix/*` | Correcciones urgentes | Temporal |

**Ideal para**: Proyectos con releases planificados (apps móviles,
software empresarial).

---

### 3. Trunk-Based Development

Todos trabajan directamente en `main` (trunk) con ramas muy cortas
(horas, no días).

```
  main ── C1 ── C2 ── C3 ── C4 ── C5 ── C6 ── C7
                 \  /         \  /
                 feat         fix
              (1-2 commits)  (1 commit)
```

**Reglas**:
- Las ramas viven **máximo 1-2 días**.
- Commits pequeños y frecuentes.
- Se usa integración continua (CI) para verificar cada cambio.

**Ideal para**: Equipos con buena cobertura de tests y CI/CD.

### Comparación

| Aspecto | Feature Branch | Git Flow | Trunk-Based |
|---------|---------------|----------|-------------|
| Complejidad | Baja | Alta | Baja |
| Ramas activas | Pocas | Muchas | Muy pocas |
| Frecuencia de merge | Media | Baja | Alta |
| Ideal para | Mayoría | Releases formales | Equipos ágiles |

### Pull Requests (PR) / Merge Requests (MR)

Independientemente del flujo, los **Pull Requests** son la forma estándar
de integrar cambios en equipo:

1. Haces push de tu rama al remoto.
2. Abres un PR en GitHub/GitLab.
3. Tus compañeros revisan el código.
4. Se discuten los cambios, se piden ajustes.
5. Cuando está aprobado, se mergea.

**Beneficios**:
- Revisión de código antes de integrar.
- Discusión documentada.
- Se pueden vincular a tareas/issues.
- Se ejecutan tests automáticos.

### Recomendaciones universales

1. **`main` siempre debe funcionar**. Nunca commitees código roto ahí.
2. **Una rama por tarea**. No mezcles varias funcionalidades en una rama.
3. **Commits pequeños y frecuentes**. Mejor 10 commits pequeños que 1
   gigante.
4. **Pull antes de push**. Mantente sincronizado con el remoto.
5. **Borra las ramas mergeadas**. Mantén el repositorio limpio.

---

## 💻 Práctica

### Ejercicio 1: Simula un Feature Branch Workflow

```bash
cd ~/practica_git

# 1. Crea una rama de feature
git switch -c feature/menu-brunch

# 2. Trabaja en ella
echo "" >> proyecto/menu.txt
echo "## Brunch dominical (10:00 - 14:00)" >> proyecto/menu.txt
echo "- Tostadas con aguacate — 8.50€" >> proyecto/menu.txt
echo "- Huevos Benedict — 10.00€" >> proyecto/menu.txt
echo "- Zumo natural — 4.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "feat: agrega menú de brunch dominical"

# 3. Vuelve a main y mergea
git switch main
git merge --no-ff feature/menu-brunch -m "Merge: incorpora menú de brunch dominical"

# 4. Limpia
git branch -d feature/menu-brunch

# 5. Verifica
git log --oneline --graph -5
```

✅ Ves la bifurcación y unión gracias a `--no-ff`.

---

### Ejercicio 2: Simula trabajo en paralelo con dos features

```bash
# Feature A: carta de cócteles
git switch -c feature/cocteles
echo "" >> proyecto/menu.txt
echo "## Cócteles" >> proyecto/menu.txt
echo "- Mojito — 8.00€" >> proyecto/menu.txt
echo "- Margarita — 9.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "feat: agrega carta de cócteles"

# Feature B: reorganizar inventario (vuelve a main para crear esta rama)
git switch main
git switch -c feature/reorganizar-inventario
echo "" >> proyecto/inventario.txt
echo "## Clasificación por temperatura" >> proyecto/inventario.txt
echo "### Refrigerados" >> proyecto/inventario.txt
echo "- Todos los lácteos y carnes" >> proyecto/inventario.txt
echo "### Ambiente" >> proyecto/inventario.txt
echo "- Productos secos, enlatados y bebidas" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "feat: reorganiza inventario por temperatura"

# Integra ambas en main
git switch main
git merge --no-ff feature/cocteles -m "Merge: incorpora carta de cócteles"
git merge --no-ff feature/reorganizar-inventario -m "Merge: reorganiza inventario"

# Limpia
git branch -d feature/cocteles
git branch -d feature/reorganizar-inventario

# Verifica
git log --oneline --graph -8
```

✅ El historial muestra dos features desarrolladas en paralelo e
integradas a main.

---

### Ejercicio 3: Simula un hotfix urgente

Mientras trabajas en una feature, llega una urgencia:

```bash
# Estás trabajando en una feature
git switch -c feature/nueva-seccion
echo "Trabajo en progreso..." >> proyecto/menu.txt
git stash push -m "WIP: nueva sección"

# Urgencia: hay un error en producción
git switch main
git switch -c hotfix/precio-erroneo
echo "NOTA: Precio de pizza corregido" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "hotfix: corrige precio de pizza"

# Integra el hotfix
git switch main
git merge --no-ff hotfix/precio-erroneo -m "Merge: hotfix precio pizza"
git branch -d hotfix/precio-erroneo

# Vuelve a tu feature
git switch feature/nueva-seccion
git stash pop
```

Limpia:

```bash
git restore proyecto/menu.txt
git switch main
git branch -d feature/nueva-seccion
```

---

## 🧠 Resumen

| Flujo | Ramas | Ideal para |
|-------|-------|------------|
| Feature Branch | `main` + `feature/*` | La mayoría de equipos |
| Git Flow | `main` + `develop` + `feature/*` + `release/*` + `hotfix/*` | Releases formales |
| Trunk-Based | `main` + ramas muy cortas | Equipos con CI/CD |

**Regla de oro**: Elige un flujo, asegúrate de que todo el equipo lo
entienda, y síguelo consistentemente.

---

> **Siguiente lección**: `03_consejos_profesionales.md` — Consejos y
> trucos que marcan la diferencia entre un usuario básico y un profesional.
