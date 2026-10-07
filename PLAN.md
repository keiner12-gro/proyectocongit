# Plan: App de comunicación interna y RRHH (tipo Humand)

Basado en el análisis de las pantallas de la app de referencia.

## 1. Estructura de navegación

Barra inferior con 4 pestañas:

| Pestaña | Contenido |
|---------|-----------|
| **Inicio** | Logo de la empresa, iconos de cumpleaños, búsqueda y notificaciones. Sub-pestañas **Muro** y **Noticias**. |
| **Chats** | Lista de conversaciones, buscador, botón `+` para nuevo chat o grupo. |
| **Apps** | Cuadrícula de módulos (ver sección 2), con badge de pendientes. |
| **Perfil** | Datos del empleado, configuración, cerrar sesión. |

## 2. Módulos

### Inicio (Muro / Noticias)
- Publicaciones fijadas ("Ver todas").
- Post: autor (avatar o iniciales), fecha relativa, texto, imagen o enlace.
- Reacciones con emojis + contador, comentarios.
- Noticias: publicaciones oficiales de RRHH.
- Cumpleaños del día.

### Chats
- Chat 1 a 1 y grupos.
- Directorio de empleados ordenado alfabéticamente con buscador.
- Estado vacío: "Aún no tienes mensajes".

### Apps (módulos)
1. **Formularios y Trámites**: formularios dinámicos por pasos (1/3, 2/3...), campos obligatorios (*), pestañas Disponibles / Completados.
2. **Mis documentos**: carpetas asignadas por RRHH (desprendibles, certificados).
3. **Encuestas**: Disponibles / Completados, buscador, badge de pendientes.
4. **Vacaciones y permisos**:
   - Tarjetas por tipo de política (luto, compensatorio, cumpleaños, pacto, médico, remunerado, no remunerado) con días disponibles o utilizados.
   - Botón **Solicitar**.
   - Lista de solicitudes con estado (Pendiente / Aprobada / Rechazada).
   - Vista **Calendario**.
5. **Desempeño**: evaluaciones y objetivos.
6. **Portal de servicios**: solicitudes a áreas internas (tickets).
7. **Eventos**: calendario de eventos con inscripción.
8. **Librerías**: documentos y manuales de la empresa.

## 3. Panel web de administración (RRHH)
- Cargar empleados (Excel/CSV), áreas y cargos.
- Publicar en el muro, fijar publicaciones.
- Crear formularios y encuestas (constructor de campos).
- Configurar políticas de permisos y aprobar solicitudes.
- Subir documentos por empleado.
- Personalizar marca: logo y colores por empresa.

## 4. Stack técnico
- **Móvil:** React Native + Expo + Expo Router.
- **Web admin:** Next.js.
- **Backend:** Supabase (Auth, Postgres, Storage, Realtime para chat, Edge Functions).
- **Push:** Expo Notifications.
- **Multiempresa:** columna `empresa_id` en todas las tablas + Row Level Security.

## 5. Modelo de datos (inicial)
```
empresas(id, nombre, logo_url, color_primario)
empleados(id, empresa_id, nombre, documento, cargo, area, jefe_id, avatar_url, fecha_nacimiento)
publicaciones(id, empresa_id, autor_id, tipo[muro|noticia], texto, media_url, fijada, creada_en)
reacciones(publicacion_id, empleado_id, emoji)
comentarios(id, publicacion_id, empleado_id, texto, creado_en)
chats(id, empresa_id, es_grupo, nombre)
chat_miembros(chat_id, empleado_id)
mensajes(id, chat_id, autor_id, texto, creado_en)
formularios(id, empresa_id, titulo, tipo[tramite|encuesta], esquema_json)
respuestas(id, formulario_id, empleado_id, datos_json, creada_en)
politicas_permiso(id, empresa_id, nombre, icono, dias_por_anio)
solicitudes_permiso(id, empleado_id, politica_id, desde, hasta, estado, aprobador_id)
documentos(id, empleado_id, carpeta, nombre, archivo_url)
eventos(id, empresa_id, titulo, fecha, lugar)
```

## 6. Fases (aprox. 4 meses)
| Fase | Entregable | Tiempo |
|------|-----------|--------|
| 1 | Diseño en Figma + modelo de datos | 2 sem |
| 2 | Login por empresa, perfil, navegación de 4 pestañas | 2 sem |
| 3 | Muro: posts, reacciones, comentarios, fijados | 3 sem |
| 4 | Vacaciones y permisos (flujo completo con aprobación) | 3 sem |
| 5 | Formularios/encuestas dinámicos + Mis documentos | 3 sem |
| 6 | Chat en tiempo real | 2 sem |
| 7 | Panel admin web + pruebas piloto + publicación en tiendas | 3 sem |

Desempeño, Portal de servicios, Eventos y Librerías quedan para una versión 2.

## 7. Mejoras de UX para diferenciarse
- Modo sin conexión: guardar formularios y enviarlos al recuperar señal (trabajadores de campo).
- Calendario de permisos que muestre cuántos días quedan **antes** de solicitar.
- Mostrar en el formulario de solicitud el saldo de días y validar fechas.
- Textos y botones grandes; fechas con selector, no texto libre (DD/MM/YYYY).
- Nombres en formato normal, no TODO EN MAYÚSCULAS.
- Estados vacíos con una acción ("Aún no tienes mensajes" → botón "Iniciar chat").

## 8. Modelo de negocio
- SaaS: cobro mensual por empleado activo.
- Demo personalizada con logo y colores del cliente.
- Ventajas: precio local, soporte en Colombia, funciones a la medida (turnos, campo, offline).
- Cumplir Ley 1581 de 2012 (protección de datos personales): política de privacidad y autorización de tratamiento de datos.
