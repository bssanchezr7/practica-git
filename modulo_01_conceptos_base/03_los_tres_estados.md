# Lección 03: Los tres estados de Git

## 📖 Teoría

### El concepto más importante de Git

Si solo pudieras aprender UNA cosa de Git, debería ser esto: tus archivos
existen en **tres espacios** y se mueven entre ellos con comandos específicos.
Todo lo demás en Git gira alrededor de esta idea.

### Los tres espacios

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   ① WORKING DIRECTORY    ② STAGING AREA     ③ REPOSITORY       │
│   (Directorio de trabajo)   (Área de preparación)  (.git/)     │
│                                                                 │
│   ┌──────────────────┐   ┌──────────────┐   ┌──────────────┐  │
│   │                  │   │              │   │              │  │
│   │  Aquí editas     │   │  Aquí eliges │   │  Aquí se     │  │
│   │  tus archivos    │──▶│  qué guardar │──▶│  guarda el   │  │
│   │  normalmente     │   │  en el       │   │  historial   │  │
│   │                  │   │  próximo     │   │  permanente  │  │
│   │  (tu carpeta)    │   │  commit      │   │  de commits  │  │
│   │                  │   │              │   │              │  │
│   └──────────────────┘   └──────────────┘   └──────────────┘  │
│                                                                 │
│          git add ──────▶         git commit ──────▶             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### ① Working Directory (Directorio de trabajo)

Es simplemente la **carpeta** de tu proyecto en tu disco duro. Cuando abres
un archivo, lo editas y guardas con Ctrl+S, estás trabajando aquí.

**Analogía**: Es tu **escritorio de trabajo** donde tienes papeles
esparcidos, algunos terminados, otros a medio hacer.

### ② Staging Area (Área de preparación)

También llamada **Index**. Es una zona intermedia donde colocas los cambios
que quieres incluir en tu próximo commit. No es una carpeta visible — es
un concepto interno de Git.

**Analogía**: Es la **bandeja de salida** en tu escritorio. Pones los
documentos terminados ahí antes de enviarlos. Puedes añadir y quitar
cosas de la bandeja hasta que estés satisfecho.

**¿Por qué existe?** Para darte **control**. No siempre quieres guardar
todos tus cambios de golpe. A veces modificas 5 archivos pero solo 2
están listos. El Staging Area te permite elegir exactamente qué va en
cada commit.

### ③ Repository (Repositorio)

Es la **base de datos** de Git, almacenada en la carpeta oculta `.git/`
dentro de tu proyecto. Aquí se guardan todos los commits (instantáneas)
de tu proyecto desde el principio de los tiempos.

**Analogía**: Es el **archivo** de tu oficina, donde se almacenan
todos los documentos enviados, con fecha y registro. Una vez archivado,
queda guardado permanentemente.

### El flujo completo

```
   Editas un        Lo seleccionas      Lo guardas en
   archivo          para guardar        el historial
      │                  │                   │
      ▼                  ▼                   ▼
  ┌────────┐  git add  ┌─────────┐  git commit  ┌────────────┐
  │Working │─────────▶│ Staging │──────────────▶│ Repository │
  │Directory│         │  Area   │               │  (.git/)   │
  └────────┘          └─────────┘               └────────────┘
```

**En resumen**:
1. **Editas** archivos (Working Directory).
2. **Seleccionas** qué cambios guardar (`git add` → Staging Area).
3. **Confirmas** los cambios (`git commit` → Repository).

### Los estados de un archivo

Un archivo puede estar en estos estados:

```
  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
  │  UNTRACKED  │     │  MODIFIED   │     │   STAGED    │
  │  (nuevo,    │     │  (conocido, │     │  (listo     │
  │   Git no lo │     │   cambió)   │     │   para el   │
  │   conoce)   │     │             │     │   commit)   │
  └──────┬──────┘     └──────┬──────┘     └──────┬──────┘
         │                   │                   │
         │    git add        │    git add        │  git commit
         └───────────────────┴───────────────────┴──────────▶
                                                       │
                                                  COMMITTED
                                                  (guardado)
```

| Estado | Significado | Dónde está |
|--------|------------|-----------|
| **Untracked** | Git no sabe que existe | Working Directory |
| **Modified** | Git lo conoce, pero cambió | Working Directory |
| **Staged** | Marcado para el próximo commit | Staging Area |
| **Committed** | Guardado en el historial | Repository |

### La diferencia clave: guardar vs commitear

| Ctrl+S (guardar) | git commit |
|-------------------|------------|
| Guarda en tu disco (Working Directory) | Guarda en el historial de Git (Repository) |
| Sobrescribe el archivo | Crea una nueva instantánea sin borrar las anteriores |
| No hay historial | Historial completo |
| No puedes volver atrás | Puedes volver a cualquier commit |

### ⚠️ Importante

- El Staging Area es lo que hace Git especial. Otros sistemas no lo tienen.
- Un archivo puede estar **parcialmente** en el staging: algunos cambios
  preparados y otros no.
- `git status` es el comando que te dice en qué estado está cada archivo.

---

## 💻 Práctica

### Ejercicio 1: Visualiza los tres estados

Desde `~/practica_git`, vamos a observar un archivo moviéndose entre los
tres estados.

Primero, veamos el estado actual:

```bash
cd ~/practica_git
git status
```

✅ Todo debería estar limpio ("nothing to commit").

---

### Ejercicio 2: Working Directory → Staging Area → Repository

Crea un archivo nuevo (estará **untracked**):

```bash
echo "Archivo de prueba para entender los tres estados" > prueba_estados.txt
git status
```

✅ Ves `prueba_estados.txt` en rojo como "Untracked files".
El archivo existe en el Working Directory, pero Git no lo conoce.

Muévelo al Staging Area:

```bash
git add prueba_estados.txt
git status
```

✅ Ahora está en verde como "Changes to be committed" → está en el
Staging Area.

Guárdalo en el Repository:

```bash
git commit -m "Agrega archivo de prueba para entender los estados"
git status
```

✅ "nothing to commit" → El archivo está en el Repository. Committed.

---

### Ejercicio 3: Observa el estado "Modified"

Modifica el archivo:

```bash
echo "Esta es una línea nueva" >> prueba_estados.txt
git status
```

✅ El archivo aparece en rojo como "Changes not staged for commit" →
está **modified** en el Working Directory.

---

### Ejercicio 4: El archivo en dos estados a la vez

Prepáralo:

```bash
git add prueba_estados.txt
```

Ahora modifícalo OTRA VEZ:

```bash
echo "Otra línea más" >> prueba_estados.txt
git status
```

✅ **Resultado esperado**: El mismo archivo aparece **dos veces**:
- En verde (staged): la versión que preparaste.
- En rojo (modified): los cambios nuevos.

💡 Esto demuestra que el Staging Area guarda una **foto** del archivo
en el momento del `git add`, no un enlace al archivo.

---

### Ejercicio 5: Limpieza

```bash
git restore prueba_estados.txt
git add prueba_estados.txt
git commit -m "Actualiza archivo de prueba con una línea nueva"
rm prueba_estados.txt
git add prueba_estados.txt
git commit -m "Elimina archivo temporal de prueba"
git status
```

✅ Directorio limpio de nuevo.

---

## 🧠 Resumen

```
             git add              git commit
Working Dir ──────────▶ Staging ──────────────▶ Repository
(editas)               (seleccionas)            (guardas)
```

| Espacio | Qué es | Comando para mover aquí |
|---------|--------|------------------------|
| Working Directory | Tu carpeta real | (editas normalmente) |
| Staging Area | Zona de preparación | `git add` |
| Repository | Historial permanente | `git commit` |

**Regla de oro**: El flujo siempre es el mismo: editar → add → commit.
Una vez que esto sea automático, ya sabes Git.

---

> **Siguiente lección**: `04_que_es_un_commit.md` — Entenderás en
> profundidad qué es exactamente un commit y por qué es tan poderoso.
