# Lección 04: Ejercicio Final Integrador — Deshacer cosas

## Objetivo

Practicar las tres herramientas de deshacer cambios en situaciones
realistas: `git restore`, `git revert` y `git reset`. Saber cuál usar
en cada momento.

## ⚠️ Guía rápida de decisión

```
  ¿Dónde está el error?
  │
  ├── En el Working Directory (aún no hice add)
  │   └── git restore archivo
  │
  ├── En el Staging Area (hice add pero no commit)
  │   └── git restore --staged archivo
  │
  ├── En un commit LOCAL (no lo he pusheado)
  │   ├── Quiero rehacerlo → git reset --soft HEAD~1
  │   ├── Quiero reorganizar → git reset HEAD~1
  │   └── Quiero borrarlo → git reset --hard HEAD~1
  │
  └── En un commit REMOTO (ya hice push)
      └── git revert hash
```

---

## Tarea 1: Error en el Working Directory

Haz cambios accidentales:

```bash
cd ~/practica_git
echo "DROP TABLE menu; -- HACKEADO" >> proyecto/menu.txt
echo "inventario = 0  # BORRAR TODO" >> proyecto/inventario.txt
git status
```

Descarta todos los cambios:

```bash
git restore .
git status
```

✅ Todo limpio. Los cambios maliciosos nunca existieron.

---

## Tarea 2: Error en el Staging Area

Preparaste algo por error:

```bash
echo "Archivo confidencial: contraseñas" > proyecto/secreto.txt
git add proyecto/secreto.txt
git status
```

Quita del staging y elimina:

```bash
git restore --staged proyecto/secreto.txt
rm proyecto/secreto.txt
git status
```

✅ El archivo nunca llegó a un commit.

---

## Tarea 3: Commit con mensaje equivocado

```bash
echo "- Ceviche de corvina — 16.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "asd cambios y cosas varias"
```

Rehaz el commit con buen mensaje:

```bash
git reset --soft HEAD~1
git commit -m "Agrega ceviche de corvina al menú"
git log --oneline -3
```

✅ Mismo contenido, mejor mensaje.

---

## Tarea 4: Commit que mezcla cambios no relacionados

```bash
echo "- Tarta Tatin — 8.00€" >> proyecto/menu.txt
echo "- Moldes para tarta: 4 unidades" >> proyecto/inventario.txt
git add .
git commit -m "Varios cambios mezclados"
```

Sepáralos en commits atómicos:

```bash
git reset HEAD~1

git add proyecto/menu.txt
git commit -m "Agrega tarta Tatin al menú"

git add proyecto/inventario.txt
git commit -m "Agrega moldes para tarta al inventario"

git log --oneline -4
```

✅ Dos commits limpios y enfocados.

---

## Tarea 5: Revertir un commit que ya "se publicó"

Simula que estos commits ya se pushearon (usa revert, no reset):

```bash
echo "## SECCIÓN DE PRUEBA - NO PUBLICAR" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega sección de prueba (error)"
```

Revierte sin alterar el historial:

```bash
git revert HEAD --no-edit
git log --oneline -4
cat proyecto/menu.txt
```

✅ El historial muestra tanto el error como su corrección. El archivo
está limpio.

---

## Tarea 6: El escenario completo

Simula un día donde todo sale mal:

**Paso 1**: Haces un cambio, te das cuenta de que es incorrecto antes
de hacer add.

```bash
echo "PRECIO INCORRECTO" >> proyecto/menu.txt
# Te das cuenta del error
git restore proyecto/menu.txt
```

**Paso 2**: Haces otro cambio, lo preparas, pero cambias de opinión.

```bash
echo "Cambio prematuro" >> proyecto/inventario.txt
git add proyecto/inventario.txt
# Cambias de opinión
git restore --staged proyecto/inventario.txt
git restore proyecto/inventario.txt
```

**Paso 3**: Haces un commit pero el mensaje es malo.

```bash
echo "- Gazpacho de cereza — 7.50€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "fix"
# Rehaz con buen mensaje
git reset --soft HEAD~1
git commit -m "Agrega gazpacho de cereza como entrante de temporada"
```

**Paso 4**: Haces un commit que no debería existir.

```bash
echo "DATOS DE PRUEBA" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega datos de prueba (error)"
# Reviértelo
git revert HEAD --no-edit
```

Verifica el resultado:

```bash
git log --oneline -6
git status
```

✅ El historial refleja las correcciones, y los archivos están limpios.

---

## Autoevaluación

### ¿Cuándo uso cada herramienta?

Responde mentalmente antes de verificar:

1. "Modifiqué un archivo pero no hice add" → **git restore archivo**
2. "Hice add pero quiero sacarlo del staging" → **git restore --staged**
3. "Hice commit pero quiero rehacer el mensaje" → **git reset --soft**
4. "Hice commit y push y necesito deshacer" → **git revert**
5. "Quiero borrar un commit local por completo" → **git reset --hard**

### ¿Tu directorio está limpio?

```bash
git status
```

### ¿El historial tiene sentido?

```bash
git log --oneline -10
```

---

## 🏆 ¡Felicidades!

Has completado el Módulo 6: Deshacer cosas. Ahora sabes:

- ✅ Descartar cambios del disco (`git restore`)
- ✅ Quitar archivos del staging (`git restore --staged`)
- ✅ Deshacer commits publicados (`git revert`)
- ✅ Reescribir historial local (`git reset --soft/mixed/hard`)
- ✅ Recuperar commits perdidos (`git reflog`)
- ✅ Elegir la herramienta correcta según la situación

---

> **Siguiente módulo**: Continúa con el **Módulo 7: Herramientas complementarias**
> → `../modulo_07_herramientas/01_git_stash.md`
