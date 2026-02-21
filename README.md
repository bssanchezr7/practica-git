# Curso práctico de Git

## Objetivo

Dominar Git de forma progresiva: desde los conceptos teóricos hasta las buenas
prácticas profesionales, pasando por el flujo de trabajo, ramas, remotos,
deshacer cambios y herramientas complementarias. Al terminar, tendrás las
herramientas para trabajar con confianza en cualquier proyecto real.

## Requisitos previos

- Tener Git instalado (`git --version` para verificar).
- Una terminal abierta en esta carpeta (`~/practica_git`).
- Ganas de experimentar: en Git, romper cosas es la mejor forma de aprender.

## Estructura del curso

```
practica_git/
│
├── README.md                              ← Estás aquí
│
├── modulo_01_conceptos_base/              ── MÓDULO 1 ──
│   ├── 01_que_es_control_versiones.md     Sistemas de control de versiones
│   ├── 02_que_problema_resuelve_git.md    Los 5 problemas que Git resuelve
│   ├── 03_los_tres_estados.md             Working Dir / Staging / Repository
│   └── 04_que_es_un_commit.md             Snapshots, hashes e inmutabilidad
│
├── modulo_02_configuracion/               ── MÓDULO 2 ──
│   ├── 01_git_config.md                   Tu identidad y preferencias
│   ├── 02_git_init.md                     Crear un repositorio desde cero
│   └── 03_git_clone.md                    Clonar un repositorio existente
│
├── modulo_03_flujo_basico/                ── MÓDULO 3 ──
│   ├── 01_git_status.md                   Inspeccionar el estado
│   ├── 02_git_add.md                      Preparar cambios
│   ├── 03_git_commit.md                   Guardar instantáneas
│   ├── 04_git_log.md                      Explorar el historial
│   ├── 05_git_diff.md                     Comparar versiones
│   └── 06_ejercicio_final.md              Ejercicio integrador
│
├── modulo_04_ramas/                       ── MÓDULO 4 ──
│   ├── 01_git_branch.md                   Crear, listar y eliminar ramas
│   ├── 02_git_switch.md                   Cambiar de rama
│   ├── 03_git_merge.md                    Unir ramas
│   ├── 04_conflictos.md                   Resolver conflictos de merge
│   └── 05_ejercicio_final.md              Ejercicio integrador
│
├── modulo_05_remotos/                     ── MÓDULO 5 ──
│   ├── 01_git_remote.md                   Conectar con repositorios remotos
│   ├── 02_git_push.md                     Enviar cambios al remoto
│   ├── 03_git_pull_fetch.md               Traer cambios del remoto
│   └── 04_ejercicio_final.md              Ejercicio integrador
│
├── modulo_06_deshacer/                    ── MÓDULO 6 ──
│   ├── 01_git_restore.md                  Descartar cambios
│   ├── 02_git_revert.md                   Deshacer commits (seguro)
│   ├── 03_git_reset.md                    Reescribir historial (con cuidado)
│   └── 04_ejercicio_final.md              Ejercicio integrador
│
├── modulo_07_herramientas/                ── MÓDULO 7 ──
│   ├── 01_git_stash.md                    Guardar cambios temporalmente
│   ├── 02_git_tag.md                      Marcar versiones importantes
│   ├── 03_gitignore.md                    Excluir archivos del seguimiento
│   └── 04_ejercicio_final.md              Ejercicio integrador
│
├── modulo_08_buenas_practicas/            ── MÓDULO 8 ──
│   ├── 01_mensajes_commit.md              Escribir buenos mensajes
│   ├── 02_flujos_trabajo.md               Feature Branch, Git Flow, Trunk
│   └── 03_consejos_profesionales.md       20 consejos + cheat sheet final
│
└── proyecto/                              ── ARCHIVOS DE PRÁCTICA ──
    ├── menu.txt                           Menú del restaurante
    └── inventario.txt                     Inventario del restaurante
```

## Cómo seguir el curso

1. Completa cada módulo **en orden** (1 → 8).
2. Dentro de cada módulo, sigue las lecciones en secuencia.
3. Cada lección tiene una sección **Teoría** y una sección **Práctica**.
4. Lee primero la teoría completa, luego ejecuta los ejercicios uno a uno.
5. No tengas miedo de experimentar más allá de lo que pide el ejercicio.

## Convención de íconos en las lecciones

- 📖 Teoría
- 💻 Práctica
- ⚠️  Importante
- 💡 Consejo
- ✅ Verificación (resultado esperado)

## Tiempo estimado

### Módulo 1: Conceptos base

| Lección | Tema                       | Duración |
|---------|----------------------------|----------|
| 01      | Control de versiones       | 10 min   |
| 02      | Problemas que Git resuelve | 10 min   |
| 03      | Los tres estados           | 20 min   |
| 04      | Qué es un commit           | 15 min   |
|         | **Subtotal**               | **~1 h** |

### Módulo 2: Configuración inicial

| Lección | Tema           | Duración   |
|---------|----------------|------------|
| 01      | `git config`   | 15 min     |
| 02      | `git init`     | 15 min     |
| 03      | `git clone`    | 15 min     |
|         | **Subtotal**   | **~45 min**|

### Módulo 3: Flujo básico de trabajo

| Lección | Tema              | Duración |
|---------|-------------------|----------|
| 01      | `git status`      | 15 min   |
| 02      | `git add`         | 20 min   |
| 03      | `git commit`      | 20 min   |
| 04      | `git log`         | 20 min   |
| 05      | `git diff`        | 25 min   |
| 06      | Ejercicio final   | 30 min   |
|         | **Subtotal**      | **~2 h** |

### Módulo 4: Ramas (branches)

| Lección | Tema                | Duración   |
|---------|---------------------|------------|
| 01      | `git branch`        | 20 min     |
| 02      | `git switch`        | 20 min     |
| 03      | `git merge`         | 25 min     |
| 04      | Conflictos de merge | 30 min     |
| 05      | Ejercicio final     | 35 min     |
|         | **Subtotal**        | **~2.5 h** |

### Módulo 5: Trabajo con remotos

| Lección | Tema                    | Duración   |
|---------|-------------------------|------------|
| 01      | `git remote`            | 15 min     |
| 02      | `git push`              | 20 min     |
| 03      | `git pull` / `git fetch`| 25 min     |
| 04      | Ejercicio final         | 30 min     |
|         | **Subtotal**            | **~1.5 h** |

### Módulo 6: Deshacer cosas

| Lección | Tema              | Duración   |
|---------|-------------------|------------|
| 01      | `git restore`     | 20 min     |
| 02      | `git revert`      | 20 min     |
| 03      | `git reset`       | 25 min     |
| 04      | Ejercicio final   | 25 min     |
|         | **Subtotal**      | **~1.5 h** |

### Módulo 7: Herramientas complementarias

| Lección | Tema              | Duración   |
|---------|-------------------|------------|
| 01      | `git stash`       | 20 min     |
| 02      | `git tag`         | 15 min     |
| 03      | `.gitignore`      | 20 min     |
| 04      | Ejercicio final   | 25 min     |
|         | **Subtotal**      | **~1.5 h** |

### Módulo 8: Buenas prácticas

| Lección | Tema                    | Duración |
|---------|-------------------------|----------|
| 01      | Mensajes de commit      | 15 min   |
| 02      | Flujos de trabajo       | 20 min   |
| 03      | Consejos profesionales  | 15 min   |
|         | **Subtotal**            | **~50 min** |

|                                      |              |
|--------------------------------------|--------------|
| **TOTAL: 8 módulos, 30 lecciones**   | **~12 h**    |

---

> "El mejor momento para aprender Git fue ayer. El segundo mejor momento es ahora."

¡Empieza con `modulo_01_conceptos_base/01_que_es_control_versiones.md` y disfruta el viaje!
