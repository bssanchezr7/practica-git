# Curso práctico de Git

## Objetivo

Dominar Git de forma progresiva: desde los conceptos teóricos hasta el manejo
de ramas, pasando por la configuración y el flujo de trabajo diario. Al
terminar, tendrás las herramientas para trabajar con confianza en cualquier
proyecto real.

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
└── proyecto/                              ── ARCHIVOS DE PRÁCTICA ──
    ├── menu.txt                           Menú del restaurante
    └── inventario.txt                     Inventario del restaurante
```

## Cómo seguir el curso

1. Completa cada módulo **en orden** (1 → 2 → 3 → 4).
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

| Lección | Tema                      | Duración |
|---------|---------------------------|----------|
| 01      | Control de versiones      | 10 min   |
| 02      | Problemas que Git resuelve| 10 min   |
| 03      | Los tres estados          | 20 min   |
| 04      | Qué es un commit          | 15 min   |
|         | **Subtotal**              | **~1 h** |

### Módulo 2: Configuración inicial

| Lección | Tema           | Duración |
|---------|----------------|----------|
| 01      | `git config`   | 15 min   |
| 02      | `git init`     | 15 min   |
| 03      | `git clone`    | 15 min   |
|         | **Subtotal**   | **~45 min** |

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

| Lección | Tema                | Duración |
|---------|---------------------|----------|
| 01      | `git branch`        | 20 min   |
| 02      | `git switch`        | 20 min   |
| 03      | `git merge`         | 25 min   |
| 04      | Conflictos de merge | 30 min   |
| 05      | Ejercicio final     | 35 min   |
|         | **Subtotal**        | **~2.5 h** |

|                                      |              |
|--------------------------------------|--------------|
| **TOTAL: 4 módulos, 18 lecciones**   | **~6.5 h**   |

---

> "El mejor momento para aprender Git fue ayer. El segundo mejor momento es ahora."

¡Empieza con `modulo_01_conceptos_base/01_que_es_control_versiones.md` y disfruta el viaje!
