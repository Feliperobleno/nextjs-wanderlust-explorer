# SPECIFICATION — Wanderlust Explorer

## 1. Descripción del proyecto

**Nombre:** Wanderlust Explorer  
**Versión:** MVP 1.2  
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
| Visualización de experiencias | Mostrar catálogo de 100 experiencias en cuadrícula de tarjetas paginadas en grupos de 12 | ✓ | Definido |
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
- `page` (number): número de página (12 experiencias por página)

**Paginación:**
- 12 experiencias por página
- Página inicial: 1
- Reiniciar a página 1 al modificar búsqueda o filtros

**Componentes esperados:**
- Navbar
- SearchBar
- FilterBar
- ExperienceCard (reutilizable)
- Grid/Container para visualización
- Controles de paginación

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
| Atributos del perfil | id, firstName, lastName, email, avatarUrl, city, country (tipo string) | ✓ | Definido |
| Contador de favoritos | Mostrar número de experiencias guardadas (dinámico) | ✓ | Definido |
| Contador actualizado | El contador se sincroniza con cambios en favoritos | ✓ | Definido |
| Navbar activa | Navbar presente indicando sección activa | ✓ | Definido |

**Restricciones:**
- No implementar autenticación
- No permitir edición del perfil
- Contador de favoritos se calcula del estado compartido (read-only)

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
- Utilizar expresiones regulares case-insensitive
- Buscar coincidencias en el campo `title` de las experiencias
- Escapar caracteres especiales del término de búsqueda antes de construir la expresión regular
- Búsqueda case-insensitive e insensible a acentos

**Comportamiento esperado:**
- La búsqueda se aplica automáticamente mientras el usuario escribe (sin necesidad de botón de búsqueda)
- Si se borra el término, desaparece el filtro de búsqueda
- Puede combinarse con filtros de categoría y destino
- NO se implementa debouncing en esta fase

**Implementación requerida:**
```typescript
const escapedTerm = searchTerm.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
const normalizedTerm = escapedTerm.normalize('NFD').replace(/[\u0300-\u036f]/g, '');
const regex = new RegExp(normalizedTerm, 'i');
const normalizedTitle = experience.title.normalize('NFD').replace(/[\u0300-\u036f]/g, '');
const matches = normalizedTitle.match(regex) !== null;
```

**Ejemplo:**
- Búsqueda: "playa" → coincide con "Playa", "PLAYA", "plaïa"

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
- Insensible a acentos: "café" coincide con "cafe", "Café", "CAFE"
- Si se ingresa un término, solo muestra experiencias cuyo `destination` contiene ese término
- Si se limpia el filtro, desaparece
- Puede combinarse con búsqueda y filtro de categoría
- Se aplica automáticamente sin necesidad de confirmar

**Implementación requerida:**
```typescript
const normalizedDestination = destination.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();
const normalizedField = experience.destination.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();
const matches = normalizedField.includes(normalizedDestination);
```

**Ejemplo:**
- Filtro: "São Paulo" → coincide con "sao paulo", "SÃO PAULO", "Sao Paulo"

---

### 5.4. Combinación de filtros y ordenamiento

**Requisito explícito:**
- La búsqueda y los filtros funcionan independientemente y pueden combinarse
- Los resultados se actualizan sin recargar completamente la página
- Orden de operaciones: filtrado → ordenamiento → paginación

**Lógica esperada:**
```
resultados = experiencias
  .filter(exp => buscar_en_titulo(exp, término_búsqueda))
  .filter(exp => filtrar_por_categoría(exp, categoría))
  .filter(exp => filtrar_por_destino(exp, destino))
  .sort(ordenamiento)
  .paginate(page, pageSize)
```

**Comportamiento esperado:**
- Todos los filtros se aplican con lógica AND (intersección)
- Si algún filtro está vacío, se omite de la evaluación
- Ordenamiento por defecto: rating descendente
- Resolver empates mediante orden alfabético por título

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
| `page` | number | Número de página | No (default: 1) |
| `sort` | string | Criterio de ordenamiento | No (default: rating-desc) |

**Valores válidos para `sort`:**
- `rating-desc` (default)
- `price-asc`
- `price-desc`

**Comportamiento con parámetros inválidos:**
- Categorías inválidas: Se ignoran
- Parámetros vacíos: Se eliminan de la URL
- Parámetros duplicados: Se utiliza el primer valor

**Ejemplo de URL:**
```
/experiences?search=beach&category=Adventure&destination=Hawaii&page=2&sort=price-asc
```

**Comportamiento esperado:**
- La URL es la fuente de verdad para los filtros activos
- Los parámetros se actualizan automáticamente cuando el usuario modifica búsqueda o filtros
- Los parámetros se eliminan de la URL cuando se limpian los filtros correspondientes
- Todos los parámetros son opcionales
- Al modificar búsqueda o filtros, la página se reinicia a 1

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
**Patrón:** Context API + Provider con useState interno  
**Nota:** `useFavorites` es un hook complementario opcional, no sustituye la gestión de estado directa

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
- **Context API y useState:** Para gestión de estado (favoritos)

### 10.2. Prohibiciones explícitas

| Restricción | Razón |
|-------------|-------|
| No utilizar Redux | Gestión de estado solo con `useState` |
| No utilizar Zustand | Gestión de estado solo con `useState` |
| No utilizar localStorage | Fuera del alcance del MVP |
| No utilizar otras librerías de gestión de estado | Mantener simplicidad |

### 10.3. Requisitos de implementación

- **State Management:** Context API + Provider con useState interno
- **Props:** Transmitir props necesarias a componentes reutilizables
- **Custom hooks:** Mantener obligatorio custom hook de filtrado
- **useFavorites:** Hook complementario (no obligatorio como requerimiento independiente)
- **Directorio de hooks:** `src/hooks/`
- **Directorio de componentes:** `src/components/`
- **Validación de dataset:** Validar 100 registros durante desarrollo, sin mecanismos adicionales en cada carga

---

## 11. Matriz de validación

| ID | Requisito | Método de validación | Resultado esperado | Estado |
|----|-----------|--------------------|------------------|--------|
| **R-001** | Rutas existentes | Verificar existencia de archivos | Existen 5 rutas (/, /experiences, /experiences/[id], /favorites, /profile) | Pendiente |
| **R-002** | Componentes base | Verificar archivos de componentes | Existen: Navbar, ExperienceCard, SearchBar, FilterBar | Pendiente |
| **R-003** | Dataset | Verificar `src/data/experiences.ts` | Array con 100 objetos Experience | Pendiente |
| **R-004** | Interfaz Experience | Verificar tipos TypeScript | Interfaz con 8 campos (id, title, description, category, destination, price, rating, imageUrl) | Pendiente |
| **R-005** | Búsqueda por título | Input de texto, verificar filtrado | Se filtra por regex case-insensitive e insensible a acentos con escape de caracteres | Pendiente |
| **R-006** | Filtro por categoría | Select de categoría, verificar filtrado | Se filtra por una sola categoría (single-select) | Pendiente |
| **R-007** | Filtro por destino | Input de destino, verificar filtrado | Se filtra por coincidencia parcial, case-insensitive | Pendiente |
| **R-008** | Combinación de filtros | Aplicar múltiples filtros | Resultados muestran intersección (AND lógico) | Pendiente |
| **R-009** | Ordenamiento | Verificar orden de resultados | Default: rating descendente; soporta precio asc/desc; empates por título alfabético | Pendiente |
| **R-010** | Paginación | Verificar controles de página | 12 experiencias por página; reinicia a página 1 al cambiar filtros | Pendiente |
| **R-011** | URL parameters | Acceder con parámetros en URL | search, category, destination, page, sort sincronizados | Pendiente |
| **R-012** | URL sincronización | Cambiar filtros, verificar URL | URL se actualiza automáticamente al cambiar filtros | Pendiente |
| **R-013** | URL prellenado | Acceder con parámetros | Campos se prerrellanan y resultados se muestran filtrados | Pendiente |
| **R-014** | Sin resultados | Buscar sin coincidencias | Muestra mensaje exacto: "No se encontraron resultados" | Pendiente |
| **R-015** | Favoritos storage | Verificar estado | useState<string[]> gestiona IDs de favoritos | Pendiente |
| **R-016** | Icono favorito | Click en corazón | Añade/elimina de favoritos y actualiza UI (relleno/vacío) | Pendiente |
| **R-017** | Favoritos persistencia | Navegar entre páginas | Favoritos se mantienen durante navegación | Pendiente |
| **R-018** | Página /favorites | Acceder a /favorites | Muestra solo experiencias marcadas como favoritas | Pendiente |
| **R-019** | Perfil simulado | Acceder a /profile | Muestra atributos: id, firstName, lastName, email, avatarUrl, city, country | Pendiente |
| **R-020** | Contador dinámico | Cambiar favoritos | Contador en /profile se actualiza automáticamente | Pendiente |
| **R-021** | Navbar en todas páginas | Verificar visibilidad | Navbar presente en todas las 5 rutas | Pendiente |
| **R-022** | Indicador activo | Navegar entre páginas | usePathname resalta correctamente la sección activa | Pendiente |
| **R-023** | Enlace regresar | Acceder a /experiences/[id] | Existe enlace/botón visible para regresar a /experiences | Pendiente |
| **R-024** | Error 404 | Acceder a ID inexistente | Muestra página de error 404 personalizada con Navbar y enlace a explorador | Pendiente |
| **R-025** | Parámetros inválidos | URL con categoría inválida | Se ignoran parámetros inválidos | Pendiente |
| **R-026** | Parámetros vacíos | URL con parámetro vacío | Se eliminan parámetros vacíos | Pendiente |
| **R-027** | Parámetros duplicados | URL con parámetro duplicado | Se utiliza primer valor | Pendiente |
| **R-028** | useState uso | Buscar en código | Se utiliza para gestionar favoritos | Pendiente |
| **R-029** | useEffect uso | Buscar en código | Se utiliza en al menos 1 componente con dependencias correctas | Pendiente |
| **R-030** | useEffect sin loops | Verificar rendimiento | No hay recargas infinitas | Pendiente |
| **R-031** | Custom hook filtrado | Verificar `useFilters` | Existe hook encapsulando lógica de búsqueda y filtrado | Pendiente |
| **R-032** | useSearchParams | Verificar importación | Se utiliza para leer parámetros de URL | Pendiente |
| **R-033** | usePathname | Verificar importación | Se utiliza para identificar sección activa | Pendiente |
| **R-034** | TypeScript tipos | ESLint/TypeScript | Tipos utilizados consistentemente | Pendiente |
| **R-035** | No Redux | Verificar dependencias | No hay importaciones de Redux | Pendiente |
| **R-036** | No Zustand | Verificar dependencias | No hay importaciones de Zustand | Pendiente |
| **R-037** | No localStorage | Verificar código | No se utiliza localStorage | Pendiente |
| **R-038** | Responsividad mobile | Verificar en viewport 320px | Funciona sin desplazamiento horizontal innecesario | Pendiente |
| **R-039** | Responsividad desktop | Verificar en viewport 1920px | Interfaz adapta correctamente a pantallas grandes | Pendiente |
| **R-040** | Accesibilidad WCAG AA | Verificar atributos ARIA | 1.1.1: Alt texto en imágenes; 1.3.1: Semántica HTML; 1.4.1: No solo color; 1.4.3: Contraste 4.5:1 (texto normal), 3:1 (texto grande); 1.4.10: Adaptable a 320px sin scroll horizontal; 1.4.11: Contraste 3:1 en componentes; 2.1.1: Navegación teclado; 2.4.7: Foco visible; 2.5.8: Objetivos 24x24px mín; 3.3.2: Etiquetas accesibles en inputs; 4.1.2: Nombre/función/estado accesibles; 4.1.3: Cambios de resultados comunicados | Pendiente |
| **R-041** | ExperienceCard | Verificar componente | Muestra información resumida + botón favorito + link a detalle | Pendiente |
| **R-042** | Coherencia visual | Revisión visual | Diseño consistente entre páginas, referencias visuales (Airbnb, GetYourGuide, Civitatis) | Pendiente |
| **R-043** | Escape caracteres | Verificar regex | Caracteres especiales se escapan antes de construir RegExp | Pendiente |
| **R-044** | Context API | Verificar Provider | Se utiliza Context API para compartir estado de favoritos | Pendiente |
| **R-045** | Búsqueda sin debouncing | Verificar comportamiento | Búsqueda se actualiza inmediatamente sin debouncing | Pendiente |

---

## 12. Decisiones aprobadas y requisitos finalizados

### 12.1. Decisiones resueltas en esta revisión v1.2

Todas las decisiones clave han sido resueltas mediante aprobación de producto:

| Decisión | Resolución |
|----------|-----------|
| **Paginación** | ✓ 12 experiencias por página, parámetro `page` en URL |
| **Ordenamiento** | ✓ Default: rating descendente; soporta precio asc/desc |
| **Reinicio de página** | ✓ Vuelve a página 1 al modificar búsqueda o filtros |
| **Parámetros inválidos** | ✓ Ignorar categorías inválidas, eliminar parámetros vacíos, usar primer valor duplicado |
| **Perfil usuario** | ✓ Simulado con 7 atributos string, contador dinámico |
| **Autenticación y edición** | ✓ No implementar |
| **State Management** | ✓ Context API + useState (no Redux, no Zustand) |
| **Accesibilidad** | ✓ Implementar criterios WCAG 2.2 AA especificados |
| **Búsqueda sin debouncing** | ✓ No implementar debouncing en esta fase |
| **Tailwind CSS** | ✓ Framework de estilos obligatorio |
| **Validación dataset** | ✓ Validar 100 registros durante dev, sin mecanismos en cada carga |
| **Referencias visuales** | ✓ Airbnb Experiences, GetYourGuide, Civitatis |

### 12.2. Supuestos confirmados

| Supuesto | Confirmación | Riesgo |
|----------|-------------|--------|
| Las categorías son mutuamente excluyentes | ✓ Single-select | Bajo |
| El ID de experiencia es único | ✓ Tipo `string`, único | Bajo |
| Los parámetros de URL son opcionales | ✓ Todos opcionales | Bajo |
| Navbar está en un layout compartido | ✓ Buena práctica | Bajo |
| Favoritos: tipo `string[]` | ✓ IDs de tipo string | Bajo |
| URL es fuente de verdad | ✓ Parámetros reflejan estado | Medio |
| Orden de operaciones | ✓ Filtrado → Ordenamiento → Paginación | Bajo |

### 12.3. Requisitos finalizados

Los siguientes requisitos están completamente definidos:

✓ Rutas y navegación (5 rutas)  
✓ Paginación (12 por página, parámetro `page`)  
✓ Ordenamiento (default rating desc, soporta precio asc/desc)  
✓ Modelo de datos (8 campos obligatorios, tipo `id: string`)  
✓ Perfil usuario (7 campos string, contador dinámico, no editable)  
✓ Búsqueda por título (regex case-insensitive, insensible a acentos, con escape)  
✓ Filtro por categoría (single-select)  
✓ Filtro por destino (coincidencia parcial, case-insensitive)  
✓ Combinación de filtros (lógica AND)  
✓ Sincronización con URL (parámetros: search, category, destination, page, sort)  
✓ Actualización automática de URL  
✓ Gestión de parámetros inválidos (ignorar, eliminar, usar primer valor)  
✓ Gestión de favoritos (useState<string[]>)  
✓ Componentes requeridos (Navbar, ExperienceCard, SearchBar, FilterBar)  
✓ Hooks requeridos (useState, useEffect, custom hook de filtrado)  
✓ State Management (Context API + Provider con useState)  
✓ Restricciones técnicas (no Redux, no Zustand, no localStorage)  
✓ Manejo de errores (error 404 cuando ID no existe, con contenido completo)  
✓ Navbar en todas las páginas (requisito explícito)  
✓ Accesibilidad (criterios WCAG 2.2 AA especificados)  
✓ Tailwind CSS obligatorio  

---

## 13. Notas finales

### 13.1. Resumen de requisitos — Estado de completitud

**Completamente definidos en v1.2:**
✓ Rutas y navegación (5 rutas)  
✓ Paginación (12 experiencias, parámetro `page`, reinicio en filtros)  
✓ Ordenamiento (rating desc default, precio asc/desc, empates alfabéticos)  
✓ Modelo de datos (8 campos, tipo `id: string`)  
✓ Perfil usuario (7 campos string, contador dinámico, no editable)  
✓ Búsqueda por título (regex case-insensitive, insensible a acentos, con escape)  
✓ Filtro por categoría (single-select)  
✓ Filtro por destino (coincidencia parcial, case-insensitive)  
✓ Combinación de filtros (lógica AND, orden: filtrado → ordenamiento → paginación)  
✓ Sincronización con URL (parámetros: search, category, destination, page, sort)  
✓ Manejo de parámetros (ignorar inválidos, eliminar vacíos, primer valor duplicado)  
✓ Gestión de favoritos (useState<string[]>, Context API)  
✓ Componentes requeridos (Navbar, ExperienceCard, SearchBar, FilterBar)  
✓ Hooks requeridos (useState, useEffect, custom hook filtrado)  
✓ State Management (Context API + Provider, no Redux/Zustand)  
✓ Restricciones técnicas (no localStorage, Tailwind CSS obligatorio)  
✓ Errores (404 personalizada con contenido)  
✓ Accesibilidad (WCAG 2.2 AA especificados)  

### 13.2. Cambios en esta revisión v1.2

**Decisiones de producto aprobadas:**
1. Paginación: 12 experiencias por página con parámetro `page`
2. Ordenamiento: Rating descendente default, opciones de precio asc/desc
3. Reinicio de página: A 1 cuando cambian búsqueda o filtros
4. Perfil usuario: Atributos específicos (id, firstName, lastName, email, avatarUrl, city, country)
5. State Management: Context API + Provider con useState confirmado
6. Búsqueda: Insensible a acentos (normalización Unicode), sin debouncing
7. Filtro destino: Insensible a acentos (normalización Unicode)
8. Parámetros URL: Gestión de inválidos, vacíos y duplicados
9. useFavorites: Hook complementario opcional, no obligatorio como requerimiento independiente
10. Accesibilidad: Doce criterios WCAG 2.2 AA incorporados
5. Parámetros URL: Incluir `page` y `sort`
6. Gestión de errores: Ignorar categorías inválidas, eliminar vacíos, usar primer duplicado
7. State Management: Context API confirmado como patrón
8. Accesibilidad: Criterios WCAG 2.2 AA específicos incluidos
9. Sin debouncing: No implementar en esta fase
10. Matriz de validación: Reemplazada con tabla única concisa (45 requisitos)

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
**Versión:** 1.2  
**Última actualización:** 2026-11-25  
**Estado:** Aprobado y listo para implementación
