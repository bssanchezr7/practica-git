# Lección 03: `git clone` — Clonar un repositorio existente

## 📖 Teoría

### ¿Qué hace `git clone`?

`git clone` crea una **copia completa** de un repositorio existente: todos
los archivos, todo el historial, todas las ramas. Es la forma de obtener
un proyecto que alguien más (o tú mismo) ya creó.

```
  Repositorio remoto              Tu máquina
  (GitHub, GitLab, etc.)

  ┌──────────────────┐    git clone    ┌──────────────────┐
  │ proyecto/        │ ──────────────▶ │ proyecto/        │
  │ ├── .git/        │                 │ ├── .git/        │
  │ ├── README.md    │                 │ ├── README.md    │
  │ ├── src/         │                 │ ├── src/         │
  │ └── (historial)  │                 │ └── (historial)  │
  └──────────────────┘                 └──────────────────┘

  Copia COMPLETA: archivos + historial + ramas + configuración
```

### `git clone` vs descargar ZIP

| Aspecto | `git clone` | Descargar ZIP |
|---------|------------|---------------|
| Historial de commits | ✅ Completo | ❌ No incluido |
| Ramas | ✅ Todas | ❌ Solo la rama actual |
| Conexión con el remoto | ✅ Configurada | ❌ No existe |
| Posibilidad de hacer pull/push | ✅ Inmediata | ❌ Imposible |
| Tamaño de descarga | Mayor | Menor |

### Sintaxis

```bash
# Clonar con HTTPS (lo más común)
git clone https://github.com/usuario/proyecto.git

# Clonar con SSH (requiere configurar llaves SSH)
git clone git@github.com:usuario/proyecto.git

# Clonar con un nombre de carpeta diferente
git clone https://github.com/usuario/proyecto.git mi-carpeta

# Clonar solo la última versión (sin historial profundo)
git clone --depth 1 https://github.com/usuario/proyecto.git
```

### ¿De dónde puedo clonar?

- **GitHub**: `https://github.com/usuario/repo.git`
- **GitLab**: `https://gitlab.com/usuario/repo.git`
- **Bitbucket**: `https://bitbucket.org/usuario/repo.git`
- **Otro servidor**: Cualquier URL que apunte a un repositorio Git.
- **Carpeta local**: También puedes clonar un repo de otra carpeta:
  `git clone /ruta/al/repo /ruta/destino`

### ¿Qué pasa después de clonar?

Después de `git clone`, tu copia local tiene:

1. Todos los archivos de la rama principal.
2. Todo el historial de commits.
3. Todas las ramas remotas (puedes cambiar a cualquiera).
4. Un **remote** llamado `origin` que apunta al repositorio original.

```bash
git remote -v
# origin  https://github.com/usuario/proyecto.git (fetch)
# origin  https://github.com/usuario/proyecto.git (push)
```

### `git init` + `git remote` vs `git clone`

`git clone` es equivalente a hacer estos pasos manualmente:

```bash
mkdir proyecto
cd proyecto
git init
git remote add origin https://github.com/usuario/proyecto.git
git fetch origin
git checkout main
```

Pero en un solo comando.

### ⚠️ Importante

- `git clone` crea una **carpeta nueva** con el nombre del repositorio.
  No clones dentro de otro repositorio Git.
- La primera clonación puede tardar si el repositorio es grande (mucho
  historial o archivos binarios grandes).
- `--depth 1` es útil para repos enormes cuando solo necesitas el código
  actual, no el historial completo.

---

## 💻 Práctica

### Ejercicio 1: Clona un repositorio público

Vamos a clonar un repositorio real de GitHub:

```bash
cd /tmp
git clone https://github.com/octocat/Hello-World.git
```

✅ **Resultado esperado**: Git descarga el repositorio completo en
`/tmp/Hello-World/`.

Explóralo:

```bash
cd Hello-World
ls -la
git log --oneline --all
git branch -a
git remote -v
```

✅ Ves los archivos, el historial, las ramas (incluyendo remotas con
`remotes/origin/...`) y la configuración del remote `origin`.

---

### Ejercicio 2: Clona con otro nombre

```bash
cd /tmp
git clone https://github.com/octocat/Hello-World.git mi-copia
ls mi-copia/
```

✅ El repositorio se clonó en una carpeta llamada `mi-copia` en vez de
`Hello-World`.

---

### Ejercicio 3: Clona tu propio repositorio local

También puedes clonar repositorios locales (útil para experimentar):

```bash
cd /tmp
git clone ~/practica_git copia_practica
cd copia_practica
git log --oneline -5
git remote -v
```

✅ **Resultado esperado**: Tienes una copia completa de tu repo de
práctica. El remote `origin` apunta a `~/practica_git`.

---

### Ejercicio 4: Diferencia entre `--depth 1` y clone completo

```bash
cd /tmp

# Clone superficial
git clone --depth 1 https://github.com/octocat/Hello-World.git shallow-copy
cd shallow-copy
git log --oneline
```

✅ Solo ves UN commit (el más reciente). El historial está recortado.

```bash
cd ../Hello-World
git log --oneline
```

✅ Aquí ves TODO el historial. Esa es la diferencia.

---

### Ejercicio 5: Limpieza

```bash
cd ~/practica_git
rm -rf /tmp/Hello-World /tmp/mi-copia /tmp/copia_practica /tmp/shallow-copy
```

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git clone URL` | Copia completa del repositorio remoto |
| `git clone URL carpeta` | Clona con nombre de carpeta personalizado |
| `git clone --depth 1 URL` | Copia solo la última versión |
| `git remote -v` | Ver a dónde apunta el repositorio clonado |

| Concepto | Detalle |
|----------|---------|
| `origin` | Nombre del remote que se crea automáticamente al clonar |
| Clone completo | Incluye archivos, historial, ramas y configuración |
| Clone vs ZIP | Clone incluye todo Git; ZIP es solo archivos |

**Regla de oro**: Usa `git clone` para obtener proyectos existentes y
`git init` para empezar proyectos nuevos.

---

> **Siguiente módulo**: Continúa con el **Módulo 3: Flujo básico de trabajo**
> → `../modulo_03_flujo_basico/01_git_status.md`
