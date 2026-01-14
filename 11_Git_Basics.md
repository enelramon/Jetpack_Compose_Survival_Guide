# 11. Fundamentos de Git para Trabajo Colaborativo

Git es un sistema de control de versiones distribuido que permite a los desarrolladores trabajar de manera colaborativa en proyectos de software. Esta guía cubre los comandos esenciales que necesitas para trabajar en equipo.

## ¿Qué es Git?

Git es una herramienta que te permite:
- Rastrear cambios en tu código a lo largo del tiempo
- Trabajar en equipo sin sobrescribir el trabajo de otros
- Revertir cambios si algo sale mal
- Mantener múltiples versiones de tu proyecto

## Configuración Inicial

Antes de empezar a usar Git, configura tu identidad:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"
```

Verifica tu configuración:
```bash
git config --list
```

## Comandos Básicos Esenciales

### 1. Inicializar o Clonar un Repositorio

**Crear un nuevo repositorio:**
```bash
git init
```

**Clonar un repositorio existente:**
```bash
git clone https://github.com/usuario/repositorio.git
```

### 2. Ver el Estado del Repositorio

Verifica qué archivos han cambiado:
```bash
git status
```

### 3. Agregar Cambios al Staging Area

**Agregar un archivo específico:**
```bash
git add nombre_archivo.cs
```

**Agregar todos los archivos modificados:**
```bash
git add .
```

**Agregar archivos por extensión:**
```bash
git add *.cs
```

### 4. Guardar Cambios (Commit)

Crea un commit con los cambios en staging:
```bash
git commit -m "Descripción clara de los cambios"
```

**Buenas prácticas para mensajes de commit:**
- Usa verbos en imperativo: "Agrega", "Corrige", "Actualiza"
- Sé específico y conciso
- Ejemplo: "Agrega validación de usuario en login"

### 5. Ver el Historial de Commits

```bash
git log
```

Ver historial simplificado:
```bash
git log --oneline
```

Ver historial con gráfico de ramas:
```bash
git log --oneline --graph --all
```

## Trabajo con Repositorios Remotos

### 1. Conectar con un Repositorio Remoto

```bash
git remote add origin https://github.com/usuario/repositorio.git
```

Ver repositorios remotos configurados:
```bash
git remote -v
```

### 2. Subir Cambios al Remoto (Push)

Envía tus commits al repositorio remoto:
```bash
git push origin main
```

Para la primera vez (establecer upstream):
```bash
git push -u origin main
```

### 3. Traer Cambios del Remoto (Pull)

Descarga y fusiona cambios del repositorio remoto:
```bash
git pull origin main
```

### 4. Actualizar Referencias sin Fusionar (Fetch)

```bash
git fetch origin
```

## Trabajo con Ramas (Branches)

Las ramas permiten trabajar en nuevas características sin afectar el código principal.

### 1. Ver Ramas

```bash
git branch
```

Ver todas las ramas (incluidas remotas):
```bash
git branch -a
```

### 2. Crear una Nueva Rama

```bash
git branch nombre-de-la-rama
```

### 3. Cambiar de Rama

```bash
git checkout nombre-de-la-rama
```

**Crear y cambiar a una nueva rama en un solo comando:**
```bash
git checkout -b nombre-de-la-rama
```

### 4. Fusionar Ramas (Merge)

Primero cambia a la rama donde quieres fusionar:
```bash
git checkout main
```

Luego fusiona la otra rama:
```bash
git merge nombre-de-la-rama
```

### 5. Eliminar una Rama

```bash
git branch -d nombre-de-la-rama
```

Forzar eliminación (si tiene cambios sin fusionar):
```bash
git branch -D nombre-de-la-rama
```

## Resolver Conflictos

Los conflictos ocurren cuando dos personas modifican las mismas líneas de código.

### Proceso de Resolución:

1. Git marcará los archivos con conflictos:
```
<<<<<<< HEAD
Tu código
=======
Código del otro desarrollador
>>>>>>> nombre-de-la-rama
```

2. Edita el archivo manualmente para decidir qué código mantener

3. Elimina los marcadores de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`)

4. Agrega los archivos resueltos:
```bash
git add archivo-resuelto.cs
```

5. Completa el merge:
```bash
git commit -m "Resuelve conflicto en archivo-resuelto.cs"
```

## Pull Requests (Solicitudes de Incorporación de Cambios)

Un **Pull Request (PR)** es una solicitud para que tus cambios sean revisados y fusionados en la rama principal del proyecto. Es la forma profesional y colaborativa de integrar código en proyectos de equipo.

### ¿Qué es un Pull Request?

Un Pull Request no es un comando de Git, sino una característica de plataformas como GitHub, GitLab o Azure DevOps. Es básicamente decir: *"Aquí están mis cambios, por favor revísenlos antes de incorporarlos al proyecto"*.

### ¿Por qué usar Pull Requests?

1. **Revisión de código:** Otros pueden revisar tu código antes de fusionarlo
2. **Discusión:** Puedes comentar líneas específicas y sugerir mejoras
3. **Control de calidad:** Se pueden ejecutar pruebas automáticas antes de fusionar
4. **Documentación:** Queda un registro de qué cambios se hicieron y por qué
5. **Aprendizaje:** Los desarrolladores junior reciben retroalimentación de los seniors

### Flujo de Trabajo con Pull Requests

#### 1. Crea una rama para tu trabajo
```bash
git checkout -b feature/mi-nueva-funcionalidad
```

#### 2. Realiza tus cambios y haz commits
```bash
git add .
git commit -m "Agrega nueva funcionalidad X"
```

#### 3. Sube tu rama al repositorio remoto
```bash
git push origin feature/mi-nueva-funcionalidad
```

#### 4. Crea el Pull Request

Ve a la plataforma web (GitHub, GitLab, etc.) y:
- Verás un botón que dice "Compare & pull request" o "Create pull request"
- Completa la información:
  - **Título:** Descripción breve de los cambios
  - **Descripción:** Explica qué cambios hiciste y por qué
  - **Revisores:** Selecciona quién debe revisar tu código
  - **Etiquetas:** Agrega etiquetas como `bugfix`, `feature`, `documentation`

#### 5. Proceso de Revisión

Los revisores pueden:
- **Aprobar** el PR si todo está bien
- **Solicitar cambios** si encuentran problemas
- **Comentar** líneas específicas del código

#### 6. Realizar Cambios Solicitados

Si te piden cambios:
```bash
# Asegúrate de estar en tu rama
git checkout feature/mi-nueva-funcionalidad

# Realiza los cambios solicitados
# ... edita archivos ...

# Haz commit de los cambios
git add .
git commit -m "Corrige problemas señalados en revisión"

# Sube los nuevos commits
git push origin feature/mi-nueva-funcionalidad
```

El Pull Request se actualizará automáticamente con tus nuevos commits.

#### 7. Fusionar el Pull Request

Una vez aprobado, alguien con permisos (puede ser tú) hará clic en **"Merge pull request"** en la plataforma web.

Opciones de fusión:
- **Merge commit:** Mantiene todos los commits (recomendado para equipos)
- **Squash and merge:** Combina todos los commits en uno solo
- **Rebase and merge:** Aplica los commits sobre la rama principal

#### 8. Limpieza Post-Fusión

```bash
# Cambia a la rama main
git checkout main

# Actualiza tu rama main local
git pull origin main

# Elimina tu rama local (ya está fusionada)
git branch -d feature/mi-nueva-funcionalidad

# Opcionalmente, elimina la rama remota
git push origin --delete feature/mi-nueva-funcionalidad
```

### Buenas Prácticas para Pull Requests

1. **Mantén los PRs pequeños:** Más fáciles de revisar (idealmente < 400 líneas)
2. **Un PR = una funcionalidad:** No mezcles cambios no relacionados
3. **Escribe descripciones claras:** Explica qué, por qué y cómo
4. **Agrega capturas de pantalla:** Si hay cambios visuales
5. **Responde a comentarios:** Aunque no estés de acuerdo, explica tu razonamiento
6. **Actualiza tu rama:** Si main avanza, trae los cambios a tu rama
7. **Prueba tu código:** Antes de crear el PR, asegúrate de que funciona

### Ejemplo de Buena Descripción de PR

```markdown
## Descripción
Agrega validación de email en el formulario de registro.

## Cambios realizados
- Validación de formato de email usando regex
- Mensaje de error cuando el email es inválido
- Pruebas unitarias para la validación

## Problema que resuelve
Cierra #42 - Los usuarios podían registrarse con emails inválidos

## Capturas de pantalla
[Adjuntar imagen del formulario con validación]

## Checklist
- [x] El código compila sin errores
- [x] Agregué pruebas unitarias
- [x] Actualicé la documentación
- [x] Probé en mi ambiente local
```

### Comandos Útiles para Trabajar con PRs

Ver diferencias entre tu rama y main:
```bash
git diff main..feature/mi-rama
```

Actualizar tu rama con cambios de main:
```bash
git checkout feature/mi-rama
git pull origin main
```

o usando rebase:
```bash
git checkout feature/mi-rama
git rebase main
```

## Deshacer Cambios

### 1. Descartar Cambios en un Archivo (antes de add)

```bash
git checkout -- nombre_archivo.cs
```

o en versiones más nuevas:
```bash
git restore nombre_archivo.cs
```

### 2. Sacar Archivos del Staging Area

```bash
git reset HEAD nombre_archivo.cs
```

o en versiones más nuevas:
```bash
git restore --staged nombre_archivo.cs
```

### 3. Deshacer el Último Commit (manteniendo cambios)

```bash
git reset --soft HEAD~1
```

### 4. Deshacer el Último Commit (descartando cambios)

```bash
git reset --hard HEAD~1
```

⚠️ **CUIDADO:** `--hard` elimina los cambios permanentemente.

## Archivo .gitignore

Crea un archivo `.gitignore` para evitar subir archivos innecesarios:

```gitignore
# Binarios y archivos compilados
bin/
obj/
*.dll
*.exe

# Visual Studio
.vs/
*.user
*.suo

# Archivos de configuración local
appsettings.Development.json
*.local.json

# Dependencias
node_modules/
packages/

# Archivos del sistema
.DS_Store
Thumbs.db
```

## Flujo de Trabajo Colaborativo Típico

### Flujo Diario:

1. **Actualiza tu repositorio local:**
```bash
git pull origin main
```

2. **Crea una rama para tu tarea:**
```bash
git checkout -b feature/nueva-funcionalidad
```

3. **Trabaja en tu código y haz commits frecuentes:**
```bash
git add .
git commit -m "Agrega funcionalidad X"
```

4. **Sube tu rama al remoto:**
```bash
git push origin feature/nueva-funcionalidad
```

5. **Crea un Pull Request** en GitHub/GitLab/Azure DevOps para revisión

6. **Después de la aprobación, fusiona a main** (generalmente desde la interfaz web)

7. **Actualiza tu rama local main:**
```bash
git checkout main
git pull origin main
```

8. **Elimina la rama local ya fusionada:**
```bash
git branch -d feature/nueva-funcionalidad
```

## Mejores Prácticas

1. **Haz commits frecuentes** con mensajes descriptivos
2. **Sincroniza regularmente** con el repositorio remoto (`git pull`)
3. **Usa ramas** para cada nueva característica o corrección
4. **Revisa los cambios** antes de hacer commit (`git status`, `git diff`)
5. **No subas archivos sensibles** (contraseñas, claves API)
6. **Comunica con tu equipo** antes de fusionar cambios importantes
7. **Mantén los commits atómicos** (un cambio lógico por commit)

## Comandos de Referencia Rápida

| Comando | Descripción |
|---------|-------------|
| `git init` | Inicializar repositorio |
| `git clone <url>` | Clonar repositorio |
| `git status` | Ver estado del repositorio |
| `git add <archivo>` | Agregar archivo al staging |
| `git commit -m "mensaje"` | Crear commit |
| `git push origin <rama>` | Subir cambios al remoto |
| `git pull origin <rama>` | Traer cambios del remoto |
| `git branch` | Listar ramas |
| `git checkout <rama>` | Cambiar de rama |
| `git checkout -b <rama>` | Crear y cambiar a nueva rama |
| `git merge <rama>` | Fusionar rama |
| `git log` | Ver historial |

## Recursos Adicionales

- **Documentación oficial:** https://git-scm.com/doc
- **GitHub Learning Lab:** https://lab.github.com/
- **Interactive Git Branching:** https://learngitbranching.js.org/
- **Git Cheat Sheet:** https://education.github.com/git-cheat-sheet-education.pdf

---

**Próximo paso:** [3. Control de Flujo en C#](3_CSharp_Control_Flow.md)

**Anterior:** [1. Introducción a C#](1_CSharp_Intro.md)

VIELKAREYES@HOTMAIL.COM
,BELKISSP@YAHOO.COM
,JUANDUARTE03@HOTMAIL.COM
,MISCAPRICITOS@GMAIL.COM
,JESUSM-VILLAR@HOTMAIL.COM
,ORDOMSRL@HOTMAIL.COM
,MARIANOSILVERIO@HOTMAIL.ES
,MOISES2751978@GMAIL.COM
,KIKICRUZ10@HOTMAIL.COM
,STANLEYJRP@GMAIL.COM
,DISMAOFFICE@GMAIL.COM
,MARIA4444FLORES@GMAIL.COM
,ALLMYHOBBIESTOGETHER@GMAIL.COM
,INFO.RAFITH@GMAIL.COM
,NIEVES_MARTE@HOTMAIL.COM
,EDWARDSHOESRD@GMAIL.COM
,DEPARTAMENTOLEGALTVB@GMAIL.COM
,MAGGIOLO@DOMINICAO.COM.DO
,PUNTOCLAVEJIMENEZ@GMAIL.COM
,JOHNMAR.G@HOTMAIL.ES