# Lección 04: `git log` — Explorar el historial de commits

## 📖 Teoría

### ¿Qué hace `git log`?

`git log` te muestra el **historial de commits** de tu proyecto. Es como leer
el diario de a bordo de un barco: cada entrada registra qué pasó, cuándo y
quién lo hizo.

### La salida por defecto

Cuando ejecutas `git log` sin opciones, ves algo como:

```
commit a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0 (HEAD -> master)
Author: Estudiante <estudiante@curso-git.local>
Date:   Fri Feb 20 12:00:00 2026 +0100

    Añade agua mineral al inventario

commit b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1
Author: Estudiante <estudiante@curso-git.local>
Date:   Fri Feb 20 11:30:00 2026 +0100

    Agrega ensalada César al menú y actualiza inventario

commit c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2
...
```

Cada entrada muestra:
1. **Hash del commit** (identificador único).
2. **Autor** (nombre y email).
3. **Fecha**.
4. **Mensaje del commit**.

### HEAD y las referencias

En la salida verás `(HEAD -> master)`. Esto significa:

- **HEAD**: Es un puntero que indica "dónde estás ahora" en el historial.
- **master**: Es el nombre de la rama actual.

```
  Commit 1 ← Commit 2 ← Commit 3 ← Commit 4
                                        ↑
                                   HEAD → master
                                   (Estás aquí)
```

### Opciones útiles de `git log`

Git log tiene decenas de opciones. Estas son las más prácticas:

#### Formato compacto: `--oneline`

```bash
git log --oneline
```

Muestra cada commit en **una sola línea**: hash corto + mensaje.

```
a1b2c3d Añade agua mineral al inventario
b2c3d4e Agrega ensalada César al menú y actualiza inventario
c3d4e5f Agrega platos nuevos al menú
d4e5f6a Inicializa el curso con estructura y archivos de práctica
```

💡 Esta es probablemente la opción que más usarás en el día a día.

#### Limitar cantidad: `-n`

```bash
git log -3          # Muestra solo los últimos 3 commits
git log --oneline -5  # Últimos 5, en formato compacto
```

#### Gráfico visual: `--graph`

```bash
git log --oneline --graph
```

Dibuja líneas que muestran cómo se relacionan los commits. Es especialmente
útil cuando hay ramas (lo verás en el futuro):

```
* a1b2c3d Añade agua mineral al inventario
* b2c3d4e Agrega ensalada César al menú y actualiza inventario
* c3d4e5f Agrega platos nuevos al menú
* d4e5f6a Inicializa el curso con estructura y archivos de práctica
```

#### Filtrar por autor: `--author`

```bash
git log --author="Estudiante"
```

Muestra solo los commits de un autor específico.

#### Filtrar por fecha: `--since` y `--until`

```bash
git log --since="2026-02-20"          # Desde esta fecha
git log --since="2 hours ago"         # Últimas 2 horas
git log --since="yesterday"           # Desde ayer
git log --until="2026-02-19"          # Hasta esta fecha
```

#### Filtrar por mensaje: `--grep`

```bash
git log --grep="menú"                 # Commits cuyo mensaje contenga "menú"
```

#### Ver qué archivos se modificaron: `--stat`

```bash
git log --stat
```

Añade un resumen de archivos cambiados y líneas añadidas/eliminadas:

```
commit a1b2c3d...
    Añade agua mineral al inventario

 proyecto/inventario.txt | 1 +
 1 file changed, 1 insertion(+)
```

#### Ver el contenido exacto de los cambios: `-p`

```bash
git log -p
```

Muestra el **diff completo** de cada commit (qué líneas se añadieron y
cuáles se eliminaron). Es muy detallado — combínalo con `-n` para limitar.

```bash
git log -p -1        # Diff del último commit solamente
```

#### Formato personalizado: `--pretty=format`

Para los más curiosos, puedes personalizar completamente la salida:

```bash
git log --pretty=format:"%h - %an, %ar : %s"
```

Produce:

```
a1b2c3d - Estudiante, hace 5 minutos : Añade agua mineral al inventario
b2c3d4e - Estudiante, hace 10 minutos : Agrega ensalada César al menú...
```

Códigos comunes:
- `%h` = hash corto
- `%an` = nombre del autor
- `%ar` = fecha relativa (hace X minutos)
- `%s` = mensaje del commit

### ⚠️ Importante

- `git log` **no modifica nada**. Es solo lectura.
- Si el historial es largo, Git abre un **paginador** (normalmente `less`).
  Usa las flechas para navegar, `q` para salir.
- Los commits se muestran del más reciente al más antiguo.

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado los ejercicios de la lección 03.
> Si los completaste, ya tienes al menos 4 commits en tu historial.

### Ejercicio 1: Explora tu historial

```bash
git log
```

✅ **Resultado esperado**: Ves todos tus commits, del más reciente al más
antiguo, con hash, autor, fecha y mensaje completos.

💡 Si la salida es larga, usa `↑` `↓` para navegar y `q` para salir.

---

### Ejercicio 2: Vista compacta con `--oneline`

```bash
git log --oneline
```

✅ **Resultado esperado**: Cada commit en una línea. Mucho más fácil de leer
cuando tienes muchos commits.

💡 Guarda este comando en tu memoria: lo usarás constantemente.

---

### Ejercicio 3: Limita los resultados

```bash
git log --oneline -2
```

✅ **Resultado esperado**: Solo los últimos 2 commits.

---

### Ejercicio 4: Ve los archivos modificados

```bash
git log --stat --oneline
```

✅ **Resultado esperado**: Cada commit muestra qué archivos se tocaron y
cuántas líneas cambiaron.

---

### Ejercicio 5: Busca en los mensajes

```bash
git log --grep="menú" --oneline
```

✅ **Resultado esperado**: Solo aparecen los commits cuyo mensaje contiene
la palabra "menú".

---

### Ejercicio 6: Ve los cambios exactos de un commit

```bash
git log -p -1
```

✅ **Resultado esperado**: Ves el diff completo del último commit — las
líneas añadidas aparecen con `+` en verde.

---

### Ejercicio 7: El gráfico visual

```bash
git log --oneline --graph --all
```

✅ **Resultado esperado**: Por ahora verás una línea recta de asteriscos
(porque no hay ramas). Cuando aprendas a crear ramas, este comando cobra
vida con bifurcaciones y merges.

---

### Ejercicio 8: Crea tu alias favorito

Este comando combina las opciones más útiles. Ejecútalo:

```bash
git log --oneline --graph --all --decorate
```

Si te gusta, puedes crear un alias para no escribirlo siempre:

```bash
git config alias.historia "log --oneline --graph --all --decorate"
```

Ahora puedes usar:

```bash
git historia
```

✅ **Resultado esperado**: La misma salida bonita, con un comando corto.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git log` | Historial completo |
| `git log --oneline` | Una línea por commit |
| `git log -n` | Últimos n commits |
| `git log --stat` | Archivos modificados por commit |
| `git log -p` | Diff completo de cada commit |
| `git log --graph` | Gráfico visual de ramas |
| `git log --grep="texto"` | Buscar en mensajes de commit |
| `git log --author="nombre"` | Filtrar por autor |
| `git log --since="fecha"` | Filtrar por fecha |

**Regla de oro**: `git log --oneline` es tu vista rápida. Cuando necesites
más detalle, añade `--stat` o `-p`.

---

> **Siguiente lección**: `lecciones/05_git_diff.md` — Aprenderás a ver
> exactamente qué cambió entre versiones.
