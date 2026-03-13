# Lección 02: `git tag` — Marcar versiones importantes

## 📖 Teoría

### ¿Qué es un tag?

Un **tag** es una etiqueta permanente que apunta a un commit específico. Según [Pro Git (Capítulo 2.6)](https://git-scm.com/book/en/v2/Git-Basics-Tagging), se usa para marcar versiones o hitos importantes del proyecto: `v1.0`, `v2.3.1`, `release-2026-02`.

```
  C1 ── C2 ── C3 ── C4 ── C5 ── C6 ── C7
              ↑              ↑          ↑
           v1.0           v1.1        main
```

### Tags vs ramas

| Aspecto | Tag | Rama |
|---------|-----|------|
| Se mueve con nuevos commits | ❌ No (fijo) | ✅ Sí (avanza) |
| Propósito | Marcar una versión | Línea de trabajo activa |
| Permanencia | Permanente | Temporal o permanente |

Un tag es como un **marcador de libro**: señala un punto fijo. Una rama
es como un **cursor**: se mueve mientras escribes.

### Dos tipos de tags

#### 1. Tags ligeros (lightweight)

Solo un nombre apuntando a un commit. Sin metadatos adicionales.

```bash
git tag v1.0
```

#### 2. Tags anotados (annotated) — recomendados

Incluyen nombre del creador, fecha, mensaje y están firmados. Son objetos
completos en Git.

```bash
git tag -a v1.0 -m "Primera versión estable del menú"
```

💡 Usa **siempre tags anotados** para releases. Los ligeros son para marcas temporales o internas.

#### 3. Tags firmados (GPG)
Para proyectos profesionales, puedes firmar tus tags con una clave GPG para asegurar su autenticidad:
```bash
git tag -s v1.0 -m "Versión firmada"
```
Luego puedes verificarla con `git tag -v v1.0`.

### Versionado semántico (Semantic Versioning)

La convención más usada para nombrar versiones:

```
  v MAJOR . MINOR . PATCH
  v 1     . 3     . 2

  MAJOR: Cambios incompatibles (rompe lo anterior)
  MINOR: Nueva funcionalidad (compatible hacia atrás)
  PATCH: Correcciones de errores
```

Ejemplos:
- `v1.0.0` → Primera versión estable.
- `v1.1.0` → Se añadió una funcionalidad.
- `v1.1.1` → Se corrigió un error.
- `v2.0.0` → Cambio grande, incompatible con v1.

### Comandos principales

```bash
# Crear tag anotado en el commit actual
git tag -a v1.0 -m "Descripción de esta versión"

# Crear tag ligero
git tag v1.0-beta

# Crear tag en un commit antiguo
git tag -a v0.9 -m "Versión previa" abc1234

# Listar tags
git tag
git tag -l "v1.*"          # Filtrar por patrón

# Ver información de un tag
git show v1.0

# Eliminar un tag local
git tag -d v1.0

# Enviar tags al remoto
git push origin v1.0       # Un tag específico
git push origin --tags      # Todos los tags

# Eliminar un tag del remoto
git push origin --delete v1.0
```

### ⚠️ Importante

- Los tags NO se pushean automáticamente con `git push`. Necesitas
  `git push --tags` o `git push origin nombre-tag`.
- Un tag no se puede mover. Si necesitas cambiar a dónde apunta,
  elimínalo y créalo de nuevo.

---

## 💻 Práctica

### Ejercicio 1: Crea tu primer tag anotado

```bash
cd ~/practica_git
git tag -a v1.0.0 -m "Versión 1.0: menú e inventario completos"
git tag
```

✅ Ves `v1.0.0` en la lista.

```bash
git show v1.0.0
```

✅ Ves la información del tag: quién, cuándo, mensaje y el commit al
que apunta.

---

### Ejercicio 2: Tag en un commit antiguo

```bash
git log --oneline -5
```

Elige el hash del primer commit y márcalo:

```bash
git tag -a v0.1.0 $(git rev-list --max-parents=0 HEAD) -m "Versión inicial del curso"
git tag
```

✅ Ves ambos tags: `v0.1.0` y `v1.0.0`.

---

### Ejercicio 3: Filtra tags

```bash
git tag -l "v1.*"
```

✅ Solo muestra tags que empiezan con `v1.`.

---

### Ejercicio 4: Haz cambios y crea otra versión

```bash
echo "- Tarta de manzana — 6.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega tarta de manzana al menú"

git tag -a v1.1.0 -m "Versión 1.1: nuevo postre añadido"
git log --oneline --decorate -5
```

✅ Ves los tags junto a los commits en el historial:

```
abc1234 (HEAD -> main, tag: v1.1.0) Agrega tarta de manzana...
def5678 (tag: v1.0.0) ...
```

---

### Ejercicio 5: Envía tags al remoto (si tienes uno)

```bash
git remote -v
```

Si tienes un remoto configurado:

```bash
git push origin --tags
```

✅ Todos los tags se envían al remoto.

---

### Ejercicio 6: Elimina un tag

```bash
git tag -d v0.1.0
git tag
```

✅ `v0.1.0` eliminado. Solo queda `v1.0.0` y `v1.1.0`.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git tag -a v1.0 -m "msg"` | Crea tag anotado |
| `git tag v1.0` | Crea tag ligero |
| `git tag` | Lista tags |
| `git show v1.0` | Detalle de un tag |
| `git tag -d v1.0` | Elimina tag local |
| `git push origin --tags` | Envía todos los tags al remoto |

---

> **Siguiente lección**: `03_gitignore.md` — Aprenderás a excluir archivos
> del seguimiento de Git.
