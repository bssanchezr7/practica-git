# Lección 01: `git remote` — Conectar con repositorios remotos

## 📖 Teoría

### ¿Qué es un remoto?

Un **remoto** (remote) es una versión de tu repositorio alojada en otro
lugar — normalmente un servidor como GitHub, GitLab o Bitbucket. Es la
forma en que Git permite la colaboración: tú trabajas en tu copia local
y la sincronizas con el remoto.

```
  Tu máquina                          Servidor (GitHub)
  ┌──────────────────┐               ┌──────────────────┐
  │ practica_git/    │               │ practica_git     │
  │ ├── .git/        │◀────────────▶│ (repositorio     │
  │ ├── menu.txt     │  push / pull  │  remoto)         │
  │ └── ...          │               │                  │
  └──────────────────┘               └──────────────────┘
       REPO LOCAL                        REPO REMOTO
                                          ("origin")
```

### ¿Qué es `origin`?

`origin` es el **nombre** por defecto que Git le da al remoto principal.
No es nada mágico — es un alias corto para no escribir la URL completa
cada vez.

```
  origin = https://github.com/tu-usuario/practica_git.git

  En vez de:  git push https://github.com/tu-usuario/practica_git.git main
  Escribes:   git push origin main
```

Puedes tener múltiples remotos con diferentes nombres:

```
  origin    → Tu repositorio en GitHub
  upstream  → El repositorio original (si hiciste fork)
  backup    → Un servidor de respaldo
```

### Comandos de `git remote`

```bash
# Listar remotos configurados
git remote
git remote -v          # Con URLs detalladas

# Añadir un remoto
git remote add nombre URL

# Eliminar un remoto
git remote remove nombre

# Renombrar un remoto
git remote rename viejo nuevo

# Ver información detallada de un remoto
git remote show nombre
```

### El modelo local + remoto

```
  ┌─────────────────────────────────────────────────────────┐
  │                   TU MÁQUINA                            │
  │                                                         │
  │  Working     Staging     Repo Local     Repo Remoto     │
  │  Directory    Area       (.git/)        (origin)        │
  │  ┌──────┐   ┌──────┐   ┌──────┐       ┌──────┐        │
  │  │      │──▶│      │──▶│      │──────▶│      │        │
  │  │      │   │      │   │      │ push  │      │        │
  │  │      │   │      │   │      │◀──────│      │        │
  │  │      │   │      │   │      │ fetch │      │        │
  │  └──────┘   └──────┘   └──────┘       └──────┘        │
  │                                                         │
  │  git add     git commit    git push / git fetch         │
  └─────────────────────────────────────────────────────────┘
```

El flujo se extiende: editar → add → commit → **push** (al remoto).
Y para traer cambios del remoto: **fetch** / **pull**.

### ⚠️ Importante

- Un repositorio puede funcionar perfectamente sin remoto (solo local).
- Los remotos son opcionales, pero esenciales para colaborar.
- `git clone` configura `origin` automáticamente.
- `git init` NO configura ningún remoto — lo añades tú manualmente.

---

## 💻 Práctica

### Ejercicio 1: Verifica si tu repo tiene remotos

```bash
cd ~/practica_git
git remote -v
```

✅ **Resultado esperado**: Probablemente no aparece nada, porque este
repositorio se creó con `git init` y no se ha conectado a ningún remoto.

---

### Ejercicio 2: Simula un remoto con un repositorio local

No necesitas GitHub para practicar remotos. Puedes usar otra carpeta
como si fuera un servidor:

```bash
# Crea un repositorio "bare" que simula un servidor remoto
git init --bare /tmp/remoto_simulado.git
```

💡 Un repositorio **bare** es uno sin Working Directory — solo contiene
la carpeta `.git/`. Es lo que tienen los servidores como GitHub.

Ahora conéctalo como remoto:

```bash
cd ~/practica_git
git remote add origin /tmp/remoto_simulado.git
git remote -v
```

✅ **Resultado esperado**:

```
origin  /tmp/remoto_simulado.git (fetch)
origin  /tmp/remoto_simulado.git (push)
```

Tienes un remoto llamado `origin` apuntando a tu "servidor simulado".

---

### Ejercicio 3: Inspecciona el remoto

```bash
git remote show origin
```

✅ Muestra información sobre el remoto: URL, ramas rastreadas, etc.

---

### Ejercicio 4: Añade un segundo remoto

```bash
git init --bare /tmp/backup_simulado.git
git remote add backup /tmp/backup_simulado.git
git remote -v
```

✅ **Resultado esperado**: Ves dos remotos:

```
backup  /tmp/backup_simulado.git (fetch)
backup  /tmp/backup_simulado.git (push)
origin  /tmp/remoto_simulado.git (fetch)
origin  /tmp/remoto_simulado.git (push)
```

---

### Ejercicio 5: Renombra y elimina remotos

```bash
git remote rename backup respaldo
git remote -v
```

✅ `backup` ahora se llama `respaldo`.

```bash
git remote remove respaldo
git remote -v
```

✅ Solo queda `origin`.

---

### Ejercicio 6: Conectar a GitHub (opcional)

Si tienes una cuenta en GitHub, puedes crear un repositorio ahí y
conectarlo. Desde github.com:

1. Crea un nuevo repositorio (sin README ni .gitignore).
2. Copia la URL HTTPS.
3. Ejecuta:

```bash
# Si ya tienes origin, primero quítalo o usa otro nombre
git remote set-url origin https://github.com/TU-USUARIO/practica_git.git

# O si no tienes origin:
git remote add origin https://github.com/TU-USUARIO/practica_git.git
```

💡 Si prefieres no usar GitHub ahora, el remoto simulado que creamos
funciona perfectamente para las siguientes lecciones.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git remote` | Lista los remotos |
| `git remote -v` | Lista con URLs |
| `git remote add nombre URL` | Añade un remoto |
| `git remote remove nombre` | Elimina un remoto |
| `git remote rename viejo nuevo` | Renombra un remoto |
| `git remote show nombre` | Información detallada |

| Concepto | Detalle |
|----------|---------|
| `origin` | Nombre por defecto del remoto principal |
| Remoto | Copia del repositorio en otro lugar (servidor) |
| Bare repo | Repositorio sin Working Directory (lo que usan los servidores) |

---

> **Siguiente lección**: `02_git_push.md` — Aprenderás a enviar tus
> commits al repositorio remoto.
