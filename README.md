# Práctica 1: Sistema de Control de Versiones con Git y Git Flow - Dani Satorre

## 1. Introducción Teórica

### ¿Qué es Git?
**Git** es un sistema de control de versiones distribuido diseñado para rastrear cambios en el código fuente durante el desarrollo de software. Permite a múltiples desarrolladores trabajar simultáneamente en el mismo proyecto sin sobrescribir el trabajo de los demás, manteniendo un historial completo de modificaciones, facilitando la reversión de errores y permitiendo la creación de ramas (branches) para trabajar de forma aislada.

### ¿Qué es la metodología Git Flow?
**Git Flow** es un modelo de ramificación (*branching model*) estandarizado para Git que define una estructura estricta de ramas diseñada alrededor de las entregas de proyectos. Separa el trabajo en distintas ramas según el propósito:

* **`main` (o `master`):** Contiene el código en producción, siempre estable y listo para su despliegue.
* **`develop`:** Rama principal de desarrollo donde se integran todas las nuevas características antes de una release.
* **`feature/*`:** Ramas temporales creadas a partir de `develop` para desarrollar una funcionalidad o sección específica.
* **`release/*`:** Ramas temporales para preparar una nueva versión de producción (pruebas finales, corrección de pequeños fallos, documentación).
* **`hotfix/*`:** Ramas de emergencia creadas directamente desde `main` para solucionar un fallo crítico en producción.

### ¿Por qué la utilizamos en nuestro desarrollo?
Utilizar Git Flow en un entorno colaborativo ofrece múltiples ventajas:
1. **Organización y Paralelismo:** Varios desarrolladores pueden trabajar en distintas funcionalidades al mismo tiempo sin interferir entre sí.
2. **Estabilidad:** La rama `main` se mantiene limpia y libre de fallos, asegurando un código estable para el cliente.
3. **Trazabilidad:** Facilita el seguimiento de versiones mediante *tags* de versión (como `v1.0`).
4. **Resolución Eficiente de Errores:** Permite corregir fallos urgentes en producción mediante *hotfixes* sin interrumpir el desarrollo que se está llevando a cabo en `develop`.

---

## 2. Explicación Detallada del Proceso de Desarrollo

A continuación se detalla la simulación del trabajo en equipo realizado por los 3 miembros del grupo.

### Paso 1: Inicialización y Estructura Inicial ([Usuario 1])
* **Descripción:** El Usuario 1, como desarrollador con más experiencia, crea el repositorio inicial e implementa la estructura del proyecto utilizando el boilerplate proporcionado. Esta estructura incluye:
  * Cabecera de la página con el nombre del curso e imagen de fondo.
  * Barra de navegación con el enlace inicial a *Home*.
  * Sección central en el *Home* con una descripción breve del curso de JavaScript.
  * Footer global de la página.
* **Creación de la rama `develop`:** Tras subir la versión base a la rama `main`, crea e inicializa la rama principal de desarrollo (`develop`).

```bash
git clone https://github.com/danisatorre/Sistema_control_versiones
git add .
git commit -m "[Usuario 1] Estructura inicial"
git push origin main

# Creación de la rama de desarrollo
git checkout -b develop
git push origin develop
```

---

### Paso 2: Desarrollo de Secciones de HTML ([Usuario 2])
El Usuario 2 se encarga de implementar las funciones de manipulación del DOM referentes a contenido y atributos HTML.

1. **Sección "Modificar contenido HTML" (`feature/contingutHTML`):**
   * Crea la rama de feature desde `develop`.
   * Implementa la sección agregando ejemplos interactivos de modificación de contenido mediante JavaScript y su respectiva explicación.
   * Fusiona la feature de vuelta a `develop`.

   ```bash
   git checkout develop
   git checkout -b feature/modificarHTML
   # ... cambios en el código ...
   git add .
   git commit -m "[USUARIO 2] Modificar el HTML"
   git checkout develop
   git merge feature/modificarHTML
   git branch -d feature/modificarHTML
   ```

2. **Sección "Modificar atributos HTML" (`feature/atributsHTML`):**
   * Crea la rama de feature desde `develop`.
   * Agrega la sección para mostrar la manipulación de atributos de elementos HTML mediante código JavaScript.
   * Fusiona la feature en `develop` y actualiza el remoto.

   ```bash
   git checkout develop
   git checkout -b feature/atributosHTML
   # ... cambios en el código ...
   git add .
   git commit -m "[USUARIO 2] Modificar atributos HTML"
   git checkout develop
   git merge feature/atributosHTML
   git branch -d feature/atributosHTML
   git push origin develop
   ```

---

### Paso 3: Desarrollo de Secciones CSS y Release v1.0 ([Usuario 3])
El Usuario 3 (nuevo integrante del equipo) se encarga de la sección de estilos CSS y del posterior empaquetado del primer lanzamiento (`v1.0`).

1. **Sección "Modificar estilos CSS" (`feature/estilsCSS`):**
   * Crea la rama de feature desde `develop`.
   * Desarrolla la sección de estilos dinámicos en CSS con código comentado explícitamente.
   * Fusiona los cambios en `develop`.

   ```bash
   git checkout develop
   git checkout -b feature/estiloCSS
   # ... cambios en el código ...
   git add .
   git commit -m "[USAURIO 3] Implementar estilo css"
   git checkout develop
   git merge feature/estiloCSS
   git branch -d feature/estiloCSS
   ```

2. **Despliegue de Release `v1.0`:**
   * Crea la rama `release/v1.0` desde `develop`.
   * Realiza las comprobaciones finales y fusiona la versión tanto a `main` como a `develop`.
   * Etiqueta la versión estable en `main` con el Tag `v1.0`.

   ```bash
   git checkout develop
   git checkout -b release/v1.0
   
   # Fusionar en main y etiquetar
   git checkout main
   git merge release/v1.0
   git tag -a v1.0 -m "[USUARIO 3] Ultimos cambios en release"
   
   # Fusionar en develop
   git checkout develop
   git merge release/v1.0
   git branch -d release/v1.0
   
   # Subir cambios al servidor remoto
   git push origin main develop --tags
   ```

---

### Paso 4: Fase de Pruebas y Aplicación de Hotfix ([Usuario 1])
El Usuario 1, tras revisar la versión desplegada `v1.0`, identifica áreas de mejora en la sección de contenido HTML realizada por el Usuario 2.

* **Aplicación de Hotfix (`hotfix/milloresV_1_0`):**
  * Crea una rama de hotfix desde `main`.
  * Efectúa las mejoras de optimización en el código.
  * Fusiona los cambios en `main` y en `develop` para sincronizar ambas ramas estables.

```bash
git checkout main
git checkout -b hotfix/mejoras_v1_0
# ... aplicación de mejoras en el código ...
git add .
git commit -m "[USUARIO 1] Hotfix: mejora aplicada en footer"

# Integración del Hotfix
git checkout main
git merge hotfix/mejoras_v1_0

git checkout develop
git merge hotfix/mejoras_v1_0

git branch -d hotfix/mejoras_v1_0
git push origin main develop
```

---

## 3. Gestión del Proyecto en GitHub

1. **Tablero Kanban (GitHub Projects):** Se ha habilitado un tablero donde se gestionan las tarjetas correspondientes a:
   * `Feature: contenidoHTML`
   * `Feature: atributosHTML`
   * `Feature: estiloCSS`
   * `Hotfix: mejoras v1_0`
2. **Colaboradores:** Se ha otorgado acceso como colaborador del proyecto al usuario `antoni-gimenez`.