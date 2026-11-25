# PROJECT BRIEF — Wanderlust Explorer

## 1. Contexto del proyecto

Wanderlust Labs es una startup de travel-tech que está construyendo una plataforma digital para descubrir, explorar y guardar experiencias únicas alrededor del mundo.

El producto permitirá a los usuarios encontrar actividades turísticas de diferentes categorías y destinos mediante una interfaz intuitiva, visualmente atractiva y adaptada a dispositivos móviles y de escritorio.

## 2. Objetivo

Desarrollar un MVP funcional de Wanderlust Explorer: una aplicación web multipágina que permita explorar, buscar, filtrar, consultar y guardar experiencias turísticas sin recargas completas de página durante la navegación.

## 3. Stack tecnológico

- React.
- Next.js con App Router.
- TypeScript.
- Tailwind CSS.
- ESLint.
- Git y GitHub.

El proyecto se inicializará desde cero utilizando create-next-app.

## 4. Requisitos funcionales

### 4.1. Página de inicio — /

- Sección hero de presentación de la plataforma.
- Botón principal de acceso al explorador de experiencias.

### 4.2. Explorador — /experiences

- Visualización de las 100 experiencias disponibles mediante una cuadrícula de tarjetas.
- Barra de búsqueda por título.
- Filtro por categoría.
- Filtro por ciudad o país de destino.
- Posibilidad de combinar la búsqueda y ambos filtros.
- Acceso a la página de detalle de cada experiencia.
- Posibilidad de añadir o eliminar experiencias de favoritos.
- Mensaje "No se encontraron resultados" cuando no existan coincidencias.

### 4.3. Detalle de experiencia — /experiences/[id]

- Obtención del identificador de la experiencia desde la URL.
- Recuperación de la información correspondiente desde el dataset local.
- Visualización de la información completa de la experiencia.

### 4.4. Favoritos — /favorites

- Visualización exclusiva de las experiencias marcadas como favoritas por el usuario.

### 4.5. Perfil — /profile

- Presentación de un perfil de usuario simulado.
- Contador actualizado del número de experiencias guardadas como favoritas.

## 5. Modelo de datos

La aplicación dispondrá de un dataset local de 100 experiencias turísticas, almacenado en:

`src/data/experiences.ts`

Se definirá una interfaz TypeScript denominada `Experience`, utilizada de forma consistente en todo el proyecto.

Cada experiencia deberá contener, como mínimo:

| Campo | Descripción |
|---|---|
| id | Identificador de la experiencia |
| title | Título de la experiencia |
| description | Descripción completa |
| category | Categoría |
| destination | Ciudad y país |
| price | Precio |
| rating | Valoración |
| imageUrl | URL de la imagen |

Las categorías permitidas serán exclusivamente:

- Adventure
- Culture
- Food
- Wellness
- Nature

Las imágenes podrán utilizar recursos placeholder.

## 6. Búsqueda y filtros

### 6.1. Búsqueda por título

La búsqueda deberá utilizar expresiones regulares case-insensitive para identificar coincidencias en el título de las experiencias.

### 6.2. Filtro por categoría

El usuario podrá seleccionar cualquiera de las cinco categorías disponibles.

### 6.3. Filtro por destino

El usuario podrá filtrar las experiencias por ciudad o país.

### 6.4. Combinación de filtros

La búsqueda y los filtros deberán funcionar de forma independiente y poder combinarse simultáneamente.

Los resultados deberán actualizarse sin recargar completamente la página.

## 7. Sincronización con la URL

El término de búsqueda y los filtros activos deberán reflejarse en la URL mediante query parameters.

Se utilizarán las herramientas de navegación de Next.js:

- useSearchParams.
- usePathname.

Cuando se acceda directamente a una URL que contenga parámetros de búsqueda:

- Los campos correspondientes deberán aparecer prerrellenados.
- Los resultados deberán mostrarse con los filtros aplicados.

El objetivo es permitir compartir enlaces que reproduzcan una búsqueda concreta.

## 8. Gestión de favoritos

Cada tarjeta de experiencia deberá incluir un icono de corazón que permita añadir o eliminar la experiencia de la lista de favoritos.

El icono deberá reflejar visualmente el estado actual.

Los identificadores de las experiencias favoritas deberán almacenarse mediante useState en un nivel compartido de la aplicación.

El estado y las acciones correspondientes deberán transmitirse a los componentes mediante props donde sea necesario.

La información deberá mantenerse disponible durante la navegación entre las páginas de la aplicación.

No se requiere persistencia entre recargas completas de página.

## 9. Componentes reutilizables

La aplicación deberá incluir, como mínimo, los siguientes componentes:

- ExperienceCard.
- SearchBar.
- FilterBar.
- Navbar.

Los componentes deberán estar organizados de forma lógica, evitando concentrar toda la aplicación en un único archivo.

## 10. Hooks de React

La aplicación deberá utilizar:

- useState para gestionar los favoritos.
- useEffect en al menos un componente, con una gestión correcta de sus dependencias.
- Al menos un custom hook que encapsule la lógica de filtrado, como useExperiences o useFilters.

## 11. Diseño y experiencia de usuario

La interfaz deberá proporcionar una experiencia de descubrimiento limpia, intuitiva y visualmente coherente.

El diseño deberá contemplar:

- Navegación clara.
- Tarjetas de experiencias.
- Protagonismo de las imágenes.
- Barra de búsqueda.
- Controles de filtrado.
- Estados visuales de favoritos.
- Estado sin resultados.
- Adaptación a dispositivos móviles y de escritorio.

La Navbar deberá estar presente en todas las páginas e identificar visualmente la sección activa mediante usePathname.

Antes de implementar los componentes visuales, deberán investigarse dos o tres interfaces reales que sirvan como referencia de diseño.

Estas referencias deberán documentarse en README.md bajo la sección `## Design References`.

## 12. Restricciones técnicas

- Utilizar React y Next.js con App Router.
- Mantener un tipado consistente mediante TypeScript.
- No utilizar Redux, Zustand ni otras librerías externas de gestión de estado.
- Gestionar el estado mediante useState nativo de React y transmitirlo mediante props o custom hooks.
- No utilizar localStorage, cuya implementación queda fuera del alcance del MVP.
- Utilizar un dataset local.
- Realizar la navegación entre páginas sin recargas completas.

## 13. Criterios de aceptación

- [ ] La aplicación dispone de las cinco páginas requeridas con navegación del lado del cliente.
- [ ] La búsqueda filtra las experiencias por título mediante expresiones regulares case-insensitive.
- [ ] Los filtros de categoría y destino funcionan independientemente y se combinan correctamente con la búsqueda.
- [ ] La búsqueda y los filtros activos se reflejan en la URL.
- [ ] El acceso directo mediante una URL con query parameters prerrellena los campos y muestra los resultados filtrados.
- [ ] useEffect se utiliza correctamente, sin bucles infinitos ni dependencias faltantes.
- [ ] useState gestiona los favoritos y transmite correctamente la información mediante props.
- [ ] Existe al menos un custom hook que encapsula lógica significativa.
- [ ] Los componentes están divididos y organizados de forma lógica.
- [ ] Los tipos e interfaces de TypeScript se utilizan consistentemente.
- [ ] La aplicación es responsiva y visualmente coherente entre páginas.
- [ ] Las referencias de diseño están documentadas en README.md.

## 14. Entregables

- Aplicación funcional Wanderlust Explorer.
- Código fuente organizado y documentado.
- Dataset local de 100 experiencias.
- README.md con referencias de diseño.
- Repositorio público de GitHub con todos los cambios registrados mediante commits.

El despliegue de la aplicación en una plataforma como Vercel será opcional.

## 15. Alcance

El proyecto corresponde a un MVP de exploración de experiencias turísticas.

La persistencia de favoritos entre recargas completas de página queda fuera del alcance actual.