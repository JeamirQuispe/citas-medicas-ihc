# Sistema Web de Gestión de Citas Médicas · Bienestar UNT

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat&logo=express&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![EJS](https://img.shields.io/badge/EJS-B4CA65?style=flat&logo=ejs&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

Aplicación web para gestionar las citas médicas del área de Bienestar Universitario de la Universidad Nacional de Trujillo. Desarrollada para el curso de Interacción Humano-Computador (IHC), con foco en usabilidad y accesibilidad.

```txt
Paciente reserva cita -> Enfermera registra triaje -> Médico atiende -> Consulta queda en historial
```

## Características

- **4 roles con acceso diferenciado:** paciente, enfermera, médico y administrativo.
- **Flujo clínico completo:** reserva de cita, triaje, atención médica (con borradores de consulta), recetas e historial.
- **Gestión administrativa:** alta y baja de usuarios, personal médico y de enfermería, y control de citas.
- **Seguridad:** contraseñas con `bcrypt`, sesiones almacenadas en MySQL, protección CSRF, limitación de intentos de inicio de sesión y control de acceso por rol.
- **Usabilidad y accesibilidad (IHC):** tamaño de texto ajustable, modo de alto contraste, validaciones en formularios, mensajes de retroalimentación y secciones de ayuda por rol.
- **Base de datos relacional:** 16 tablas (usuarios, citas, triajes, antecedentes, consultas, medicamentos, notificaciones) con scripts de esquema y datos de prueba.

## Tecnologías

| Capa | Tecnología |
|---|---|
| Backend | Node.js, Express 5 |
| Vistas | EJS con layouts, CSS y JavaScript del lado del cliente |
| Base de datos | MySQL (`mysql2`) |
| Seguridad | bcrypt, express-session, CSRF y rate limiting propios |
| Gestor de paquetes | pnpm |

## Estructura del proyecto

```txt
src/
  config/        conexión a MySQL y almacén de sesiones
  controllers/   lógica por rol (admin, médico, enfermera, paciente, perfil, auth)
  middlewares/   autenticación, roles, CSRF y rate limit
  routes/        rutas por rol
  services/      envío de correos (SMTP)
  views/         plantillas EJS por rol
  public/        estilos, scripts del cliente e imágenes
database/        esquema, datos de prueba y script de limpieza
```

## Equipo

Proyecto académico desarrollado en equipo en la Universidad Nacional de Trujillo:

- [Jeamir Quispe](https://github.com/JeamirQuispe)
- [Nelson Pineda](https://github.com/NelsonPA26) · repositorio original: [NelsonPA26/citas-medicas-ihc](https://github.com/NelsonPA26/citas-medicas-ihc)

Este repositorio conserva el historial de commits de la rama de trabajo `jeamir`.

## Requisitos

Antes de instalar, tener:

- Node.js LTS.
- pnpm.
- MySQL o XAMPP con MySQL activo.
- Navegador web.

Verificar versiones:

```bash
node -v
pnpm -v
```

Si no tienes pnpm:

```bash
npm install -g pnpm
```

## Instalacion

1. Abrir una terminal en la carpeta del proyecto.

2. Instalar dependencias:

```bash
pnpm install
```

3. Crear el archivo `.env` a partir de `.env.example`.

En Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Ejemplo de `.env` para XAMPP sin clave:

```env
PORT=3000
NODE_ENV=development
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=citas_medicas_ihc
SESSION_SECRET=reemplaza_esto_por_un_secreto_aleatorio_de_al_menos_32_caracteres
APP_URL=http://localhost:3000
```

## Base de datos

Los scripts utiles estan en `database/`:

- `schema.sql`: crea la base de datos y todas las tablas.
- `seed_pruebas_masivas_citas_medicas.sql`: carga usuarios y datos de prueba.
- `limpiar_bd_citas_medicas.sql`: borra datos de prueba sin eliminar las tablas.

Primera instalacion o reinicio completo:

```bash
mysql -u root < database/schema.sql
mysql -u root citas_medicas_ihc < database/seed_pruebas_masivas_citas_medicas.sql
```

Si MySQL pide clave:

```bash
mysql -u root -p < database/schema.sql
mysql -u root -p citas_medicas_ihc < database/seed_pruebas_masivas_citas_medicas.sql
```

Cuando la base ya existe y solo quieres volver a cargar datos:

```bash
mysql -u root citas_medicas_ihc < database/limpiar_bd_citas_medicas.sql
mysql -u root citas_medicas_ihc < database/seed_pruebas_masivas_citas_medicas.sql
```

Importante: el seed borra y reemplaza la data de prueba del sistema. No lo ejecutes sobre informacion real.

## Ejecutar el sistema

Modo desarrollo:

```bash
pnpm dev
```

Modo normal:

```bash
pnpm start
```

Abrir en el navegador:

```txt
http://localhost:3000
```

## Usuarios de prueba

Todos usan la contrasena:

```txt
UNT12345*
```

El inicio de sesion se realiza con el correo institucional.

Administrativos:

```txt
admin01@unitru.edu.pe
admin02@unitru.edu.pe
admin03@unitru.edu.pe
```

Medicos:

```txt
medico01@unitru.edu.pe
medico02@unitru.edu.pe
medico03@unitru.edu.pe
medico04@unitru.edu.pe
medico06@unitru.edu.pe
```

Enfermeras:

```txt
enfermera01@unitru.edu.pe
enfermera02@unitru.edu.pe
enfermera03@unitru.edu.pe
```

Pacientes:

```txt
paciente01@unitru.edu.pe
paciente02@unitru.edu.pe
...
paciente20@unitru.edu.pe
```

Tambien existen usuarios inactivos para probar validaciones: `medico05@unitru.edu.pe` y `enfermera04@unitru.edu.pe`.

## Datos que carga el seed

La poblacion masiva deja escenarios suficientes para probar los modulos:

- Cada paciente tiene citas en los estados `pendiente`, `triaje_registrado`, `en_consulta`, `completada` y `cancelada`.
- Las enfermeras activas tienen triajes registrados en varios estados para probar pendientes, activos e historial.
- Los medicos activos tienen citas listas para atender, consultas en borrador y consultas completadas para historial.
- Se cargan antecedentes, notificaciones, consentimiento de privacidad y catalogo de medicamentos para recetas.

Estados usados por las citas:

```txt
pendiente
triaje_registrado
en_consulta
completada
cancelada
```

## Archivos que no se deben entregar o subir

No incluir:

- `.env`, porque contiene configuracion local.
- `node_modules/`, porque se instala con `pnpm install`.
- `tmp/` o cache generado durante pruebas.

El archivo `.env.example` si debe quedarse, porque sirve como plantilla para configurar el proyecto.
