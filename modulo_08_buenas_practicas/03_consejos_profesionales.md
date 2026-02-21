# Lección 03: Consejos profesionales

## 📖 Los 20 consejos que marcan la diferencia

### Commits

**1. Commits atómicos**: Cada commit debe hacer UNA cosa. Si tu mensaje
necesita "y" ("Agrega menú Y corrige precio Y actualiza inventario"),
son tres commits.

**2. Commitea pronto y seguido**: Es mejor tener 20 commits pequeños
que 1 commit gigante. Los commits pequeños son más fáciles de revisar,
revertir y entender.

**3. No commitees código roto**: Cada commit debería dejar el proyecto
en un estado funcional. Usa `git stash` si necesitas guardar trabajo
a medias.

**4. Nunca commitees secretos**: Contraseñas, claves API, tokens de
acceso... una vez que entran en el historial de Git, quedan ahí para
siempre (incluso si los borras en un commit posterior). Usa `.gitignore`
y variables de entorno.

```
  ⚠️ Si commiteaste un secreto por error:
  1. Cambia la contraseña/clave INMEDIATAMENTE.
  2. Usa herramientas como git-filter-repo para limpiarlo.
  3. Asume que el secreto ya fue comprometido.
```

### Ramas

**5. Rama por tarea**: Una rama = una feature/fix/tarea. No mezcles
cambios no relacionados en la misma rama.

**6. Nombres descriptivos**: `feature/menu-vegano` es mejor que `rama1`.
`fix/calculo-iva` es mejor que `bugfix`.

**7. Mantén tus ramas cortas**: Las ramas que viven semanas son difíciles
de mergear. Intenta que las ramas duren días, no semanas.

**8. Borra las ramas mergeadas**: Las ramas muertas ensucian el
repositorio. Después de mergear, elimina la rama local y remota.

### Sincronización

**9. Pull antes de push**: Siempre `git pull` antes de `git push`.
Te ahorra rechazos y conflictos mayores.

**10. Fetch frecuente**: `git fetch` es gratis y seguro. Ejecútalo
seguido para ver si hay cambios en el remoto.

**11. No hagas push --force a ramas compartidas**: `--force` reescribe
el historial remoto. Si alguien más tiene esos commits, crearás un
desastre. Solo úsalo en ramas personales y con `--force-with-lease`
(más seguro).

### Organización

**12. Un buen `.gitignore` desde el inicio**: Configúralo antes de
tu primer commit. Es más fácil ignorar desde el principio que limpiar
después.

**13. Usa tags para releases**: Marca las versiones publicadas con
tags anotados. Es tu referencia rápida a estados estables.

**14. README actualizado**: Un buen `README.md` en la raíz del proyecto
ahorra horas de explicaciones.

### Herramientas

**15. Aprende los alias de Git**: Configura atajos para los comandos
que más usas:

```bash
git config --global alias.st "status"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.cm "commit -m"
git config --global alias.last "log -1 HEAD"
```

**16. Usa `git diff --staged` antes de cada commit**: Verifica que vas
a commitear exactamente lo que quieres.

**17. Aprende `git reflog`**: Tu red de seguridad cuando algo sale mal.
Puede salvar tu trabajo.

### Filosofía

**18. Git es tu amigo, no tu enemigo**: Si algo sale mal, casi siempre
hay forma de recuperarlo. No entres en pánico.

**19. Practica en un repo de prueba**: Antes de hacer algo arriesgado
en un proyecto real, pruébalo en un repositorio temporal.

**20. Lee los mensajes de error**: Git suele decirte exactamente qué
salió mal y cómo solucionarlo. Lee la salida completa antes de buscar
en internet.

---

## 💻 Práctica final

### Ejercicio: Configura tu Git profesional

Aplica estas configuraciones en tu máquina:

```bash
# Alias esenciales
git config --global alias.st "status"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.unstage "restore --staged"
git config --global alias.undo "restore"

# Configuraciones recomendadas
git config --global init.defaultBranch main
git config --global color.ui auto
git config --global pull.rebase false
```

Pruébalos:

```bash
cd ~/practica_git
git st
git lg
git last
```

✅ Comandos cortos que ahorran tiempo.

---

## 🧠 Cheat Sheet definitivo

### Comandos del día a día (95% de tu uso)

```bash
git status                    # ¿Dónde estoy?
git add archivo               # Preparar cambio
git commit -m "mensaje"       # Guardar cambio
git log --oneline -10         # Ver historial
git diff                      # Ver cambios
git switch -c rama            # Crear rama
git switch main               # Volver a main
git merge rama                # Unir rama
git push                      # Enviar al remoto
git pull                      # Traer del remoto
```

### Deshacer cosas

```bash
git restore archivo           # Descartar cambios del disco
git restore --staged archivo  # Quitar del staging
git revert HEAD               # Deshacer último commit (seguro)
git reset --soft HEAD~1       # Rehacer último commit
git reset --hard HEAD~1       # Borrar último commit (⚠️)
```

### Herramientas extra

```bash
git stash                     # Guardar trabajo temporal
git stash pop                 # Recuperar trabajo
git tag -a v1.0 -m "msg"     # Marcar versión
git remote -v                 # Ver remotos
git branch -a                 # Ver todas las ramas
git reflog                    # Historial de movimientos
```

---

## 🏆 ¡Felicidades! Has completado el curso completo de Git

```
  Módulo 1: Conceptos base         ✅ Entiendes qué es Git y por qué
  Módulo 2: Configuración          ✅ Sabes configurar e inicializar
  Módulo 3: Flujo básico           ✅ Dominas status/add/commit/log/diff
  Módulo 4: Ramas                  ✅ Creas, cambias, mergeas y resuelves conflictos
  Módulo 5: Remotos                ✅ Colaboras con push/pull/fetch
  Módulo 6: Deshacer               ✅ Corriges cualquier error
  Módulo 7: Herramientas           ✅ Usas stash, tags y gitignore
  Módulo 8: Buenas prácticas       ✅ Trabajas como un profesional
```

### ¿Qué sigue?

- **Practica todos los días**: Usa Git en todos tus proyectos.
- **Explora temas avanzados**: `git rebase`, `git cherry-pick`,
  `git bisect`, hooks, submódulos.
- **Contribuye a código abierto**: Es la mejor forma de practicar
  Git en equipo.

---

> "Ahora tienes el conocimiento. La maestría viene con la práctica."
