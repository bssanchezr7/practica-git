# Lección 06: Ejercicio Final Integrador

## Objetivo

Poner en práctica **todo** lo aprendido en las lecciones 01-05 en un escenario
realista. Vas a simular un día de trabajo completo en el restaurante "El Buen
Código", creando un historial de commits limpio y profesional.

## ⚠️ Reglas del ejercicio

1. Ejecuta `git status` antes de cada `git add` y cada `git commit`.
2. Ejecuta `git diff` (o `git diff --staged`) antes de cada commit para
   verificar qué vas a guardar.
3. Cada commit debe tener un mensaje descriptivo y en imperativo.
4. No uses `git add .` ni `git commit -a` — prepara los archivos uno a uno
   para practicar selectividad.

---

## Escenario

Es lunes por la mañana. Eres el encargado del restaurante "El Buen Código"
y tienes que hacer varias actualizaciones al menú y al inventario.

---

## Tarea 1: Actualización de precios

Los precios de algunos platos han cambiado. Abre `proyecto/menu.txt` con tu
editor y haz los siguientes cambios:

1. Cambia el precio del "Spaghetti carbonara" de 12.00€ a **13.50€**.
2. Cambia el precio de la "Pizza margarita" de 10.50€ a **11.00€**.

Ahora sigue estos pasos:

```bash
# 1. Verifica qué cambió
git status

# 2. Ve el detalle exacto de los cambios
git diff proyecto/menu.txt

# 3. Prepara SOLO menu.txt
git add proyecto/menu.txt

# 4. Verifica qué vas a commitear
git diff --staged

# 5. Confirma los cambios
git commit -m "Actualiza precios de spaghetti y pizza"

# 6. Verifica que todo está limpio
git status
```

---

## Tarea 2: Nuevo plato del día

El chef ha creado un nuevo plato del día. Añade al final de `proyecto/menu.txt`:

```
## Plato del día (Lunes)
- Paella valenciana — 15.00€
  Incluye: arroz, mariscos, azafrán y verduras frescas.
```

Flujo:

```bash
git status
git diff
git add proyecto/menu.txt
git diff --staged
git commit -m "Agrega paella valenciana como plato del día"
git status
```

---

## Tarea 3: Recepción de mercancía

Ha llegado un pedido del proveedor. Actualiza `proyecto/inventario.txt`
añadiendo al final:

```
## Recepción de mercancía - Lunes
- Tomates: 30 kg
- Aceite de oliva: 15 litros
- Arroz bomba: 20 kg
- Gambas frescas: 10 kg
- Azafrán: 100 g
```

Flujo:

```bash
git status
git diff
git add proyecto/inventario.txt
git diff --staged
git commit -m "Registra recepción de mercancía del lunes"
git status
```

---

## Tarea 4: Múltiples cambios, un solo commit relacionado

El inspector de sanidad ha pedido que se añada la información de alérgenos.
Modifica AMBOS archivos:

En `proyecto/menu.txt`, añade al final:

```
---
Nota: Consulte con el personal sobre alérgenos e intolerancias.
```

En `proyecto/inventario.txt`, añade al final:

```
## Etiquetado de alérgenos
- Todos los productos han sido verificados y etiquetados.
- Última revisión: Lunes
```

Ahora prepara y confirma AMBOS en un solo commit:

```bash
git status
git diff
git add proyecto/menu.txt
git add proyecto/inventario.txt
git diff --staged
git commit -m "Agrega información de alérgenos al menú e inventario"
git status
```

💡 Estos cambios van juntos porque son parte de la misma tarea
(cumplir con la normativa de alérgenos).

---

## Tarea 5: Revisión del historial

Ahora que tienes un historial rico, explóralo:

```bash
# 1. Vista completa
git log

# 2. Vista compacta
git log --oneline

# 3. ¿Cuántos commits tienes en total?
git log --oneline | wc -l

# 4. ¿Qué archivos tocaste en los últimos 4 commits?
git log --stat -4

# 5. ¿Qué cambió exactamente en el commit de precios?
#    (busca el commit por su mensaje)
git log --grep="precios" -p

# 6. Compara el estado actual con el primer commit
#    (usa el hash que aparece en git log --oneline para el primer commit)
git diff <hash-primer-commit> HEAD --stat
```

---

## Tarea 6: El error descubierto a tiempo

Acabas de notar que el precio de la paella debería ser 16.00€, no 15.00€.

1. Haz el cambio en `proyecto/menu.txt`.
2. Antes de prepararlo, revisa con `git diff` que solo cambiaste lo que
   querías cambiar.
3. Prepara, verifica con `git diff --staged` y confirma.

```bash
# Haz el cambio en menu.txt (cambia 15.00 por 16.00)
git diff
git add proyecto/menu.txt
git diff --staged
git commit -m "Corrige precio de la paella de 15.00€ a 16.00€"
```

---

## Autoevaluación

Cuando termines, responde estas preguntas ejecutando comandos:

### Pregunta 1: ¿Cuántos commits has creado en total?

```bash
git log --oneline | wc -l
```

### Pregunta 2: ¿Está tu working directory limpio?

```bash
git status
```

Debería decir: "nothing to commit, working tree clean".

### Pregunta 3: ¿Puedes ver el historial completo en una línea?

```bash
git log --oneline --graph
```

### Pregunta 4: ¿Qué archivos has modificado en todo el curso?

```bash
git diff --stat HEAD~6 HEAD
```

### Pregunta 5: ¿Hay algún commit con cambios en ambos archivos?

```bash
git log --oneline --stat | head -30
```

Busca un commit que muestre ambos archivos — debería ser el de alérgenos.

---

## 🏆 ¡Felicidades!

Si llegaste hasta aquí, ya dominas el flujo básico de trabajo en Git:

```
┌──────────┐    git add    ┌──────────┐   git commit  ┌──────────┐
│  Editar  │──────────────▶│ Preparar │──────────────▶│ Guardar  │
│  archivos│               │ cambios  │               │ en       │
│          │               │          │               │ historial│
└──────────┘               └──────────┘               └──────────┘
      ↑                         ↑                          │
      │    git status      git diff --staged          git log
      │    git diff                                    git log -p
      │                                                    │
      └────────────────────────────────────────────────────┘
                    Inspeccionar siempre
```

### Lo que ahora sabes hacer:

- ✅ Inspeccionar el estado del proyecto (`git status`)
- ✅ Preparar cambios selectivamente (`git add`)
- ✅ Crear commits limpios con mensajes descriptivos (`git commit`)
- ✅ Explorar el historial con múltiples formatos (`git log`)
- ✅ Comparar versiones a cualquier nivel de detalle (`git diff`)

### Próximos pasos sugeridos:

1. **Ramas** (`git branch`, `git switch`, `git merge`) — Trabaja en paralelo.
2. **Remotos** (`git push`, `git pull`) — Colabora con otros.
3. **Deshacer** (`git restore`, `git revert`) — Corrige errores.

---

> "No se trata de memorizar comandos, sino de entender el flujo.
> Los comandos vendrán solos con la práctica."
