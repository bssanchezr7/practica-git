# Lección 01: Escribir buenos mensajes de commit

## 📖 Teoría

### ¿Por qué importan los mensajes de commit?

Los mensajes de commit son la **documentación viva** de tu proyecto. Según [Pro Git (Capítulo 5.2)](https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project#_commit_guidelines), un buen historial permite que otros desarrolladores (y tú mismo en el futuro) entiendan el contexto y la intención de cada cambio.

Un buen historial:

```
a1b2c3d Corrige cálculo de IVA en platos con descuento
b2c3d4e Agrega menú infantil con tres opciones
c3d4e5f Actualiza precios de entrantes para temporada de verano
d4e5f6a Elimina platos descontinuados del menú
```

Un mal historial:

```
a1b2c3d fix
b2c3d4e cambios
c3d4e5f update
d4e5f6a wip
e5f6a7b asdf
```

El segundo historial es **inútil**. Nadie (ni tú dentro de 2 meses)
podrá entender qué pasó.

### La estructura recomendada

```
<tipo>: <descripción corta en imperativo>     ← Línea 1 (max ~50 chars)
                                               ← Línea 2 vacía
<cuerpo explicativo opcional>                  ← Línea 3+ (max 72 chars/línea)
Explica el "por qué", no el "qué".
El código ya dice qué cambió.
```

### Regla 1: Usa el imperativo

Escribe como si completaras la frase: "Si aplico este commit, ___":

```
  ✅ "Agrega menú infantil"          → Si aplico este commit, agrega menú infantil
  ✅ "Corrige precio de la pasta"    → Si aplico este commit, corrige precio...
  ✅ "Elimina sección obsoleta"      → Si aplico este commit, elimina sección...

  ❌ "Agregado menú infantil"
  ❌ "Corrigiendo precio de la pasta"
  ❌ "Se eliminó la sección obsoleta"
```

### Regla 2: Primera línea corta y descriptiva

Máximo ~50 caracteres. Debe ser un resumen que se entienda sin contexto.

```
  ✅ "Agrega validación de alérgenos al menú"     (43 chars)
  ❌ "Agrega validación de alérgenos al menú del restaurante para cumplir
      con la nueva normativa europea de seguridad alimentaria"  (demasiado largo)
```

### Regla 3: Si necesitas explicar más, usa el cuerpo

```
Corrige cálculo de IVA en platos con descuento

El IVA se estaba calculando sobre el precio original en lugar del
precio con descuento aplicado. Esto generaba cobros incorrectos
del 3-5% en platos con promociones activas.

Afecta a: menú del día, 2x1 martes, happy hour.
```

### Regla 4: Separa el "qué" del "por qué"

El código dice **qué** cambió. El mensaje de commit dice **por qué**.

```
  ❌ "Cambia 10.50 por 11.00 en la línea 23 de menu.txt"
     (Esto ya se ve en el diff)

  ✅ "Actualiza precio de pizza por aumento del coste de mozzarella"
     (Esto NO se ve en el diff — es la motivación)
```

### Prefijos de tipo (Conventional Commits)

Una convención popular es añadir un prefijo que clasifique el cambio, siguiendo la especificación oficial de [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):

| Prefijo | Uso | Ejemplo |
|---------|-----|---------|
| `feat:` | Nueva funcionalidad | `feat: agrega menú infantil` |
| `fix:` | Corrección de error | `fix: corrige precio incorrecto` |
| `docs:` | Documentación | `docs: actualiza README` |
| `style:` | Formato, sin cambio de lógica | `style: ordena secciones del menú` |
| `refactor:` | Reorganización de código | `refactor: separa menú por categorías` |
| `test:` | Tests | `test: agrega pruebas de cálculo de IVA` |
| `chore:` | Mantenimiento | `chore: actualiza dependencias` |

No es obligatorio, pero muchos equipos lo usan como estándar.

### Los 7 pecados capitales de los mensajes de commit

1. **"fix"** — ¿Qué arreglaste?
2. **"cambios"** — ¿Qué cambios?
3. **"WIP"** — Entonces no está listo para commit.
4. **"."** — Sin palabras.
5. **"asdf"** — Esto debería ser ilegal.
6. **Mensajes de 200 caracteres en una línea** — Primera línea corta.
7. **Mensajes en 5 idiomas diferentes** — Elige uno y sé consistente.

---

## 💻 Práctica

### Ejercicio 1: Evalúa mensajes de commit

Para cada mensaje, decide si es bueno o malo y por qué:

1. `"Update menu.txt"`
2. `"Agrega sección de postres con 5 opciones de temporada"`
3. `"fix bug"`
4. `"feat: implementa sistema de reservas online"`
5. `"cambios varios en el inventario y el menú para la semana"`

**Respuestas**:
1. ❌ Demasiado genérico. ¿Qué se actualizó?
2. ✅ Descriptivo y en imperativo.
3. ❌ ¿Qué bug? ¿Dónde?
4. ✅ Usa conventional commits, es claro.
5. ❌ "Cambios varios" no dice nada. Debería ser 2+ commits separados.

---

### Ejercicio 2: Practica escribiendo buenos mensajes

Haz estos cambios y escribe mensajes descriptivos:

```bash
cd ~/practica_git
echo "- Café solo — 1.80€" >> proyecto/menu.txt
echo "- Café con leche — 2.20€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega opciones de café a la carta de bebidas"
```

```bash
echo "- Café en grano: 5 kg" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Registra stock de café en el inventario"
```

```bash
git log --oneline -3
```

✅ Cada commit tiene un mensaje que se entiende sin ver el diff.

---

### Ejercicio 3: Escribe un commit con cuerpo

```bash
echo "" >> proyecto/menu.txt
echo "---" >> proyecto/menu.txt
echo "Precios válidos hasta el 30 de junio de 2026." >> proyecto/menu.txt
echo "IVA incluido en todos los precios." >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "$(cat <<'EOF'
Agrega nota legal de precios al menú

La normativa local requiere indicar la vigencia de los precios
y la inclusión del IVA de forma visible en el menú.
EOF
)"
```

```bash
git log -1
```

✅ Ves la primera línea como título y el cuerpo como explicación.

---

## 🧠 Resumen

| Regla | Ejemplo |
|-------|---------|
| Imperativo | "Agrega", "Corrige", "Elimina" |
| Línea 1 ≤50 chars | `Agrega menú infantil` |
| Cuerpo opcional | Explica el "por qué" |
| Prefijo de tipo | `feat:`, `fix:`, `docs:` |
| Específico | No "cambios", no "update" |

---

> **Siguiente lección**: `02_flujos_trabajo.md` — Aprenderás los flujos
> de trabajo más usados en equipos profesionales.
