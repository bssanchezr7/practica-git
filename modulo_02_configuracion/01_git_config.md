# Lección 01: `git config` — Configurar tu identidad

## 📖 Teoría

### ¿Por qué configurar Git?

Cada commit que hagas llevará tu nombre y email. Git necesita saber quién
eres antes de que puedas guardar nada en el historial. Es como firmar
tu trabajo.

### Los tres niveles de configuración

Git tiene tres niveles de configuración, cada uno con diferente alcance. Según la [documentación oficial de git-config](https://git-scm.com/docs/git-config#_configuration_file) y [Pro Git (Capítulo 1.6)](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup), estos archivos se leen en orden de jerarquía:

```
  ┌──────────────────────────────────────────────────────────┐
  │                                                          │
  │  ③ SYSTEM    → Todos los usuarios de esta máquina       │
  │     /etc/gitconfig                                       │
  │                                                          │
  │  ② GLOBAL    → Todos tus repositorios (tu usuario)      │
  │     ~/.gitconfig                                         │
  │                                                          │
  │  ① LOCAL     → Solo este repositorio                    │
  │     .git/config                                          │
  │                                                          │
  │  Prioridad: LOCAL > GLOBAL > SYSTEM                     │
  │  (el más específico gana)                                │
  │                                                          │
  └──────────────────────────────────────────────────────────┘
```

| Nivel | Flag | Archivo (típico) | Alcance |
|-------|------|---------|---------|
| System | `--system` | `/etc/gitconfig` | Toda la máquina |
| Global | `--global` | `~/.gitconfig` | Tu usuario |
| Local | `--local` | `.git/config` | Solo este repo |
| Worktree| `--worktree` | `.git/config.worktree` | Específico del worktree |

Lo más común es usar `--global` (configuras una vez y aplica a todo).

### Configuraciones esenciales

```bash
# Tu nombre (aparece en cada commit)
git config --global user.name "Tu Nombre"

# Tu email (aparece en cada commit)
git config --global user.email "tu@email.com"

# Editor de texto por defecto (para mensajes de commit largos)
git config --global core.editor "code --wait"   # VS Code / Cursor
# git config --global core.editor "nano"         # nano
# git config --global core.editor "vim"          # vim

# Rama por defecto al hacer git init
git config --global init.defaultBranch main
```

### Configuraciones recomendadas

```bash
# Colorear la salida de Git (normalmente ya está activado)
git config --global color.ui auto

# Mostrar conflictos con tres secciones en vez de dos
git config --global merge.conflictstyle diff3

# Evitar merges automáticos al hacer pull
git config --global pull.rebase false

# Autocorregir comandos escritos con errores (espera 1.5s antes de ejecutar)
git config --global help.autocorrect 15
```

### Ver y gestionar la configuración

```bash
# Ver TODA la configuración activa
git config --list

# Ver solo configuración global
git config --global --list

# Ver un valor específico
git config user.name

# Eliminar una configuración
git config --global --unset clave.nombre

# Abrir el archivo de configuración en tu editor
git config --global --edit
```

### ⚠️ Importante

- Si no configuras `user.name` y `user.email`, Git te pedirá que lo hagas
  antes de tu primer commit.
- El email que uses aquí es el que se asocia a tus commits. Si usas GitHub,
  usa el mismo email de tu cuenta para que los commits se vinculen a tu
  perfil.
- La configuración local (por repositorio) tiene prioridad sobre la global.
  Útil si usas un email diferente para el trabajo y proyectos personales.

---

## 💻 Práctica

### Ejercicio 1: Verifica tu configuración actual

```bash
git config --list
```

✅ Ves todas las configuraciones activas. Busca `user.name` y `user.email`.

Si este repositorio tiene configuración local, también la verás:

```bash
git config --local --list
```

---

### Ejercicio 2: Configura tu identidad global

Configura tu nombre y email para todos tus repositorios:

```bash
git config --global user.name "Tu Nombre Aquí"
git config --global user.email "tu_email@ejemplo.com"
```

⚠️ Sustituye los valores por tu nombre y email reales.

Verifica:

```bash
git config user.name
git config user.email
```

✅ **Resultado esperado**: Muestra los valores que acabas de configurar.

---

### Ejercicio 3: Configura tu editor

Elige tu editor preferido:

```bash
# Si usas VS Code o Cursor:
git config --global core.editor "code --wait"

# Si prefieres nano (más simple):
git config --global core.editor "nano"

# Si prefieres vim:
git config --global core.editor "vim"
```

Para probar que funciona:

```bash
git config --global --edit
```

✅ Se abre tu editor con el archivo `~/.gitconfig`. Ciérralo sin cambiar
nada (o examina el contenido, es interesante).

---

### Ejercicio 4: Configura la rama por defecto

```bash
git config --global init.defaultBranch main
```

Esto asegura que cuando crees nuevos repositorios, la rama principal se
llame `main` (no `master`, que era el nombre antiguo).

---

### Ejercicio 5: Configuración local vs global

Configura un nombre diferente SOLO para este repositorio:

```bash
git config --local user.name "Estudiante de Git"
```

Ahora compara:

```bash
echo "Global: $(git config --global user.name)"
echo "Local:  $(git config --local user.name)"
echo "Efectivo: $(git config user.name)"
```

✅ **Resultado esperado**: El valor "efectivo" (sin flag) es el local,
porque tiene prioridad sobre el global.

Restaura la configuración local para que coincida:

```bash
git config --local --unset user.name
git config user.name
```

✅ Ahora usa el valor global de nuevo.

---

### Ejercicio 6: Crea un alias útil

Los alias te permiten crear atajos para comandos largos:

```bash
git config --global alias.st "status"
git config --global alias.cm "commit -m"
git config --global alias.lg "log --oneline --graph --all --decorate"
```

Pruébalos:

```bash
git st
git lg
```

✅ `git st` equivale a `git status`. `git lg` muestra un historial
bonito y compacto.

💡 Puedes crear tantos alias como quieras. Los más usados son `st`,
`cm`, `co` (checkout), `br` (branch) y `lg`.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git config --global user.name "nombre"` | Configura tu nombre |
| `git config --global user.email "email"` | Configura tu email |
| `git config --global core.editor "editor"` | Configura tu editor |
| `git config --list` | Ver toda la configuración |
| `git config clave` | Ver un valor específico |
| `git config --global alias.xx "comando"` | Crear un atajo |
| `git config --global --edit` | Editar el archivo de configuración |

**Regla de oro**: Configura `user.name` y `user.email` una vez con
`--global` y olvídate. Solo usa `--local` si necesitas valores distintos
por proyecto.

---

> **Siguiente lección**: `02_git_init.md` — Aprenderás a crear un
> repositorio Git desde cero.
