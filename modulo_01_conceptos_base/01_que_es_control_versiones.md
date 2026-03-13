# Lección 01: ¿Qué es un sistema de control de versiones?

## 📖 Teoría

### El problema

Imagina que estás escribiendo un documento importante. Haces cambios, guardas.
Más tarde haces otros cambios, guardas de nuevo. Al día siguiente te das
cuenta de que los cambios de ayer eran mejores. ¿Cómo vuelves atrás?

La "solución" clásica:

```
informe.txt
informe_v2.txt
informe_v2_final.txt
informe_v2_final_DEFINITIVO.txt
informe_v2_final_DEFINITIVO_ahora_si.txt
```

¿Te suena? Este caos tiene nombre: **el problema del control de versiones
manual**. Y tiene consecuencias reales:

- No sabes qué cambió entre una versión y otra.
- No sabes cuándo se hizo cada cambio ni por qué.
- Si trabajan varias personas, el desastre se multiplica.
- Recuperar un estado anterior es una pesadilla.

### La solución: sistemas de control de versiones (VCS)

Un **sistema de control de versiones** (VCS, por sus siglas en inglés) es una herramienta que registra los cambios en tus archivos a lo largo del tiempo, de modo que puedas recuperar versiones específicas más adelante. Según el libro oficial [Pro Git (Capítulo 1.1)](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control), esta capacidad es fundamental para cualquier proyecto de software. Te permite:

| Capacidad | Sin VCS | Con VCS |
|-----------|---------|---------|
| Volver a una versión anterior | Buscar en tu carpeta de "backups" | Un comando |
| Saber qué cambió y cuándo | Ni idea | Registro detallado automático |
| Trabajar en equipo sin pisarse | "Oye, no toques ese archivo" | Cambios paralelos que se combinan |
| Experimentar sin miedo | "Mejor no toco nada" | Creas una rama, pruebas, y decides |
| Recuperar archivos borrados | Rezar | Un comando |

### Tipos de control de versiones

#### 1. Local

El más primitivo. Copias de carpetas en tu propio disco.

```
  ┌──────────────────┐
  │    Tu máquina     │
  │                  │
  │  Versión 1       │
  │  Versión 2       │
  │  Versión 3       │
  └──────────────────┘
```

**Problema**: Si tu disco muere, lo pierdes todo. No permite colaborar.

#### 2. Centralizado (CVCS - Centralized Version Control Systems)

Un servidor central guarda el historial. Todos se conectan a él (ejemplos: CVS, Subversion, Perforce).

Un servidor central guarda el historial. Todos se conectan a él.

```
  ┌──────────────┐
  │   Servidor    │ ← Historial completo aquí
  └──────┬───────┘
         │
    ┌────┼────┐
    │    │    │
  Dev1  Dev2  Dev3   ← Solo la última versión
```

**Problema**: Si el servidor cae, nadie puede trabajar. Un solo punto de
fallo.

#### 3. Distribuido (DVCS - Distributed Version Control Systems)

Cada persona tiene una **copia completa** del historial (ejemplos: Git, Mercurial, Bazaar). No hay un punto único de fallo.

```
  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
  │   Dev 1      │     │   Dev 2      │     │   Dev 3      │
  │  Historial   │◀───▶│  Historial   │◀───▶│  Historial   │
  │  completo    │     │  completo    │     │  completo    │
  └──────────────┘     └──────────────┘     └──────────────┘
                             │
                       ┌─────┴─────┐
                       │ Servidor  │ ← Opcional, para sincronizar
                       │ (GitHub)  │
                       └───────────┘
```

**Git es un sistema de control de versiones distribuido.** Es el más usado
en el mundo y el estándar de la industria.

---

## 💻 Práctica

### Ejercicio 1: Experimenta el problema

Crea una carpeta temporal y simula el caos de las versiones manuales:

```bash
mkdir /tmp/sin_git && cd /tmp/sin_git
echo "Mi primer informe" > informe.txt
cp informe.txt informe_v2.txt
echo "Mi informe mejorado" > informe_v2.txt
cp informe_v2.txt informe_v2_final.txt
echo "Versión definitiva" > informe_v2_final.txt
```

Ahora contesta:

```bash
ls -la
```

- ¿Cuál es la versión actual?
- ¿Qué cambió entre cada versión?
- ¿Quién hizo los cambios? ¿Cuándo?

✅ No puedes responder con certeza a ninguna pregunta. Ese es el problema
que Git resuelve.

Limpia:

```bash
rm -rf /tmp/sin_git
```

---

### Ejercicio 2: Verifica que tienes Git instalado

```bash
git --version
```

✅ **Resultado esperado**: Algo como `git version 2.x.x`. Si ves un error,
necesitas instalar Git antes de continuar.

---

### Ejercicio 3: Explora la ayuda de Git

```bash
git help
```

✅ Verás una lista de los comandos más comunes. No necesitas memorizarlos
ahora — es solo para que sepas que la ayuda existe.

Para ver ayuda detallada de un comando específico:

```bash
git help commit
```

(Usa `q` para salir de la ayuda.)

---

## 🧠 Resumen

| Concepto | Detalle |
|----------|---------|
| VCS | Herramienta que registra cambios en archivos |
| VCS local | Copias en tu disco (frágil, sin colaboración) |
| VCS centralizado | Un servidor central (punto único de fallo) |
| VCS distribuido | Cada persona tiene el historial completo |
| Git | VCS distribuido, el estándar actual |

---

> **Siguiente lección**: `02_que_problema_resuelve_git.md` — Descubrirás
> en concreto qué problemas resuelve Git y por qué cambió la industria.
