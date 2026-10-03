
## Objetivo

Determinar los actores que interactúan con el sistema DMGOTRAVEL y delimitar las responsabilidades de cada uno dentro del proceso de consulta, reserva y administración de servicios turísticos.

DMGOTRAVEL se plantea como un sistema web de gestión de reservas y servicios de una operadora turística. La solución considera dos perfiles principales de usuario: **Cliente** y **Administrador**. El acceso a las funciones protegidas se controla mediante autenticación y autorización basada en roles.

## Actores del sistema

| Actor | Tipo | ¿Qué necesita realizar? |
|---|---|---|
| **Cliente** | Primario | Consultar el catálogo público, visualizar servicios y paquetes turísticos, registrarse, autenticarse, crear reservas, consultar su historial y cancelar sus propias reservas cuando se encuentren en estado permitido. |
| **Administrador** | Primario | Administrar servicios y paquetes, configurar capacidad, precio, duración e imágenes 360°, consultar todas las reservas, cambiar su estado, consultar clientes y revisar el registro de auditoría. |
| **Servicio de autenticación** | Interno / soporte | Gestionar la autenticación de usuarios y la emisión/verificación de tokens API utilizados para acceder a recursos protegidos. |
| **Servicio de almacenamiento de imágenes** | Externo / infraestructura | Almacenar las imágenes asociadas a los servicios o paquetes turísticos y proporcionar las URL accesibles que se registran en el sistema. |
| **Base de datos MariaDB** | Infraestructura | Persistir usuarios, roles/permisos, servicios o paquetes, reservas y registros de auditoría, manteniendo integridad referencial y consistencia transaccional. |
| **Frontend web** | Cliente consumidor | Presentar la interfaz de usuario y consumir exclusivamente la API REST del backend para ejecutar los flujos de cliente y administrador. |

## Responsabilidad de los actores principales

### Cliente

El Cliente representa al turista que utiliza DMGOTRAVEL para descubrir y reservar servicios. Su interacción comienza en el catálogo público y continúa con la autenticación cuando desea realizar operaciones protegidas.

Operaciones principales:

- Consultar servicios y paquetes activos.
- Visualizar información relevante como título, descripción, precio, capacidad, duración e imagen 360°.
- Crear una cuenta.
- Iniciar sesión.
- Crear una reserva para un servicio o paquete activo.
- Consultar sus propias reservas.
- Cancelar una reserva propia mientras permanezca en estado **pending**.

### Administrador

El Administrador representa al personal de DMGOTRAVEL encargado de mantener y controlar la operación del sistema.

Operaciones principales:

- Gestionar servicios y paquetes mediante operaciones CRUD.
- Activar, actualizar o desactivar ofertas turísticas.
- Configurar precio, capacidad, duración e imágenes.
- Consultar todas las reservas.
- Confirmar, cancelar o completar reservas.
- Consultar el listado de clientes.
- Revisar eventos relevantes registrados en la auditoría.

## Frontera del sistema

El sistema DMGOTRAVEL concentra la lógica de negocio, persistencia y seguridad dentro de un **monolito web**. El frontend es un consumidor de la API REST y los servicios de infraestructura externos se mantienen como dependencias controladas del sistema.

## Fuente

- Repositorio DMGOTRAVEL: https://github.com/AiColleman/DMGOTRAVEL
