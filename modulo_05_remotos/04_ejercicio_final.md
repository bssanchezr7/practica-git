# Lección 04: Ejercicio Final Integrador — Remotos

## Objetivo

Simular un flujo de colaboración completo: trabajar con remotos, pushear
cambios, resolver divergencias y mantener sincronizado el repositorio.

## ⚠️ Reglas del ejercicio

1. Usa `git status` y `git log --oneline --graph --all` frecuentemente.
2. Siempre verifica con `git remote -v` que los remotos estén bien.
3. Haz commits con mensajes descriptivos.

---

## Preparación

Asegúrate de tener el remoto configurado:

```bash
cd ~/practica_git
git remote -v
```

Si no tienes un remoto, crea uno simulado:

```bash
git init --bare /tmp/remoto_ejercicio.git
git remote add origin /tmp/remoto_ejercicio.git 2>/dev/null
git push -u origin main
```

---

## Tarea 1: Flujo básico — push y pull

Haz un cambio, commitea y pushea:

```bash
echo "" >> proyecto/menu.txt
echo "## Menú infantil" >> proyecto/menu.txt
echo "- Nuggets con patatas — 6.50€" >> proyecto/menu.txt
echo "- Mini pizza — 5.00€" >> proyecto/menu.txt
echo "- Macarrones con queso — 5.50€" >> proyecto/menu.txt

git add proyecto/menu.txt
git commit -m "Agrega menú infantil"
git push
```

Verifica que el remoto tiene los cambios clonando en otra carpeta:

```bash
git clone "$(git remote get-url origin)" /tmp/verificacion_remoto
cat /tmp/verificacion_remoto/proyecto/menu.txt
rm -rf /tmp/verificacion_remoto
```

✅ El menú infantil aparece en la copia clonada.

---

## Tarea 2: Simula colaboración

Crea un "compañero de trabajo" y simula que ambos trabajan en paralelo:

```bash
# Tu compañero clona el repo
git clone "$(git remote get-url origin)" /tmp/compañero
cd /tmp/compañero

# Tu compañero hace cambios
echo "" >> proyecto/inventario.txt
echo "## Material de cocina" >> proyecto/inventario.txt
echo "- Sartenes antiadherentes: 6 unidades" >> proyecto/inventario.txt
echo "- Ollas grandes: 4 unidades" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega material de cocina al inventario"
git push origin main
cd ~/practica_git
```

---

## Tarea 3: Trae los cambios del compañero

Usa el flujo seguro (fetch → revisar → merge):

```bash
# 1. Descarga sin integrar
git fetch origin

# 2. Revisa qué hay de nuevo
git log main..origin/main --oneline

# 3. Mira el detalle
git diff main origin/main

# 4. Si estás conforme, integra
git merge origin/main
```

✅ Tu repo local ahora tiene el material de cocina.

---

## Tarea 4: Trabajad en paralelo (divergencia)

Ahora ambos trabajan al mismo tiempo sin sincronizarse:

```bash
# Tu compañero hace un cambio
cd /tmp/compañero
git pull
echo "- Tablas de cortar: 8 unidades" >> proyecto/inventario.txt
git add proyecto/inventario.txt
git commit -m "Agrega tablas de cortar"
git push origin main

# Tú haces un cambio sin pullear primero
cd ~/practica_git
echo "## Eventos especiales" >> proyecto/menu.txt
echo "Reservas para grupos: consultar disponibilidad" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Agrega sección de eventos especiales"
```

Intenta pushear:

```bash
git push
```

✅ **Resultado esperado**: RECHAZADO. El remoto tiene cambios que tú no.

Solución:

```bash
git pull
git push
```

Verifica el historial:

```bash
git log --oneline --graph -6
```

✅ Ves el merge que combina tu trabajo y el de tu compañero.

---

## Tarea 5: Trabaja con ramas remotas

```bash
# Crea una rama, trabaja y pushéala
git switch -c feature/carta-vinos
echo "" >> proyecto/menu.txt
echo "## Carta de vinos" >> proyecto/menu.txt
echo "- Rioja Reserva 2020 — 18.00€" >> proyecto/menu.txt
echo "- Ribera del Duero Crianza — 15.00€" >> proyecto/menu.txt
echo "- Albariño D.O. Rías Baixas — 12.00€" >> proyecto/menu.txt
git add proyecto/menu.txt
git commit -m "Crea carta de vinos"
git push -u origin feature/carta-vinos
```

Simula que tu compañero revisa y aprueba:

```bash
cd /tmp/compañero
git fetch origin
git branch -a
```

✅ Tu compañero ve la rama `remotes/origin/feature/carta-vinos`.

Vuelve, mergea y limpia:

```bash
cd ~/practica_git
git switch main
git merge feature/carta-vinos
git push
git branch -d feature/carta-vinos
git push origin --delete feature/carta-vinos
```

---

## Tarea 6: Limpieza final

```bash
rm -rf /tmp/compañero
git log --oneline --graph -10
git status
```

---

## Autoevaluación

### ¿Tu repo y el remoto están sincronizados?

```bash
git fetch origin
git status
```

✅ Debería decir "Your branch is up to date with 'origin/main'".

### ¿Puedes ver todo el historial de colaboración?

```bash
git log --oneline --graph --all
```

### ¿Las ramas están limpias?

```bash
git branch -a
```

✅ Solo `main` local y `remotes/origin/main`.

---

## 🏆 ¡Felicidades!

Has completado el Módulo 5: Trabajo con remotos. Ahora sabes:

- ✅ Configurar y gestionar remotos (`git remote`)
- ✅ Enviar tus cambios al servidor (`git push`)
- ✅ Traer cambios de otros (`git fetch`, `git pull`)
- ✅ Resolver rechazos de push (pull antes de push)
- ✅ Trabajar con ramas remotas
- ✅ Simular flujos de colaboración

---

> **Siguiente módulo**: Continúa con el **Módulo 6: Deshacer cosas**
> → `../modulo_06_deshacer/01_git_restore.md`
