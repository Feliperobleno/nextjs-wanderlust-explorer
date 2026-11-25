# SPECIFICATION — Wanderlust Explorer

## 1. Descripción del proyecto

**Nombre:** Wanderlust Explorer  
**Versión:** MVP 1.1  
**Tipo:** Aplicación web multipágina para descubrimiento y gestión de experiencias turísticas  

Wanderlust Explorer es una plataforma digital que permite a los usuarios explorar, buscar, filtrar y guardar experiencias turísticas únicas alrededor del mundo. La aplicación proporciona una interfaz intuitiva, visualmente atractiva y adaptada a dispositivos móviles y de escritorio.

## 2. Objetivo funcional

Desarrollar un MVP funcional que permita:
- Explorar un catálogo de 100 experiencias turísticas
- Buscar experiencias por título (búsqueda case-insensitive con expresiones regulares)
- Filtrar experiencias por categoría (Adventure, Culture, Food, Wellness, Nature)
- Filtrar experiencias por destino (ciudad/país)
- Combinar búsqueda y filtros simultáneamente
- Guardar/eliminar experiencias de una lista de favoritos
- Compartir búsquedas mediante URLs con query parameters
- Navegar sin recargas completas de página

## 3. Rutas y navegación

### 3.1. Ruta: `/` (Página de inicio)

**Propósito:**  
Presentar la plataforma Wanderlust Explorer con una interfaz de bienvenida.

**Requisitos funcionales:**
- Sección hero de presentación de la plataforma
- Botón principal que permite acceder al explorador de experiencias (/experiences)
- Navbar presente con indicación visual de la sección activa (requisito explícito)

**Componentes esperados:**
- Navbar
- Hero section
- CTA button

---

### 3.2. Ruta: `/experiences` (Explorador de experiencias)

**Propósito:**  
Visualizar y explorar el catálogo de 100 experiencias disponibles con capacidades de búsqueda y filtrado.

**Requisitos funcionales:**

| Requisito | Descripción | Explícito | Estado |
|-----------|-------------|----------|--------|
| Visualización de experiencias | Mostrar las 100 experiencias en una cuadrícula de tarjetas | ✓ | Definido |
| Búsqueda por título | Filtrar por término usando regex case-insensitive `/term/i` | ✓ | Definido |
| Filtro por categoría | Permitir seleccionar cualquiera de las 5 categorías | ✓ | Definido |
| Filtro por destino | Permitir filtrar por ciudad o país de destino | ✓ | Definido |
| Combinación de filtros | Búsqueda y filtros pueden aplicarse simultáneamente | ✓ | Definido |
| Acceso a detalle | Cada tarjeta permite acceder a `/experiences/[id]` | ✓ | Definido |
| Gestión de favoritos | Icono de corazón para añadir/eliminar de favoritos | ✓ | Definido |
| Sin resultados | Mostrar mensaje cuando no hay coincidencias | ✓ | Definido |
| Sin recarga completa | Los resultados se actualizan sin recargar la página | ✓ | Definido |
| Navbar activa | Navbar presente indicando sección activa | ✓ | Definido |

**Parámetros de búsqueda esperados:**
- `search` (string): término de búsqueda en título
- `category` (string): categoría seleccionada
- `destination` (string): destino seleccionado

**Componentes esperados:**
- Navbar
- SearchBar
- FilterBar
- ExperienceCard (reutilizable)
- Grid/Container para visualización

---

### 3.3. Ruta: `/experiences/[id]` (Detalle de experiencia)

**Propósito:**  
Mostrar información completa de una experiencia específica.

**Requisitos funcionales:**

| Requisito | Descripción | Explícito | Estado |
|-----------|-------------|----------|--------|
| Obtención del ID | Leer identificador desde la URL | ✓ | Definido |
| Recuperación de datos | Obtener información desde dataset local `src/data/experiences.ts` | ✓ | Definido |
| Visualización completa | Mostrar todos los atributos de la experiencia | ✓ | Definido |
| Gestión de favoritos | Permitir añadir/eliminar de favoritos desde el detalle | ✓ | Definido |
| Navegación | Navbar presente e indicador de sección activa | ✓ | Definido |
| Enlace para regresar | Permite regresar a /experiences | ✓ | Definido |
| Manejo de error 404 | Si el ID no existe, mostrar página de error 404 | ✓ | Definido |

**Comportamiento esperado:**
- El estado de favoritos debe estar sincronizado con la lista global de favoritos
- La página incluye un enlace/botón claramente visible para regresar al explorador

---

### 3.4. Ruta: `/favorites` (Lista de favoritos)

**Propósito:**  
Mostrar exclusivamente las experiencias marcadas como favoritas por el usuario.

**Requisitos funcionales:**

| Requisito | Descripción | Explícito | Estado |
|-----------|-------------|----------|--------|
| Visualización de favoritos | Mostrar solo experiencias marcadas como favoritas | ✓ | Definido |
| Formato de visualización | Mismo formato de tarjetas que /experiences | ✓ | Definido |
| Gestión en esta página | Permitir eliminar de favoritos desde esta página | ✓ | Definido |
| Sin resultados | Mostrar mensaje "No se encontraron resultados" si no hay favoritos | ✓ | Definido |
| Navbar activa | Navbar presente indicando sección activa | ✓ | Definido |

**Comportamiento esperado:**
- El estado de favoritos debe ser compartido con el resto de la aplicación
- Se mantiene durante la navegación entre páginas
- Si no hay favoritos, muestra el mensaje exacto: "No se encontraron resultados"
- No persiste entre recargas completas de página (fuera del alcance)

---

### 3.5. Ruta: `/profile` (Perfil de usuario)

**Propósito:**  
Presentar un perfil de usuario simulado con información agregada.

**Requisitos funcionales:**

| Requisito | Descripción | Explícito | Estado |
|-----------|-------------|----------|--------|
| Perfil simulado | Presentar perfil de usuario de demostración | ✓ | Definido |
| Contador de favoritos | Mostrar número de experiencias guardadas | ✓ | Definido |
| Contador actualizado | El contador se sincroniza con cambios en favoritos | ✓ | Definido |
| Navbar activa | Navbar presente indicando sección activa | ✓ | Definido |

---

## 4. Modelo de datos: Experience

### 4.1. Ubicación del dataset

- **Archivo:** `src/data/experiences.ts`
- **Formato:** Array de objetos que cumplen la interfaz `Experience`
- **Cantidad:** 100 experiencias

### 4.2. Interfaz TypeScript Experience

**Atributos obligatorios:**

| Campo | Tipo | Descripción | Obligatorio |
|-------|------|-------------|-------------|
| `id` | `string` | Identificador único de la experiencia | ✓ |
| `title` | `string` | Título de la experiencia | ✓ |
| `description` | `string` | Descripción completa de la experiencia | ✓ |
| `category` | `'Adventure' \| 'Culture' \| 'Food' \| 'Wellness' \| 'Nature'` | Categoría de la experiencia | ✓ |
| `destination` | `string` | Ciudad y país del destino | ✓ |
| `price` | `number` | Precio de la experiencia (cantidad numérica) | ✓ |
| `rating` | `number` | Valoración de la experiencia (escala numérica) | ✓ |
| `imageUrl` | `string` | URL de la imagen de la experiencia | ✓ |

**Restricciones del modelo:**

- Las categorías están limitadas a exactamente 5 valores: `Adventure`, `Culture`, `Food`, `Wellness`, `Nature`
- Las imágenes pueden utilizar recursos placeholder (sin restricción sobre proveedor específico)
- El ID debe ser único para cada experiencia (tipo `string`)
- Se utilizan exclusivamente estos 8 atributos, sin extensiones adicionales

---

## 5. Reglas de filtrado y búsqueda

### 5.1. Búsqueda por título

**Requisito explícito:**
- Utilizar expresiones regulares case-insensitive: `/term/i`
- Buscar coincidencias en el campo `title` de las experiencias
- Escapar caracteres especiales del término de búsqueda antes de construir la expresión regular

**Comportamiento esperado:**
- La búsqueda se aplica automáticamente mientras el usuario escribe (sin necesidad de botón de búsqueda)
- Si se borra el término, desaparece el filtro de búsqueda
- Puede combinarse con filtros de categoría y destino

**Implementación requerida:**
```typescript
const escapedTerm = searchTerm.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
const regex = new RegExp(escapedTerm, 'i');
const matches = experience.title.match(regex) !== null;
```

---

### 5.2. Filtro por categoría

**Requisito explícito:**
- El usuario puede seleccionar cualquiera de las 5 categorías disponibles

**Comportamiento esperado:**
- Selección única (single-select): el usuario puede elegir una sola categoría
- Si se selecciona una categoría, solo muestra experiencias con esa categoría
- Si se deselecciona (limpia la selección), desaparece el filtro de categoría
- Puede combinarse con búsqueda y filtro de destino
- Se aplica automáticamente sin necesidad de confirmar la selección

---

### 5.3. Filtro por destino

**Requisito explícito:**
- El usuario puede filtrar las experiencias por ciudad o país de destino

**Comportamiento esperado:**
- Utiliza coincidencias parciales (búsqueda que incluya substring)
- Case-insensitive: no distingue entre mayúsculas y minúsculas
- Si se ingresa un término, solo muestra experiencias cuyo `destination` contiene ese término
- Si se limpia el filtro, desaparece
- Puede combinarse con búsqueda y filtro de categoría
- Se aplica automáticamente sin necesidad de confirmar

**Implementación requerida:**
```typescript
const destinationFilter = destination.toLowerCase();
const matches = experience.destination.toLowerCase().includes(destinationFilter);
```

---

### 5.4. Combinación de filtros

**Requisito explícito:**
- La búsqueda y los filtros funcionan independientemente y pueden combinarse
- Los resultados se actualizan sin recargar completamente la página

**Lógica esperada:**
```
resultados = experiencias
  .filter(exp => buscar_en_titulo(exp, término_búsqueda))
  .filter(exp => filtrar_por_categoría(exp, categoría))
  .filter(exp => filtrar_por_destino(exp, destino))
```

**Comportamiento esperado:**
- Todos los filtros se aplican con lógica AND (intersección)
- Si algún filtro está vacío, se omite de la evaluación

---

### 5.5. Sin resultados

**Requisito explícito:**
- Mostrar mensaje exactamente: "No se encontraron resultados" cuando no existan coincidencias

**Comportamiento esperado:**
- El mensaje aparece cuando la combinación de filtros no produce resultados
- Se mantiene visible hasta que se modifiquen los filtros
- Tiene prioridad sobre la visualización de tarjetas vacías

---

## 6. Sincronización con la URL

### 6.1. Query parameters aprobados

**Parámetros que deben sincronizarse:**

| Parámetro | Tipo | Descripción | Obligatorio |
|-----------|------|-------------|-------------|
| `search` | string | Término de búsqueda en título | No |
| `category` | string | Categoría seleccionada | No |
| `destination` | string | Destino seleccionado | No |

**Ejemplo de URL:**
```
/experiences?search=beach&category=Adventure&destination=Hawaii
```

**Comportamiento esperado:**
- La URL es la fuente de verdad para los filtros activos
- Los parámetros se actualizan automáticamente cuando el usuario modifica búsqueda o filtros
- Los parámetros se eliminan de la URL cuando se limpian los filtros correspondientes
- Todos los parámetros son opcionales

---

### 6.2. Hooks de Next.js a utilizar

**useSearchParams:**
- Leer los parámetros actuales de la URL
- Acceder a valores como: `searchParams.get('search')`

**usePathname:**
- Obtener la ruta actual para identificar la sección activa
- Utilizar en Navbar para resaltar el enlace correspondiente

---

### 6.3. Comportamiento de lectura y escritura

**Lectura:**
- Al cargar la página `/experiences`, leer los parámetros de búsqueda
- Prerrellenar los campos de entrada (SearchBar, FilterBar) con los valores de la URL
- Aplicar los filtros automáticamente

**Escritura:**
- Cuando el usuario modifica búsqueda o filtros, actualizar automáticamente la URL
- Los parámetros de URL reflejan siempre el estado actual de los filtros
- Eliminar parámetros cuando el usuario limpia los filtros
- El usuario puede copiar la URL actual para compartir una búsqueda específica

---

### 6.4. Acceso directo mediante URL

**Requisito explícito:**
- Cuando se accede directamente a una URL con parámetros de búsqueda:
  - Los campos deben aparecer prerrellenados
  - Los resultados deben mostrarse con los filtros aplicados

**Caso de uso:**
1. Usuario A realiza una búsqueda: `/experiences?search=beach&category=Adventure`
2. Usuario A copia la URL y la comparte
3. Usuario B accede directamente a esa URL
4. Los campos aparecen prerrellenados y los resultados filtrados se muestran inmediatamente

---

## 7. Gestión de favoritos

### 7.1. Requisitos explícitos

- Cada tarjeta de experiencia incluye un icono de corazón
- El icono permite añadir/eliminar la experiencia de favoritos
- El icono refleja visualmente el estado actual (relleno vs. vacío)
- Los identificadores de favoritos (IDs de tipo `string`) se almacenan mediante `useState`
- El estado se gestiona en un nivel compartido de la aplicación
- El estado se transmite mediante props
- Se mantiene disponible durante la navegación entre páginas
- No requiere persistencia entre recargas completas de página

### 7.2. Estructura de estado

**Ubicación:** Componente root o layout principal  
**Tipo:** `useState<string[]>([])`  
**Contenido:** Array de IDs (strings) de experiencias marcadas como favoritos

**Nota:** La arquitectura concreta del estado compartido (props, custom hook, Context API) se definirá en fases posteriores del proyecto.

---

### 7.3. Acciones esperadas

| Acción | Descripción | Ubicación |
|--------|-------------|-----------|
| Marcar como favorito | Añadir ID a la lista de favoritos | ExperienceCard, Detalle |
| Desmarcar favorito | Remover ID de la lista de favoritos | ExperienceCard, Detalle, Favorites |
| Ver estado | Consultar si una experiencia está en favoritos | ExperienceCard, Detalle |

---

## 8. Componentes reutilizables

### 8.1. Componentes requeridos

| Componente | Ubicación esperada | Responsabilidades |
|------------|-------------------|------------------|
| **Navbar** | Presente en todas las páginas | Navegación entre rutas, indicador de sección activa via `usePathname` |
| **ExperienceCard** | /experiences, /favorites, /experiences/[id] | Mostrar información resumida de experiencia, botón favorito, link a detalle |
| **SearchBar** | /experiences | Input de búsqueda, sincronización con URL |
| **FilterBar** | /experiences | Selectores de categoría y destino, sincronización con URL |

### 8.2. Recomendación de estructura

```
src/
├── components/
│   ├── Navbar.tsx
│   ├── ExperienceCard.tsx
│   ├── SearchBar.tsx
│   ├── FilterBar.tsx
│   └── [otros componentes]
├── data/
│   └── experiences.ts
├── types/
│   └── experience.ts
├── hooks/
│   └── [custom hooks como useExperiences, useFilters]
├── app/
│   ├── layout.tsx
│   ├── page.tsx           (/)
│   ├── experiences/
│   │   ├── page.tsx       (/experiences)
│   │   └── [id]/
│   │       └── page.tsx   (/experiences/[id])
│   ├── favorites/
│   │   └── page.tsx       (/favorites)
│   └── profile/
│       └── page.tsx       (/profile)
```

---

## 9. Hooks de React requeridos

### 9.1. useState

**Ubicación mínima requerida:**
- Gestión de favoritos en un componente compartido (layout o context-like)

**Requisito explícito:**
- Gestionar lista de favoritos mediante `useState` con tipo `string[]`
- Transmitir mediante props a componentes que lo necesiten

---

### 9.2. useEffect

**Requisitos explícitos:**
- Utilizar en al menos un componente
- Gestión correcta de dependencias
- Sin bucles infinitos

**Casos de uso potenciales:**
- Sincronización de SearchParams con estado local
- Inicialización de filtros desde URL
- Actualización de favoritos cuando cambia el ID de la experiencia

---

### 9.3. Custom Hook de filtrado (obligatorio)

**Requisito explícito:**
- Encapsular la lógica de búsqueda y filtrado en un custom hook

**Nombre sugerido:** `useFilters`

**Responsabilidades del custom hook:**
- Leer parámetros de URL usando `useSearchParams`
- Aplicar lógica de búsqueda (regex case-insensitive con escape de caracteres)
- Aplicar filtro de categoría (single-select)
- Aplicar filtro de destino (coincidencia parcial, case-insensitive)
- Combinar todos los filtros con lógica AND
- Retornar experiencias filtradas
- Actualizar URL cuando cambian los filtros

**Comportamiento esperado:**
- El hook encapsula toda la lógica de filtrado y sincronización con URL
- Facilita la reutilización de la lógica en diferentes componentes
- Abstrae la complejidad de `useSearchParams` y la lógica de filtrado

---

### 9.4. Nota sobre favoritos

**`useFavorites` (si se implementa):**
- Es un custom hook adicional, complementario al custom hook de filtrado
- No sustituye el custom hook obligatorio de filtrado
- Encapsula la lógica de gestión de favoritos (añadir, eliminar, consultar estado)
- Ubicación: Puede ser implementado pero no es obligatorio en esta fase

---

## 10. Restricciones técnicas

### 10.1. Stack tecnológico confirmado

- **React:** Uso obligatorio
- **Next.js:** App Router (no Pages Router)
- **TypeScript:** Tipado consistente
- **Tailwind CSS:** Para estilos
- **ESLint:** Configurado en el proyecto

### 10.2. Prohibiciones explícitas

| Restricción | Razón |
|-------------|-------|
| No utilizar Redux | Gestión de estado solo con `useState` |
| No utilizar Zustand | Gestión de estado solo con `useState` |
| No utilizar localStorage | Fuera del alcance del MVP |
| No utilizar otras librerías de gestión de estado | Mantener simplicidad |

### 10.3. Requisitos obligatorios

- Mantener consistencia de tipado con TypeScript
- Utilizar dataset local (`src/data/experiences.ts`)
- Respetar arquitectura App Router de Next.js
- Realizar navegación sin recargas completas de página
- Componentes lógicamente divididos (no monolíticos)

---

## 11. Criterios de aceptación verificables

Los siguientes criterios permiten validar la implementación:

### 11.1. Criterios de estructura

- [ ] **Rutas:** Existen las 5 rutas requeridas: `/`, `/experiences`, `/experiences/[id]`, `/favorites`, `/profile`
- [ ] **Componentes:** Existen al menos: Navbar, ExperienceCard, SearchBar, FilterBar
- [ ] **Dataset:** Archivo `src/data/experiences.ts` contiene 100 experiencias
- [ ] **Tipado:** Interfaz `Experience` definida con los 8 campos obligatorios
- [ ] **Organización:** Componentes divididos en carpetas lógicas (no monolíticos)

### 11.2. Criterios de búsqueda y filtros

- [ ] **Búsqueda por título:** La búsqueda filtra experiencias usando `/term/i` (regex case-insensitive)
- [ ] **Filtro por categoría:** Las experiencias se filtran correctamente por categoría seleccionada
- [ ] **Filtro por destino:** Las experiencias se filtran correctamente por destino seleccionado
- [ ] **Combinación de filtros:** Búsqueda + categoría + destino funcionan juntos (AND lógico)
- [ ] **Sin resultados:** Se muestra mensaje "No se encontraron resultados" cuando corresponde

### 11.3. Criterios de URL y sincronización

- [ ] **Query parameters:** `search`, `category`, `destination` aparecen en la URL de `/experiences`
- [ ] **Lectura de parámetros:** Usar `useSearchParams` para leer parámetros
- [ ] **Lectura de ruta:** Usar `usePathname` para identificar sección activa
- [ ] **Prellenado:** Acceder a URL con parámetros prerrellena campos y muestra resultados filtrados
- [ ] **Compartibilidad:** Copiar URL de búsqueda y abrirla en otra ventana reproduce la búsqueda

### 11.4. Criterios de favoritos

- [ ] **Almacenamiento:** Los favoritos se almacenan con `useState`
- [ ] **Icono visual:** ExperienceCard muestra icono de corazón con estado visual (relleno/vacío)
- [ ] **Persistencia:** El estado de favoritos se mantiene al navegar entre páginas
- [ ] **Lista de favoritos:** `/favorites` muestra solo experiencias marcadas como favoritas
- [ ] **Contador:** `/profile` muestra contador actualizado de favoritos

### 11.5. Criterios de hooks y lógica

- [ ] **useState:** Se utiliza correctamente para gestionar favoritos
- [ ] **useEffect:** Se utiliza en al menos un componente con dependencias correctas
- [ ] **Sin bucles infinitos:** useEffect no causa recargas infinitas
- [ ] **Custom hook:** Existe al menos un custom hook encapsulando lógica significativa

### 11.6. Criterios de UI/UX

- [ ] **Navbar presente:** Navbar visible en todas las páginas
- [ ] **Indicador activo:** usePathname resalta correctamente la sección activa
- [ ] **Responsividad:** Interfaz funciona en dispositivos móviles y escritorio
- [ ] **Coherencia visual:** Diseño consistente entre páginas

### 11.7. Criterios de código

- [ ] **TypeScript:** Tipos e interfaces utilizados consistentemente
- [ ] **Sin Redux:** No hay dependencias de Redux
- [ ] **Sin Zustand:** No hay dependencias de Zustand
- [ ] **Sin localStorage:** No se utiliza localStorage (fuera del alcance)
- [ ] **Commits:** Cambios registrados en Git con mensajes descriptivos

### 11.8. Criterios de documentación

- [ ] **Design References:** README.md documenta 2-3 referencias de diseño reales

---

## 12. Decisiones pendientes y supuestos

### 12.1. Decisiones resueltas en esta revisión

Todas las decisiones clave han sido resueltas mediante aprobación de producto:

| Decisión | Resolución |
|----------|-----------|
| **Filtro de categoría múltiple** | ✓ Single-select (una sola categoría) |
| **Filtro de destino múltiple** | ✓ Single-select (una solo destino) |
| **Búsqueda de destino** | ✓ Case-insensitive con coincidencia parcial |
| **Actualización de URL** | ✓ Automática al cambiar filtros (sin botón "Buscar") |
| **Error en /experiences/[id]** | ✓ Mostrar página de error 404 |
| **Escape de caracteres especiales** | ✓ Escape obligatorio antes de construir RegExp |
| **Mensaje sin resultados** | ✓ Exactamente: "No se encontraron resultados" |
| **Navbar en todas las páginas** | ✓ Requisito explícito (no inferido) |

### 12.2. Supuestos confirmados

| Supuesto | Confirmación | Riesgo |
|----------|-------------|--------|
| Las categorías son mutuamente excluyentes | ✓ Single-select | Bajo |
| El ID de experiencia es único | ✓ Tipo `string`, único | Bajo |
| Los parámetros de URL son opcionales | ✓ Todos opcionales | Bajo |
| Navbar está en un layout compartido | ✓ Buena práctica | Bajo |
| Favoritos: tipo `string[]` | ✓ IDs de tipo string | Bajo |
| URL es fuente de verdad | ✓ Parámetros reflejan estado | Medio |

### 12.3. Decisiones arquitectónicas pendientes

Las siguientes decisiones se tomarán en fases posteriores:

| Decisión | Opciones | Momento de decisión |
|----------|----------|-------------------|
| **Implementación de estado compartido** | Props drilling, Context API, o custom hooks | Fase: Definición de arquitectura de estado |
| **Ubicación exacta del layout con Navbar** | Root layout o layout compartida de /experiences | Fase: Inicialización de Next.js |
| **Estructura exacta de directorios** | Flexible, respetando estructura sugerida | Fase: Inicialización de Next.js |

---

## 13. Notas finales

### 13.1. Resumen de requisitos — Estado de completitud

**Completamente definidos y resueltos:**
✓ Rutas y navegación (5 rutas)  
✓ Modelo de datos (8 campos obligatorios, tipo `id: string`)  
✓ Búsqueda por título (regex case-insensitive con escape de caracteres especiales)  
✓ Filtro por categoría (single-select)  
✓ Filtro por destino (coincidencia parcial, case-insensitive)  
✓ Combinación de filtros (lógica AND)  
✓ Sincronización con URL (query parameters: search, category, destination)  
✓ Actualización automática de URL  
✓ Gestión de favoritos (useState, tipo `string[]`)  
✓ Componentes requeridos (Navbar, ExperienceCard, SearchBar, FilterBar)  
✓ Hooks requeridos (useState, useEffect, custom hook de filtrado)  
✓ Restricciones técnicas (no Redux, no Zustand, no localStorage)  
✓ Manejo de errores (error 404 cuando ID no existe)  
✓ Navbar en todas las páginas (requisito explícito)  

**Parcialmente definidos (decisiones arquitectónicas posteriores):**
~ Implementación específica de estado compartido (props, Context, custom hooks)  
~ Ubicación exacta de componentes en la estructura de directorios  

**Requisitos cerrados:** No hay decisiones pendientes sobre requisitos funcionales

### 13.2. Cambios en esta revisión v1.1

**Cambios realizados:**
1. Actualizado tipo `id` a `string` (exclusivamente)
2. Eliminadas propuestas de atributos adicionales (duration, participants, reviews, language, difficulty)
3. Especificadas reglas de escape de caracteres en regex
4. Definido filtro de categoría como single-select
5. Definido filtro de destino con coincidencia parcial case-insensitive
6. Confirmada actualización automática de URL sin botón de búsqueda
7. Especificado mensaje exacto "No se encontraron resultados"
8. Cambio de favoritos a tipo `string[]`
9. Separación clara de custom hook de filtrado vs. `useFavorites`
10. Cambio de Navbar a requisito explícito
11. Especificado error 404 para IDs no existentes
12. Especificada presencia de enlace para regresar en /experiences/[id]
13. Eliminadas todas las decisiones pendientes resueltas
14. Versionado a 1.1

---

## 14. Próximos pasos y fases del proyecto

Una vez aprobada esta especificación, el proyecto seguirá las siguientes fases en orden:

### Fase 1: Investigación y diseño visual
1. **Investigación de referencias:** Analizar 2-3 interfaces reales de plataformas similares
2. **Documentación de referencias:** Registrar referencias en README.md bajo sección `## Design References`

### Fase 2: Diseño
3. **Diseño de interfaces:** Crear prototipos de las 5 páginas en Google Stitch
4. **Revisión y aprobación:** Validación de diseños antes de implementación

### Fase 3: Generación de especificación visual
5. **Generación de DESIGN_SPEC.md:** Mediante herramienta Visual to Spec (si aplica)

### Fase 4: Inicialización técnica
6. **Inicialización de Next.js:** Crear proyecto con create-next-app, App Router y TypeScript
7. **Definición del contrato de datos:** Especificar interfaz TypeScript `Experience`
8. **Generación del dataset:** Crear 100 experiencias en `src/data/experiences.ts`

### Fase 5: Arquitectura de estado
9. **Definición de arquitectura:** Resolver cómo implementar estado compartido (props, Context API)
10. **Implementación de custom hooks:** Crear `useFilters` y `useFavorites`

### Fase 6: Desarrollo incremental
11. **Desarrollo de componentes:** Implementar Navbar, ExperienceCard, SearchBar, FilterBar
12. **Desarrollo de páginas:** Implementar `/`, `/experiences`, `/experiences/[id]`, `/favorites`, `/profile`
13. **Integración:** Conectar componentes, sincronización con URL, gestión de favoritos

### Fase 7: Validación
14. **Testing:** Validar todos los criterios de aceptación (sección 11)
15. **Ajustes finales:** Responsividad, coherencia visual, optimizaciones

### Fase 8: Despliegue (opcional)
16. **Despliegue en Vercel:** Publicación de la aplicación (opcional según requisitos)

---

**Documento generado como parte de Spec-Driven Development (SDD)**  
**Versión:** 1.1  
**Última actualización:** 2026-11-25  
**Estado:** Aprobado y listo para implementación
