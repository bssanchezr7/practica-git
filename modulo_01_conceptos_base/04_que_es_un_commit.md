# Lección 04: ¿Qué es un commit?

## 📖 Teoría

### Un commit es una instantánea

Un commit **NO** es una lista de cambios (diff). Es una **foto completa**
(snapshot) de todos los archivos de tu proyecto en un momento dado.

**Analogía**: Piensa en los puntos de guardado de un videojuego. Cada commit
captura el estado exacto del juego — posición del personaje, inventario,
progreso, todo. Puedes tener cientos de guardados y volver a cualquiera.

```
  Commit 1            Commit 2            Commit 3
  ┌───────────┐      ┌───────────┐      ┌───────────┐
  │ 📸 Foto   │      │ 📸 Foto   │      │ 📸 Foto   │
  │ completa  │─────▶│ completa  │─────▶│ completa  │
  │ del       │      │ del       │      │ del       │
  │ proyecto  │      │ proyecto  │      │ proyecto  │
  │           │      │           │      │           │
  │ 10:00 AM  │      │ 11:30 AM  │      │ 02:15 PM  │
  └───────────┘      └───────────┘      └───────────┘
```

### Anatomía de un commit

Cada commit almacena:

```
┌─────────────────────────────────────────────────┐
│                    COMMIT                        │
│                                                 │
│  ┌─────────────────────────────────────────┐    │
│  │ Hash: a1b2c3d4e5f6...                   │    │
│  │ (identificador único, generado por Git) │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  Autor:    Tu Nombre <tu@email.com>             │
│  Fecha:    2026-02-20 10:30:00                  │
│  Mensaje:  "Agrega menú del restaurante"        │
│                                                 │
│  Padre:    f4e5d6a (el commit anterior)         │
│                                                 │
│  Contenido (snapshot):                          │
│    ├── README.md .......... (versión 3)         │
│    ├── proyecto/menu.txt .. (versión 2)         │
│    └── proyecto/inv.txt ... (versión 1)         │
│                                                 │
└─────────────────────────────────────────────────┘
```

#### 1. Hash SHA-1

Un identificador único de **40 caracteres hexadecimales**. Git lo calcula
a partir del contenido del commit. Es imposible que dos commits diferentes
tengan el mismo hash.

```
a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0
└──────┘
  ↑ Normalmente solo se muestran los primeros 7 caracteres
```

#### 2. Autor y fecha

Quién creó el commit y cuándo. Git lo registra automáticamente usando
tu configuración (`git config`).

#### 3. Mensaje

Una descripción legible de qué contiene este commit y por qué. Es lo
más importante para los humanos que lean el historial después.

#### 4. Padre(s)

El commit (o commits) anterior(es). Esto forma la **cadena** del historial:

```
  C1 ← C2 ← C3 ← C4
                    ↑
                  HEAD (estás aquí)

  Cada commit apunta a su padre, formando el historial.
```

El primer commit del repositorio no tiene padre (se llama "root commit").
Un commit de merge tiene dos padres.

#### 5. Contenido (snapshot)

La foto completa de todos los archivos rastreados. Git es inteligente:
no duplica archivos que no cambiaron — solo guarda una referencia.

### Snapshots, no diffs

Este es un concepto crucial. Otros sistemas guardan **listas de cambios**
(diffs). Git guarda **fotos completas** (snapshots):

```
  OTROS SISTEMAS (basados en diffs):

  Versión 1    Δ1      Δ2      Δ3
  ┌──────┐ + ┌───┐ + ┌───┐ + ┌───┐ = Estado actual
  │ Base │   │+3 │   │-1 │   │+5 │
  └──────┘   │lin│   │lin│   │lin│
              └───┘   └───┘   └───┘

  Para reconstruir el estado actual, necesitas aplicar todos los deltas.


  GIT (basado en snapshots):

  Commit 1     Commit 2     Commit 3     Commit 4
  ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐
  │ Foto │    │ Foto │    │ Foto │    │ Foto │
  │compl.│    │compl.│    │compl.│    │compl.│
  └──────┘    └──────┘    └──────┘    └──────┘

  Cada commit ES el estado completo. No necesitas calcular nada.
```

**Ventaja**: Acceder a cualquier versión es instantáneo. No necesitas
"rebobinar" diffs.

**Eficiencia**: Git no duplica archivos sin cambios. Usa referencias
internas para archivos idénticos entre commits.

### Commits son inmutables

Una vez creado, un commit **nunca cambia**. Su hash se calcula a partir
de su contenido — si algo cambiara, el hash sería diferente, y sería
un commit distinto.

Esto significa:
- El historial es **confiable**: nadie puede alterar el pasado sin que
  se note.
- "Modificar" un commit realmente significa crear uno nuevo que lo
  reemplaza.

### Buenos mensajes de commit

El mensaje es la única parte del commit pensada para humanos. Invertir
30 segundos en escribir un buen mensaje te ahorra horas de confusión
después.

```
  ❌ MAL                              ✅ BIEN
  ────────────────────────            ────────────────────────────────
  "cambios"                           "Agrega sección de postres al menú"
  "fix"                               "Corrige precio incorrecto de la pasta"
  "wip"                               "Actualiza inventario con pedido del lunes"
  "cosas"                             "Elimina platos fuera de temporada"
  "asdfjkl"                           "Reorganiza menú por categorías"
```

**Reglas prácticas**:
1. Usa **imperativo**: "Agrega", "Corrige", "Elimina" (no "Agregado",
   "Corrigiendo").
2. Primera línea: **máximo ~50 caracteres**.
3. Responde a: "Si aplico este commit, ___" → "Agrega postres al menú".

---

## 💻 Práctica

### Ejercicio 1: Examina un commit existente

Veamos los commits que ya existen en este repositorio:

```bash
cd ~/practica_git
git log --oneline
```

Elige cualquier hash de la lista y examínalo en detalle:

```bash
git show <hash>
```

(Reemplaza `<hash>` por los 7 caracteres de un commit.)

✅ **Resultado esperado**: Ves toda la información del commit: hash,
autor, fecha, mensaje y el diff de los cambios.

---

### Ejercicio 2: Ve solo los metadatos

```bash
git log -1 --format="Hash:    %H%nAutor:   %an <%ae>%nFecha:   %ai%nMensaje: %s"
```

✅ Ves los componentes del último commit de forma estructurada.

---

### Ejercicio 3: Entiende la cadena de padres

```bash
git log --oneline --graph
```

✅ Cada `*` es un commit. Las líneas muestran la relación padre→hijo.
El commit más reciente está arriba.

---

### Ejercicio 4: Comprueba que los commits son inmutables

Mira el hash del último commit:

```bash
git log -1 --format="%H"
```

Apunta ese hash. Ahora haz un nuevo commit:

```bash
echo "Probando inmutabilidad" > /tmp/inmutable_test.txt
cp /tmp/inmutable_test.txt prueba_inmutable.txt
git add prueba_inmutable.txt
git commit -m "Commit temporal para probar inmutabilidad"
```

Mira el hash del nuevo commit:

```bash
git log -1 --format="%H"
```

Y verifica que el commit anterior sigue exacto:

```bash
git log -2 --format="%H %s"
```

✅ El commit anterior tiene el mismo hash de antes. No cambió. Los
commits son inmutables.

Limpia:

```bash
rm prueba_inmutable.txt
git add prueba_inmutable.txt
git commit -m "Elimina archivo temporal de prueba de inmutabilidad"
```

---

## 🧠 Resumen

| Concepto | Detalle |
|----------|---------|
| Commit | Instantánea completa del proyecto en un momento |
| Hash SHA-1 | Identificador único de 40 caracteres |
| Padre | El commit anterior (forma la cadena del historial) |
| Snapshot | Foto completa, no una lista de cambios |
| Inmutable | Una vez creado, nunca cambia |
| Mensaje | Descripción legible para humanos |

```
  Los commits forman una cadena (el historial):

  C1 ← C2 ← C3 ← C4 ← C5
                          ↑
                        HEAD (presente)

  Puedes ir a cualquier punto. Ninguno desaparece.
```

---

> **Siguiente módulo**: Continúa con el **Módulo 2: Configuración inicial**
> → `../modulo_02_configuracion/01_git_config.md`
