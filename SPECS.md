# SPECS.md — Panel de Administración de AgentHub

## 1. Descripción del producto

**AgentHub** es una plataforma SaaS donde las empresas alquilan **agentes de IA**
preconfigurados y les añaden **skills** (habilidades como navegar la web, leer
documentos o gestionar calendarios). Este documento especifica el **panel de
administración interno**: la herramienta que usa el **administrador de la
plataforma** (no el cliente final) para vigilar ingresos, gestionar usuarios,
agentes, skills, contrataciones y errores.

Es un **prototipo de referencia**, completamente diseñado y con datos
hardcodeados. No consume ninguna API ni backend.

## 2. Stack técnico y restricciones

- HTML5 semántico en un único archivo `index.html`.
- **Tailwind CSS solo vía CDN** (`https://cdn.tailwindcss.com`). Sin archivos CSS
  propios y sin atributos `style` en línea.
- **JavaScript vanilla únicamente**: sin React/Vue, sin jQuery, sin herramientas
  de build.
- Modo oscuro con la estrategia de clase de Tailwind (`darkMode: 'class'`),
  alternando la clase `dark` sobre `<html>`.
- Etiquetas semánticas: `<aside>`, `<nav>`, `<header>`, `<main>`, `<section>`,
  `<table>` con `<thead>/<tbody>` y `<th scope>`.
- Contraste mínimo AA (4.5:1) en texto de cuerpo, en claro y en oscuro.
- Usable en viewport de escritorio y tablet.

## 3. Estructura global

- **Barra lateral persistente** (`<aside>`) con navegación a las 6 secciones e
  **indicador de sección activa**.
- **Barra superior** (`<header>`) con el título del panel y el **toggle de modo
  claro/oscuro**.
- **Área de contenido** (`<main>`) donde se muestra una sección a la vez; el
  resto quedan ocultas (`hidden`). El cambio de sección ocurre en la misma
  página, por lo que **el modo elegido se conserva al navegar**.

## 4. Especificaciones por sección

### 4.1 Dashboard
1. Cuatro **tarjetas de métrica** en cuadrícula responsive, cada una con icono,
   etiqueta y valor hardcodeado: *Ingresos totales (este mes)*, *Pérdida por
   descuentos y cupones*, *Agentes activos*, *Agentes fallando*. Los tres últimos
   colores de acento diferencian el tipo de métrica.
2. Debajo de las tarjetas, un **panel de ancho completo** que representa el
   *gráfico de actividad semanal* mediante barras hardcodeadas etiquetadas de
   lunes a domingo dentro de un contenedor con borde.
3. La métrica *Agentes fallando* usa acento de alerta (rojo) para que el problema
   destaque a simple vista.

### 4.2 Gestión de usuarios
1. **Tabla** con al menos 5 usuarios hardcodeados: nombre, email, plan y **badge
   de estado** (Activo / Suspendido / Pendiente) con color por estado.
2. Cada fila tiene un **dropdown de acciones** (botón kebab) con al menos "Ver
   detalle" y "Eliminar".
3. "Ver detalle" abre un **modal** con el registro completo del usuario (empresa,
   fecha de alta, agentes contratados, último acceso). Se cierra con el botón de
   cierre y haciendo clic en el backdrop.

### 4.3 Gestión de agentes
1. Listado de al menos 4 agentes mostrando **nombre, propietario, badge de estado
   (activo / inactivo / fallando)** y una **lista de skills colapsada**.
2. Un control expandible revela las skills del agente con **transición visible**;
   volver a hacer clic las colapsa. Colapsadas por defecto.
3. Cada agente tiene un **dropdown** con "Configurar" (abre un modal con el prompt
   de sistema del agente en un `<textarea>` editable) y "Eliminar".

### 4.4 Skills
1. **Catálogo** de al menos 4 skills: nombre, descripción breve e indicador de
   **cuántos agentes la tienen habilitada**.
2. Bloque explicativo dentro del panel sobre **qué es una "skill"** en AgentHub.
3. Cada skill tiene un **dropdown** con "Ver detalle" (modal con la descripción
   ampliada y el recuento de uso) y "Eliminar".

### 4.5 Contrataciones de agentes
1. **Tabla** con al menos 4 contratos: cliente, agente alquilado, skills
   contratadas, fechas de inicio/fin e importe total pagado.
2. Cada fila tiene un **dropdown** con "Ver detalle".
3. "Ver detalle" abre un **modal** con el desglose completo del contrato,
   incluyendo la **lista desglosada de skills y sus precios individuales** que
   suman el importe total.

### 4.6 Log de errores
1. Al menos 6 entradas hardcodeadas: **timestamp, nombre del agente, badge de tipo
   de error con código de color** y descripción breve.
2. Los tipos de error se categorizan visualmente por gravedad (Crítico / Timeout /
   Autenticación / Cuota / Advertencia / Info) con badges de color distinto.
3. Cada entrada tiene un **dropdown** con "Ver detalle" (modal con la traza
   completa del error) y "Marcar como resuelto" (marca visualmente la entrada).

## 5. Inventario de componentes reutilizables

- **Sidebar de navegación** con estado activo.
- **Tarjeta de métrica** (icono + etiqueta + valor + acento).
- **Tabla de datos** con cabecera semántica.
- **Dropdown de acciones** (botón kebab + menú flotante).
- **Modal** genérico (backdrop + panel + título + cuerpo + cierre).
- **Badge** de estado / tipo con código de color.
- **Lista de skills colapsable** con transición.
- **Toggle de modo oscuro**.

## 6. Criterios de aceptación

1. `SPECS.md` está commiteado **antes** que cualquier archivo HTML (verificable en
   el historial de Git).
2. La barra lateral da acceso a las **6 secciones** y marca la sección activa.
3. Toda la interfaz alterna entre **claro y oscuro** con el toggle, y el modo se
   mantiene al cambiar de sección.
4. Todas las filas de los listados tienen un **dropdown funcional** que se abre al
   hacer clic, se cierra al volver a hacer clic y se cierra al hacer clic fuera.
5. "Ver detalle" abre un **modal en al menos 4 secciones** distintas (usuarios,
   agentes vía "Configurar", skills, contrataciones, errores).
6. Todos los **modales se cierran** con el botón de cierre y haciendo clic en el
   backdrop.
7. Las listas de skills de los agentes están **colapsadas por defecto** y se
   expanden/colapsan con una **transición visible**.
8. Los **datos hardcodeados son consistentes** entre secciones: los nombres de
   agente coinciden en Gestión de agentes, Contrataciones y Log de errores.
9. El desglose del modal de un contrato **suma exactamente** el importe total de
   la fila.
10. El HTML usa **etiquetas semánticas** (`section`, `table`, `nav`, `header`,
    `main`) y las clases utilitarias de Tailwind de forma consistente, sin estilos
    en línea ni CSS externo propio.
11. El layout es usable en viewports de **escritorio y tablet**.
