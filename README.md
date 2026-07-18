# Portal Municipalidad de Escazú

Solución web orientada a la atención ciudadana para un gobierno local. Este portal digitaliza la interacción entre los habitantes del municipio y la administración, facilitando consultas, gestión de trámites cívicos y la comunicación directa mediante interfaces claras y accesibles.

---

## Stack Tecnológico

**Backend & Servidor**
- **Entorno:** Node.js.
- **Framework REST:** Express.js.
- **Gestión de Procesos:** Nodemon (para desarrollo).

**Frontend (Cliente)**
- **Arquitectura UI:** HTML5 semántico y CSS3 puro (Grid/Flexbox y Bento UI).
- **Lógica de Interfaz:** Vanilla JS con ES6 Modules (`type="module"`).
- **Consumo de API:** Fetch API nativo.
- **Notificaciones/UX:** SweetAlert2.

**Persistencia de Datos (Mock)**
- **Data Store local:** JSON (Lectura y escritura).

---

## Funcionalidades Principales y Flujos de Usuario

El ecosistema digital está segmentado mediante un sistema de Control de Acceso Basado en Roles (RBAC), dividiendo la experiencia entre ciudadanos y administradores gubernamentales:

### Módulo Ciudadano (`dashboard-ciudadano`)
- **Pasarela de Servicios Públicos:** Módulo dedicado (`serviciosPublicos.js`) que digitaliza y facilita el pago de impuestos y servicios municipales, mejorando la recaudación y la experiencia del usuario (UX) mediante flujos guiados.
- **Veeduría de Proyectos Municipales:** Panel de transparencia (`Proyectos.js`) donde los ciudadanos pueden visualizar el avance, presupuesto y detalles de las obras de infraestructura del municipio.
- **Gestión de Peticiones:** Interfaz asíncrona para reportar incidentes o solicitar trámites cívicos, impulsada por notificaciones no bloqueantes (SweetAlert2).

### Módulo Administrativo (`dashboard-admin`)
- **Sistema Integrado de Recursos Humanos:** Un submódulo completo de gestión interna (`Empleados.js` y `planilla.js`) que permite a la municipalidad administrar el alta/baja de funcionarios, estructurar departamentos y calcular/gestionar la nómina (planilla) de forma centralizada.
- **Auditoría y Gestión de Reportes:** Panel de control central (`gestionarReportes.js`) para que los funcionarios procesen, categoricen y den resolución a las peticiones ingresadas por los ciudadanos.

---

## Valor de Negocio y Logros Técnicos

Este proyecto (GovTech) destaca por aplicar conceptos fundamentales y robustos de desarrollo *Full-Stack* bajo una arquitectura limpia y altamente escalable:

- **Ingeniería de Frontend Desacoplado (Vanilla JS Modular):** A diferencia de las tradicionales "páginas espagueti", el cliente está estrictamente modularizado por dominio de negocio (ej. `planilla.js`, `serviciosPublicos.js`, `gestionarReportes.js`). Esta separación de responsabilidades (SoC) facilita la mantenibilidad y previene colisiones en entornos sin *frameworks*.
- **Control de Acceso Basado en Roles (RBAC):** Implementación de una capa de seguridad y enrutamiento en el cliente que valida el estado de la sesión (`rol: "admin"` vs `"ciudadano"`), aislando el panel de auditoría interno de la vista pública de trámites.
- **Arquitectura Monolítica Ligera:** Uso de **Express.js** para orquestar tanto la entrega de *assets* estáticos como la gestión de las peticiones en un solo proceso de Node.js. Esto demuestra un profundo entendimiento de los ciclos de vida HTTP, *middlewares* y el modelo asíncrono de V8.
- **Motor de Persistencia Basado en Archivos (Mock NoSQL):** Emplear lectura/escritura estructurada contra un `db.json` (aprox. 17KB de relaciones complejas) es una decisión arquitectónica sumamente inteligente para un MVP. Permite validar entidades (Usuarios, Proyectos, Planillas) sin cuellos de botella de infraestructura, dejando las APIs listas para un "Drop-in Replacement" futuro con MongoDB o PostgreSQL.
- **Estrategia de Inclusión Digital (Accesibilidad & Performance):** Al tratarse de software gubernamental, forzar *frameworks* pesados habría excluido a ciudadanos con dispositivos de gama baja. Renderizar HTML puro y gestionar el estado del DOM mediante *Vanilla JS* garantiza métricas excepcionales de *Time to Interactive (TTI)* y compatibilidad universal.

---

## Arquitectura Preparada para Producción (Production-Ready)

Aunque el proyecto se diseñó como una solución de ejecución local, su código es extremadamente portable y está listo para ser alojado en infraestructuras *Cloud* ligeras:
- **PaaS Deployment:** Preparada para ser desplegada en plataformas como **Render**, **Railway**, o **Heroku** directamente desde GitHub. El comando `node server.js` basta para poner en marcha tanto la API como el Frontend simultáneamente en la nube.
- **Escalabilidad:** El aislamiento del puerto mediante variables de entorno (`process.env.PORT`) asegura que el proyecto pueda ser ejecutado por balanceadores de carga sin necesidad de modificar el código fuente.

---

## Instalación y Ejecución Local (Guía Paso a Paso)

<details>
<summary><b>Haz clic aquí para ver la Guía de Instalación y Ejecución Local</b></summary>

### 1. Prerrequisitos
- [Node.js](https://nodejs.org/) (Versión 18+).
- Git.

### 2. Clonar el Repositorio
```bash
git clone https://github.com/asporras7-dev/Proyecto_Municipalidad_Escazu.git
cd Proyecto_Municipalidad_Escazu
```

### 3. Instalar Dependencias
```bash
npm install
```

### 4. Ejecutar el Servidor
Para entornos de desarrollo (con recarga automática mediante `nodemon`):
```bash
npm run dev
```

Para emular el entorno de producción:
```bash
npm start
```

El portal ciudadano estará disponible en `http://localhost:3000` (o el puerto configurado en el servidor).

### 5. Credenciales de Prueba (Testing)
Para probar los flujos de autorización y visualizar los módulos administrativos, utiliza el siguiente usuario precargado en la base de datos local:
- **Rol Administrador:** Correo: `juan@correo.com` | Clave: `1234`
- **Rol Ciudadano:** Puedes registrarte en el portal para crear tu propia cuenta.

</details>

---

## Estructura del Proyecto

<details>
<summary><b>Haz clic aquí para desplegar la Arquitectura de Carpetas</b></summary>

```text
Proyecto_Municipalidad_Escazu/
├── public/               # Frontend: Archivos estáticos servidos por Express
│   ├── pages/            # Vistas HTML modulares (home, dashboards, planillas, etc.)
│   ├── css/              # Hojas de estilo Vanilla CSS separadas por componente
│   ├── js/               # Controladores Vanilla JS utilizando ES6 Modules
│   ├── img/              # Recursos gráficos y multimedia corporativa
│   └── services/         # Servicios aislados para peticiones Fetch/API
├── server.js             # Orquestador Node.js / Express API
├── db.json               # Base de datos simulada (Relaciones de Usuarios, Proyectos, etc.)
├── package.json          # Dependencias del proyecto (Express, Nodemon)
├── package-lock.json     # Árbol de dependencias bloqueado
└── .gitignore            # Archivos excluidos del control de versiones
```
</details>

