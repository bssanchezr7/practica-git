# Lección 03: `.gitignore` — Excluir archivos del seguimiento

## 📖 Teoría

### ¿Qué es `.gitignore`?

`.gitignore` es un archivo de texto que le dice a Git **qué archivos o
carpetas debe ignorar**. Los archivos listados en `.gitignore` no aparecen
en `git status`, no se añaden con `git add .`, y nunca entran en un commit.

### ¿Por qué ignorar archivos?

Algunos archivos NO deberían estar en un repositorio:

| Tipo | Ejemplo | Por qué ignorarlo |
|------|---------|-------------------|
| **Secretos** | `.env`, `credentials.json` | Seguridad: contraseñas, claves API |
| **Dependencias** | `node_modules/`, `venv/` | Se pueden regenerar con un comando |
| **Compilados** | `*.class`, `*.pyc`, `dist/` | Se generan a partir del código fuente |
| **Archivos del SO** | `.DS_Store`, `Thumbs.db` | Específicos de tu máquina |
| **Archivos del editor** | `.idea/`, `.vscode/` | Preferencias personales del IDE |
| **Logs y temporales** | `*.log`, `tmp/` | No aportan valor al proyecto |

### Sintaxis de `.gitignore`

```bash
# Comentarios empiezan con #

# Ignorar un archivo específico
secreto.txt

# Ignorar todos los archivos con una extensión
*.log
*.tmp
*.pyc

# Ignorar una carpeta completa
node_modules/
dist/
__pycache__/

# Ignorar archivos en cualquier subdirectorio
**/debug.log

# Negación: NO ignorar un archivo específico (excepción)
!importante.log

# Ignorar archivos en una carpeta específica
docs/*.pdf

# Ignorar todo dentro de una carpeta excepto algo
build/*
!build/.gitkeep
```

### Reglas de los patrones

| Patrón | Significado |
|--------|------------|
| `archivo.txt` | Ignora `archivo.txt` en cualquier carpeta |
| `/archivo.txt` | Ignora solo en la raíz del proyecto |
| `carpeta/` | Ignora la carpeta y todo su contenido |
| `*.log` | Ignora todo lo que termine en `.log` |
| `**/temp` | Ignora `temp` en cualquier nivel de profundidad |
| `!excepcion.txt` | NO ignora este archivo (excepción) |
| `doc/*.pdf` | Ignora PDFs solo dentro de `doc/` |
| `doc/**/*.pdf` | Ignora PDFs en `doc/` y subcarpetas |

### `.gitignore` global

Puedes tener un `.gitignore` global para tu usuario (aplica a todos
tus repos):

```bash
git config --global core.excludesfile ~/.gitignore_global
```

Útil para ignorar archivos del sistema operativo y tu editor en TODOS
los proyectos.

### ⚠️ Importante

- `.gitignore` solo afecta a archivos **que aún no están rastreados**.
  Si ya hiciste `git add` o commit de un archivo, `.gitignore` no lo
  ignorará. Necesitas quitarlo del seguimiento primero:

  ```bash
  git rm --cached archivo.txt     # Lo quita del seguimiento (no del disco)
  echo "archivo.txt" >> .gitignore
  git commit -m "Deja de rastrear archivo.txt"
  ```

- `.gitignore` sí se versiona (va dentro del repositorio). Es una buena
  práctica compartirlo con el equipo.

---

## 💻 Práctica

### Ejercicio 1: Crea un `.gitignore` básico

```bash
cd ~/practica_git
cat > .gitignore << 'EOF'
# Archivos del sistema operativo
.DS_Store
Thumbs.db

# Archivos temporales
*.tmp
*.log
*.bak

# Carpetas que no deben versionarse
tmp/
temp/

# Archivos de configuración local
.env
.env.local
EOF

git status
```

✅ `.gitignore` aparece como nuevo archivo sin rastrear.

```bash
git add .gitignore
git commit -m "Agrega .gitignore con patrones básicos"
```

---

### Ejercicio 2: Comprueba que funciona

Crea archivos que deberían ser ignorados:

```bash
echo "debug info" > app.log
echo "contraseñas secretas" > .env
echo "archivo temporal" > notas.tmp
mkdir tmp && echo "basura" > tmp/basura.txt
git status
```

✅ **Resultado esperado**: Ninguno de estos archivos aparece en
`git status`. Git los está ignorando.

---

### Ejercicio 3: Fuerza la inclusión de un archivo ignorado

Si necesitas añadir un archivo ignorado excepcionalmente:

```bash
echo "Este log sí es importante" > errores_criticos.log
git add errores_criticos.log
```

✅ Git te dice que el archivo está ignorado. Fuerza la adición:

```bash
git add -f errores_criticos.log
git status
```

✅ Ahora aparece en el staging. (No hagas commit, es solo prueba.)

```bash
git restore --staged errores_criticos.log
rm errores_criticos.log
```

---

### Ejercicio 4: Deja de rastrear un archivo ya commiteado

Imagina que commiteaste un archivo que debería ignorarse:

```bash
echo "MI_CLAVE_SECRETA=abc123" > config_local.txt
git add config_local.txt
git commit -m "Agrega config local (error: no debería estar aquí)"
```

Ahora quítalo del seguimiento:

```bash
echo "config_local.txt" >> .gitignore
git rm --cached config_local.txt
git status
```

✅ El archivo está marcado para eliminación del seguimiento (pero sigue
en tu disco). Confirma:

```bash
git add .gitignore
git commit -m "Excluye config_local.txt del seguimiento"
```

Verifica:

```bash
ls config_local.txt     # Sigue en el disco
git status              # No aparece (ignorado)
```

✅ El archivo existe en tu carpeta pero Git ya no lo rastrea.

Limpia:

```bash
rm config_local.txt
```

---

### Ejercicio 5: Verifica qué archivos están siendo ignorados

```bash
git status --ignored
```

✅ Muestra los archivos ignorados al final de la salida.

Limpia los archivos de prueba:

```bash
rm -f app.log .env notas.tmp
rm -rf tmp
```

---

## 🧠 Resumen

| Concepto | Detalle |
|----------|---------|
| `.gitignore` | Archivo que lista patrones de archivos a ignorar |
| `*.log` | Ignora todos los `.log` |
| `carpeta/` | Ignora una carpeta completa |
| `!archivo` | Excepción (no ignorar este archivo) |
| `git rm --cached` | Deja de rastrear sin borrar del disco |
| `git add -f` | Fuerza añadir un archivo ignorado |
| `git status --ignored` | Muestra archivos ignorados |

---

> **Siguiente lección**: `04_ejercicio_final.md` — Ejercicio integrador
> que combina stash, tags y gitignore.
