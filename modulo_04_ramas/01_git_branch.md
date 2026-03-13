# Lección 01: `git branch` — Crear, listar y eliminar ramas

## 📖 Teoría

### ¿Qué es una rama?

Una rama es simplemente un **puntero móvil a un commit**. Nada más. Según [Pro Git (Capítulo 3.1)](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell), Git no copia archivos al crear una rama; simplemente crea un archivo de 41 bytes que contiene el hash del commit al que apunta. Esto las hace increíblemente rápidas y livianas.

Cuando creas una rama nueva, Git crea un nuevo puntero. Cuando haces un commit en esa rama, el puntero avanza automáticamente al nuevo commit.

### ¿Por qué existen las ramas?

Imagina que tu restaurante funciona perfectamente. Un día quieres probar un
menú experimental, pero no quieres arriesgar el menú principal que ya funciona.

**Sin ramas**: Modificas el menú directamente. Si el experimento sale mal,
tienes que recordar cómo era antes y revertir todo a mano.

**Con ramas**: Creas una rama `menu-experimental`, haces los cambios ahí.
Si funciona, lo unes al menú principal. Si no, simplemente borras la rama.
El menú principal nunca se tocó.

```
                          menu-experimental
                         /
  C1 ── C2 ── C3 ── C4      ← main (menú principal, intacto)
                         \
                          nueva-carta-vinos
```

### El modelo mental correcto

```
  ┌──────────────────────────────────────────────────────┐
  │                                                      │
  │  main       ─── C1 ── C2 ── C3 ── C4                │
  │                                     ↑                │
  │                                   HEAD               │
  │                                                      │
  │  Una rama es un puntero ──▶ apunta a un commit       │
  │  HEAD es un puntero ──▶ apunta a la rama activa      │
  │                                                      │
  └──────────────────────────────────────────────────────┘
```

- **main** (o master): La rama principal por defecto.
- **HEAD**: Un puntero especial que indica "en qué rama estás ahora". Según la documentación oficial, HEAD suele apuntar a la rama actual, la cual a su vez apunta al último commit de esa línea de desarrollo.

### Ramas son baratas

En otros sistemas de control de versiones, crear una rama significaba copiar
todo el proyecto. En Git, crear una rama es instantáneo — solo crea un
puntero de 41 bytes (el hash SHA-1 del commit). Por eso Git te anima a
crear ramas para todo.

### Comandos de `git branch`

```bash
# LISTAR ramas
git branch                # Lista ramas locales
git branch -v             # Lista con el último commit de cada rama
git branch -a             # Lista locales + remotas

# CREAR una rama nueva
git branch nombre-rama    # Crea la rama (NO te mueve a ella)

# RENOMBRAR una rama
git branch -m viejo-nombre nuevo-nombre
git branch -m nuevo-nombre  # Renombra la rama actual

# ELIMINAR una rama
git branch -d nombre-rama   # Elimina (solo si ya fue mergeada)
git branch -D nombre-rama   # Elimina FORZADO (aunque no esté mergeada)
```

### Convenciones para nombrar ramas

No hay reglas estrictas, pero estas convenciones son muy comunes:

| Prefijo | Uso | Ejemplo |
|---------|-----|---------|
| `feature/` | Nueva funcionalidad | `feature/menu-vegano` |
| `fix/` | Corrección de errores | `fix/precio-incorrecto` |
| `hotfix/` | Corrección urgente | `hotfix/alergia-critica` |
| `experiment/` | Pruebas/experimentos | `experiment/carta-fusion` |

**Reglas prácticas para nombres**:
- Usa minúsculas y guiones: `menu-vegano` (no `MenuVegano`).
- Sé descriptivo: `feature/carta-postres` (no `rama1`).
- No uses espacios ni caracteres especiales.

### ⚠️ Importante

- `git branch nombre` **crea** la rama pero **NO te cambia a ella**.
  Seguirás en la rama donde estabas. Para cambiarte, necesitas
  `git switch` (lección 02).
- No puedes eliminar la rama en la que estás. Primero cámbiate a otra.
- `git branch -d` es seguro: solo borra ramas ya mergeadas.
  `git branch -D` es la versión "estoy seguro, borra aunque no esté
  mergeada".

---

## 💻 Práctica

> **Prerrequisito**: Debes haber completado el Módulo 3 (lecciones 01-06)
> o al menos tener algunos commits en tu historial. Verifica con
> `git log --oneline`.

### Ejercicio 1: ¿En qué rama estás?

```bash
git branch
```

✅ **Resultado esperado**: Ves una sola rama con un asterisco indicando
que es la activa:

```
* main
```

(O `* master`, dependiendo de tu configuración.)

💡 El asterisco `*` siempre marca la rama en la que te encuentras.

---

### Ejercicio 2: Crea tu primera rama

Crea una rama para añadir un menú de postres:

```bash
git branch feature/carta-postres
```

Ahora lista las ramas:

```bash
git branch
```

✅ **Resultado esperado**:

```
  feature/carta-postres
* main
```

La nueva rama existe, pero sigues en `main` (el asterisco lo confirma).

💡 `git branch nombre` **solo crea** el puntero. No te mueve.

---

### Ejercicio 3: Crea varias ramas

Crea algunas ramas más para practicar:

```bash
git branch feature/menu-vegano
git branch fix/precio-pasta
git branch experiment/carta-fusion
```

Lista todas con detalle:

```bash
git branch -v
```

✅ **Resultado esperado**: Ves 5 ramas, todas apuntando al mismo commit
(porque no has hecho commits nuevos en ninguna):

```
  experiment/carta-fusion  df2de15 Inicializa curso...
  feature/carta-postres    df2de15 Inicializa curso...
  feature/menu-vegano      df2de15 Inicializa curso...
  fix/precio-pasta         df2de15 Inicializa curso...
* main                     df2de15 Inicializa curso...
```

💡 Todas las ramas son punteros al mismo commit. Aún no han divergido.

---

### Ejercicio 4: Renombra una rama

Renombra la rama de corrección de precios:

```bash
git branch -m fix/precio-pasta fix/corregir-precio-spaghetti
```

Verifica:

```bash
git branch
```

✅ **Resultado esperado**: La rama ahora se llama
`fix/corregir-precio-spaghetti`.

---

### Ejercicio 5: Elimina ramas que no necesitas

Elimina la rama de experimentación:

```bash
git branch -d experiment/carta-fusion
```

✅ **Resultado esperado**:

```
Deleted branch experiment/carta-fusion (was df2de15).
```

Verifica:

```bash
git branch
```

La rama `experiment/carta-fusion` ya no aparece.

---

### Ejercicio 6: Intenta eliminar la rama activa

```bash
git branch -d main
```

✅ **Resultado esperado**: Git te lo impide con un error:

```
error: Cannot delete branch 'main' checked out at...
```

💡 No puedes borrar la rama en la que estás parado. Tiene sentido: sería
como cortar la rama del árbol en la que estás sentado.

---

### Ejercicio 7: Limpieza para las siguientes lecciones

Deja solo las ramas que usaremos en las próximas lecciones:

```bash
git branch -d fix/corregir-precio-spaghetti
git branch -d feature/menu-vegano
```

Verifica que te quedan solo dos:

```bash
git branch
```

✅ **Resultado esperado**:

```
  feature/carta-postres
* main
```

Perfecto. En la siguiente lección aprenderás a moverte entre estas ramas.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git branch` | Lista ramas locales |
| `git branch -v` | Lista con último commit |
| `git branch nombre` | Crea una rama nueva |
| `git branch -m viejo nuevo` | Renombra una rama |
| `git branch -d nombre` | Elimina (si ya fue mergeada) |
| `git branch -D nombre` | Elimina forzado |

**Regla de oro**: Las ramas son baratas y desechables. Crea una rama para
cada tarea, funcionalidad o experimento. Cuando termines, la mergeas o la
borras.

---

> **Siguiente lección**: `02_git_switch.md` — Aprenderás a
> moverte entre ramas y a trabajar en cada una de forma independiente.
