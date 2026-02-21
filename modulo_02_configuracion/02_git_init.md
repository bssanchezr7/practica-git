# Lección 02: `git init` — Crear un repositorio desde cero

## 📖 Teoría

### ¿Qué hace `git init`?

`git init` convierte una carpeta normal en un **repositorio Git**. Crea una
carpeta oculta `.git/` que contiene toda la maquinaria que Git necesita para
rastrear cambios.

```
  ANTES de git init:              DESPUÉS de git init:
  ┌──────────────────┐           ┌──────────────────┐
  │ mi_proyecto/     │           │ mi_proyecto/     │
  │ ├── archivo1.txt │           │ ├── .git/        │ ← ¡NUEVO!
  │ └── archivo2.txt │           │ ├── archivo1.txt │
  └──────────────────┘           │ └── archivo2.txt │
                                 └──────────────────┘
  (carpeta normal)               (repositorio Git)
```

### ¿Qué hay dentro de `.git/`?

```
.git/
├── HEAD            ← Puntero a la rama actual
├── config          ← Configuración local del repositorio
├── description     ← Descripción (usado por GitWeb)
├── hooks/          ← Scripts que se ejecutan en ciertos eventos
├── info/           ← Información adicional
├── objects/        ← Base de datos de Git (commits, archivos, etc.)
└── refs/           ← Punteros a commits (ramas, tags)
```

⚠️ No necesitas entender cada carpeta ahora. Lo importante es saber que
**toda la historia de tu proyecto vive dentro de `.git/`**. Si borras esta
carpeta, pierdes todo el historial (pero no los archivos actuales).

### ¿Cuándo usar `git init`?

- Cuando empiezas un **proyecto nuevo** desde cero.
- Cuando quieres empezar a rastrear un **proyecto existente** que no usa Git.

### `git init` vs `git clone`

| Comando | Uso | Resultado |
|---------|-----|-----------|
| `git init` | Crear un repo nuevo | Repo vacío (sin commits) |
| `git clone` | Copiar un repo existente | Repo con todo el historial |

---

## 💻 Práctica

### Ejercicio 1: Crea un repositorio desde cero

Crea una carpeta nueva y conviértela en repositorio:

```bash
mkdir /tmp/mi_primer_repo
cd /tmp/mi_primer_repo
git init
```

✅ **Resultado esperado**:

```
Initialized empty Git repository in /tmp/mi_primer_repo/.git/
```

Verifica que se creó `.git/`:

```bash
ls -la
```

✅ Ves la carpeta `.git/` (oculta, por eso necesitas `-a`).

---

### Ejercicio 2: Explora la carpeta `.git/`

```bash
ls .git/
```

✅ Ves las carpetas y archivos internos de Git: HEAD, config, objects, refs...

Mira qué dice HEAD:

```bash
cat .git/HEAD
```

✅ **Resultado esperado**: `ref: refs/heads/main` (o `refs/heads/master`).
Esto indica la rama actual.

---

### Ejercicio 3: Tu primer commit en el repo nuevo

```bash
echo "# Mi Primer Proyecto" > README.md
git status
git add README.md
git commit -m "Primer commit: agrega README"
```

✅ Has creado el primer commit (root commit) de este repositorio.

```bash
git log --oneline
```

✅ Un solo commit en el historial.

---

### Ejercicio 4: ¿Qué pasa si haces `git init` en un repo existente?

```bash
git init
```

✅ **Resultado esperado**:

```
Reinitialized existing Git repository in /tmp/mi_primer_repo/.git/
```

💡 No pasa nada malo. `git init` en un repo existente es **inofensivo** —
no borra nada, no reinicia el historial. Solo "reinicializa" la
configuración interna.

---

### Ejercicio 5: Limpieza

```bash
cd ~/practica_git
rm -rf /tmp/mi_primer_repo
```

✅ El repo temporal se elimina. Tu repo de práctica sigue intacto.

---

### Ejercicio 6: Observa el `.git/` de tu repo de práctica

```bash
cd ~/practica_git
ls .git/
cat .git/HEAD
git log --oneline -3
```

✅ Tu repositorio de práctica tiene más contenido en `.git/` porque ya
tiene varios commits.

---

## 🧠 Resumen

| Comando | Efecto |
|---------|--------|
| `git init` | Crea un repositorio Git en la carpeta actual |
| `ls .git/` | Muestra la estructura interna de Git |
| `cat .git/HEAD` | Muestra la rama actual |

**Regla de oro**: `git init` se ejecuta UNA vez al inicio del proyecto.
Después, nunca más necesitas tocarlo.

---

> **Siguiente lección**: `03_git_clone.md` — Aprenderás a obtener una
> copia completa de un repositorio que ya existe.
