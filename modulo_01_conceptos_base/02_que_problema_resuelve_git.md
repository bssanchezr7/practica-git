# Lección 02: ¿Qué problema resuelve Git?

## 📖 Teoría

### Git resuelve cinco problemas fundamentales

#### Problema 1: "¿Qué cambió?"

Sin Git, comparar dos versiones de un archivo requiere abrirlos lado a lado
y buscar diferencias a ojo. Con Git:

```bash
git diff          # Te muestra exactamente cada línea que cambió
git log           # Te muestra el historial de todos los cambios
```

Git registra **cada cambio, quién lo hizo y cuándo**, automáticamente.

#### Problema 2: "Quiero volver atrás"

Sin Git, volver a una versión anterior significa buscar entre backups
(si los tienes). Con Git:

```bash
git log             # Ves la lista de todos los estados guardados
git checkout abc123 # Vuelves a cualquier punto del pasado
```

Cada commit es un **punto de restauración**. Puedes tener miles y volver
a cualquiera en un instante.

#### Problema 3: "Quiero probar algo sin romper lo que funciona"

Sin Git, experimentar es arriesgado. Con Git:

```bash
git switch -c experimento    # Creas una realidad paralela
# ... haces cambios ...
git switch main              # Vuelves a la versión estable, intacta
```

Las **ramas** te permiten experimentar libremente. Si el experimento
funciona, lo incorporas. Si no, lo descartas sin consecuencias.

#### Problema 4: "Somos varios trabajando en lo mismo"

Sin Git, colaborar en los mismos archivos es un caos de "no toques eso
que lo estoy editando". Con Git:

```
  Ana trabaja en el menú     ──▶  git merge  ──▶  Versión combinada
  Luis trabaja en el inventario ──▶             con ambos cambios
```

Git **combina automáticamente** los cambios de distintas personas. Solo
pide ayuda cuando dos personas modifican exactamente las mismas líneas.

#### Problema 5: "Se rompió todo, ¿qué pasó?"

Sin Git, depurar un error es buscar a ciegas. Con Git:

```bash
git log       # ¿Qué cambios se hicieron recientemente?
git blame     # ¿Quién cambió esta línea y cuándo?
git bisect    # Busca automáticamente en qué commit se introdujo el error
```

Git es una **máquina del tiempo** que te permite investigar el pasado.

### Git en números

- Creado por **Linus Torvalds** en 2005 (el creador de Linux).
- Usado por el **95%+** de los equipos de desarrollo profesionales.
- **GitHub** aloja más de 200 millones de repositorios.
- Se usa para código, documentación, libros, diseños, datos y más.

### No solo para programadores

Aunque Git nació para gestionar código fuente, sirve para cualquier
archivo de texto:

- Documentación y manuales
- Archivos de configuración
- Notas y apuntes
- Recetas (como en nuestro restaurante)
- Datos en formato CSV/JSON
- Contratos y documentos legales

⚠️ Git funciona mejor con **archivos de texto**. No es ideal para archivos
binarios grandes (imágenes, vídeos, ejecutables), aunque puede manejarlos.

---

## 💻 Práctica

### Ejercicio 1: Piensa en tu propio caso

Antes de seguir, reflexiona:

1. ¿Alguna vez perdiste un cambio importante en un archivo?
2. ¿Alguna vez tuviste que colaborar en un documento y fue caótico?
3. ¿Alguna vez quisiste volver a una versión anterior y no pudiste?

Estos son exactamente los problemas que Git resuelve. Tenlos en mente
mientras aprendes — te ayudará a entender el "para qué" de cada comando.

---

### Ejercicio 2: Explora un repositorio Git real

Vamos a ver cómo se ve el historial de un proyecto real. Desde tu terminal:

```bash
cd ~/practica_git
git log --oneline
```

✅ **Resultado esperado**: Ves la lista de commits de este repositorio de
práctica. Cada línea es un punto del tiempo al que podrías volver.

```bash
git log --oneline --stat
```

✅ Ahora ves qué archivos se tocaron en cada commit. Eso es el poder de
Git: trazabilidad completa.

---

### Ejercicio 3: ¿Quién, cuándo, qué?

```bash
git log --format="%h | %an | %ar | %s"
```

✅ Para cada commit ves: hash | autor | hace cuánto tiempo | qué se hizo.

💡 Imagina esto en un proyecto con 50 personas y miles de commits. Git
hace manejable lo que de otra forma sería imposible.

---

## 🧠 Resumen

| Problema | Solución de Git |
|----------|----------------|
| "¿Qué cambió?" | `git diff`, `git log` |
| "Quiero volver atrás" | `git checkout`, `git revert` |
| "Quiero probar sin romper" | Ramas (`git branch`, `git switch`) |
| "Somos varios trabajando" | `git merge`, `git pull` |
| "¿Quién rompió esto?" | `git blame`, `git bisect` |

---

> **Siguiente lección**: `03_los_tres_estados.md` — El concepto más
> importante de Git: los tres espacios donde viven tus archivos.
