# Lección 04: Ejercicio Final Integrador — Herramientas complementarias

## Objetivo

Combinar `git stash`, `git tag` y `.gitignore` en un escenario realista
donde el restaurante prepara un lanzamiento de nueva temporada.

---

## Escenario

Es viernes. El restaurante "El Buen Código" va a lanzar su menú de
primavera el lunes. Tienes que preparar la versión final, etiquetar el
release, y asegurarte de que los archivos temporales no entren al
repositorio.

---

## Tarea 1: Configura el `.gitignore` del proyecto

Antes de empezar, asegúrate de que estos archivos nunca entren al repo:

```bash
cd ~/practica_git
cat >> .gitignore << 'EOF'

# Archivos de planificación interna (no publicar)
*.draft
planning/
notas_internas.txt

# Archivos de pruebas de menú
*_test.txt
EOF

git add .gitignore
git commit -m "Actualiza .gitignore para el lanzamiento de primavera"
```

Verifica creando archivos que deberían ignorarse:

```bash
echo "Ideas sueltas..." > menu_primavera.draft
echo "Prueba de precios" > precios_test.txt
mkdir planning && echo "Calendario" > planning/calendario.txt
git status
```

✅ Ninguno aparece. Están correctamente ignorados.

---

## Tarea 2: Marca la versión actual antes de cambios

Antes de hacer cambios, etiqueta el estado actual:

```bash
git tag -a v1.2.0 -m "Versión 1.2: menú de invierno (pre-primavera)"
git tag
```

✅ Ahora siempre puedes volver a esta versión exacta.

---

## Tarea 3: Trabaja en el menú de primavera

```bash
echo "" >> proyecto/menu.txt
echo "## Menú de Primavera 2026" >> proyecto/menu.txt
echo "- Ensalada de espárragos trigueros — 9.00€" >> proyecto/menu.txt
echo "- Risotto de guisantes y menta — 13.00€" >> proyecto/menu.txt
```

A mitad del trabajo, te interrumpen con una urgencia...

---

## Tarea 4: Usa stash para atender la urgencia

```bash
git stash push -m "WIP: menú de primavera a medias"
git status
```

✅ Cambios guardados. Working Directory limpio.

Atiende la urgencia:

```bash
echo "AVISO: Cerrado por festivo el 1 de marzo" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega aviso de cierre por festivo"
```

Recupera tu trabajo:

```bash
git stash pop
git status
git diff proyecto/menu.txt
```

✅ Tus cambios del menú de primavera están de vuelta, combinados con
el aviso de cierre.

---

## Tarea 5: Completa y confirma el menú de primavera

```bash
echo "- Alcachofas a la plancha — 10.50€" >> proyecto/menu.txt
echo "- Fresas con nata — 5.50€" >> proyecto/menu.txt

git add proyecto/menu.txt
git diff --staged
git commit -m "Completa el menú de primavera 2026"
```

---

## Tarea 6: Actualiza el inventario de temporada

```bash
cat >> proyecto/inventario.txt << 'EOF'

## Productos de primavera
- Espárragos trigueros: 15 kg
- Guisantes frescos: 10 kg
- Menta fresca: 2 kg
- Alcachofas: 20 unidades
- Fresas: 8 kg
EOF

git add proyecto/inventario.txt
git commit -m "Agrega productos de primavera al inventario"
```

---

## Tarea 7: Etiqueta el lanzamiento

```bash
git tag -a v2.0.0 -m "Versión 2.0: Lanzamiento menú de primavera 2026"
git log --oneline --decorate -5
```

✅ Ves `v2.0.0` en el commit más reciente.

Si tienes remoto, publica todo:

```bash
git push 2>/dev/null
git push --tags 2>/dev/null
```

---

## Tarea 8: Comprueba que puedes volver a versiones anteriores

```bash
# Ve los tags disponibles
git tag -l

# Mira cómo era el menú en v1.2.0
git show v1.2.0:proyecto/menu.txt

# Compara el menú entre versiones
git diff v1.2.0 v2.0.0 -- proyecto/menu.txt
```

✅ Puedes ver exactamente qué cambió entre la versión de invierno y
la de primavera.

---

## Tarea 9: Limpieza

```bash
rm -f menu_primavera.draft precios_test.txt
rm -rf planning
git status
```

✅ Todo limpio. Los archivos temporales nunca entraron al repositorio.

---

## Autoevaluación

### ¿Los archivos temporales están correctamente ignorados?

```bash
echo "test" > prueba.draft
git status
rm prueba.draft
```

✅ No aparece en `git status`.

### ¿Los tags están en su lugar?

```bash
git tag -l
```

### ¿El stash está vacío?

```bash
git stash list
```

✅ No hay stashes pendientes.

---

## 🏆 ¡Felicidades!

Has completado el Módulo 7: Herramientas complementarias. Ahora sabes:

- ✅ Guardar trabajo temporal con `git stash`
- ✅ Marcar versiones con `git tag`
- ✅ Excluir archivos con `.gitignore`
- ✅ Combinar las tres herramientas en un flujo real

---

> **Siguiente módulo**: Continúa con el **Módulo 8: Buenas prácticas**
> → `../modulo_08_buenas_practicas/01_mensajes_commit.md`
