
# Sistema Web de Votaciones · Festival de la Leyenda Vallenata 2027

**Desarrollo Web · Fundación Universitaria del Área Andina · Entrega 3: módulo de votaciones, validaciones y API GET.**

Aplicación web desarrollada con **Django y MySQL**, siguiendo el patrón arquitectónico MVT. El proyecto permite gestionar ediciones, categorías, participantes y votaciones del Festival de la Leyenda Vallenata.

Esta versión integra el proceso completo de votación, validaciones en el servidor, consulta de participación, API GET en formato JSON, visualización de resultados publicados y mejoras en la interfaz de usuario.

> **Regla central:** cada usuario puede elegir una sola opción por categoría y edición. Puede votar en otras categorías, pero no emitir un segundo voto en la misma categoría, aunque esta tenga varios subeventos. Esta regla se garantiza mediante una restricción `UNIQUE` en MySQL.

---

## 1. Funcionalidades del sistema

### Gestión del festival
- Gestión de ediciones del Festival.
- Administración de categorías.
- Administración de votaciones y participantes.
- Activación y desactivación de categorías y opciones.
- Gestión administrativa mediante Django Admin.
- Registro de auditoría de las operaciones administrativas.

### Gestión de usuarios
- Registro de nuevos usuarios.
- Inicio y cierre de sesión.
- Autenticación mediante correo electrónico.
- Control de acceso según el perfil del usuario.
- Protección de las operaciones de votación.

### Módulo de votaciones
- Consulta de categorías y opciones disponibles.
- Selección y confirmación del voto.
- Registro de votos en MySQL.
- Validación de votaciones abiertas y fechas de participación.
- Restricción de un voto por usuario, categoría y edición.
- Prevención de votos duplicados incluso ante solicitudes simultáneas.
- Historial de participación mediante la sección **Mis votos**.
- Consulta administrativa de los votos registrados.

### Resultados
- Visualización de resultados de votaciones publicadas.
- Conteo de votos por participante.
- Cálculo de porcentajes de participación.
- Consulta de resultados asociados a las categorías y ediciones.
- Restricción de visualización según el estado de publicación.

### Interfaz
- Diseño responsive adaptable a diferentes tamaños de pantalla.
- Interfaz desarrollada con Bootstrap 5 local.
- Navegación mediante menús y enlaces.
- Visualización de categorías y cantidad de votaciones.
- Indicadores de votaciones abiertas.
- Plantillas HTML organizadas por funcionalidad.

### API
- Endpoint GET para consultar categorías.
- Respuesta en formato JSON.
- Consulta de información desde MySQL.
- Filtro opcional por edición.

---

## 2. Tecnologías y versiones

| Componente | Versión o tecnología |
|---|---|
| Lenguaje | Python 3.13 |
| Framework | Django 5.2 |
| Base de datos | MySQL 8.x |
| Conector | mysqlclient >= 2.2 |
| Variables de entorno | python-decouple >= 3.8 |
| Frontend | HTML5, CSS3 y JavaScript |
| Framework visual | Bootstrap 5 |
| Arquitectura | MVT |
| Control de versiones | Git y GitHub |

Las dependencias se encuentran en el archivo `requirements.txt`.

---

## 3. Requisitos previos

Antes de ejecutar el proyecto se necesita:

- Python 3.10 o superior.
- MySQL Server 8.x.
- Git.
- PowerShell en Windows.
- Un navegador web actualizado.

---

## 4. Instalación y ejecución en Windows

### Paso 1. Clonar el repositorio

```powershell
git clone https://github.com/ericaurte3-cpu/festival_votaciones_entrega.git
cd festival_votaciones_entrega
```

### Paso 2. Crear el entorno virtual

```powershell
python -m venv venv
```

Activar el entorno:

```powershell
venv\Scripts\Activate.ps1
```

Si PowerShell bloquea la activación:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Luego vuelve a ejecutar:

```powershell
venv\Scripts\Activate.ps1
```

### Paso 3. Instalar dependencias

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Paso 4. Crear la base de datos MySQL

Ingresar al cliente MySQL como administrador y ejecutar:

```sql
CREATE DATABASE festival_votaciones
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

CREATE USER 'festival_user'@'localhost'
IDENTIFIED BY 'TU_CONTRASEÑA';

GRANT ALL PRIVILEGES ON festival_votaciones.*
TO 'festival_user'@'localhost';

GRANT ALL PRIVILEGES ON test_festival_votaciones.*
TO 'festival_user'@'localhost';

FLUSH PRIVILEGES;
```

Reemplazar `TU_CONTRASEÑA` por una contraseña propia.

### Paso 5. Configurar variables de entorno

Crear el archivo `.env` a partir de la plantilla:

```powershell
copy .env.example .env
```

Abrir el archivo:

```powershell
notepad .env
```

Configurar los valores correspondientes:

```env
DJANGO_SECRET_KEY=coloca_una_clave_secreta_propia
DJANGO_DEBUG=True
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1

DB_NAME=festival_votaciones
DB_USER=festival_user
DB_PASSWORD=tu_contraseña_mysql
DB_HOST=127.0.0.1
DB_PORT=3306
```

**Importante:** el archivo `.env` contiene información privada y no debe publicarse en GitHub. El archivo `.env.example` funciona como plantilla de configuración.

### Paso 6. Aplicar migraciones

```powershell
python manage.py migrate
```

Este comando crea las tablas necesarias en MySQL.

### Paso 7. Cargar datos de prueba

```powershell
python manage.py cargar_datos_prueba --votantes 3 --password "UnaClaveDePrueba-2027"
```

Este comando permite cargar datos ficticios y usuarios de prueba.

También se puede crear un administrador:

```powershell
python manage.py createsuperuser
```

### Paso 8. Ejecutar el servidor

```powershell
python manage.py runserver
```

Abrir en el navegador:

- Sitio principal: http://127.0.0.1:8000/
- Administración: http://127.0.0.1:8000/admin/
- API de categorías: http://127.0.0.1:8000/api/categorias/

---

## 5. Usuarios de prueba

El comando de carga de datos permite crear tres usuarios de prueba:

| Usuario | Correo |
|---|---|
| Votante 1 | votante1@example.com |
| Votante 2 | votante2@example.com |
| Votante 3 | votante3@example.com |

La contraseña será la indicada mediante el parámetro `--password`.

Si se omite este parámetro, el comando genera una contraseña aleatoria y la muestra una sola vez.

---

## 6. Cómo probar el módulo de votaciones

1. Iniciar sesión con uno de los usuarios de prueba.
2. Ingresar a una categoría con votación abierta.
3. Seleccionar un participante.
4. Pulsar **Continuar y confirmar voto**.
5. Revisar el resumen de la selección.
6. Confirmar el voto.
7. Intentar votar nuevamente en la misma categoría para comprobar la restricción de duplicados.
8. Ingresar a otra categoría y comprobar que sí se permite votar.
9. Consultar la sección **Mis votos**.
10. Ingresar como administrador para consultar los votos registrados.

---

## 7. API GET

El proyecto dispone del endpoint:

```text
GET /api/categorias/
```

Este servicio devuelve información en formato JSON sobre las categorías públicas, sus ediciones, votaciones y opciones activas.

La API:

- No requiere iniciar sesión.
- No expone información personal de los usuarios.
- No expone los votos individuales.
- Permite filtrar por edición mediante el parámetro `edicion`.

Ejemplo:

```text
http://127.0.0.1:8000/api/categorias/?edicion=1
```

Solo admite solicitudes GET. Otros métodos reciben una respuesta HTTP 405.

---

## 8. Diseño y seguridad de las votaciones

El sistema implementa diferentes mecanismos para garantizar la integridad de los votos.

### Restricción de unicidad

Se utiliza una restricción `UniqueConstraint` sobre usuario, categoría y edición.

Esto evita que un usuario registre más de un voto en una misma categoría y edición.

### Validaciones en el servidor

Antes de registrar un voto se comprueba:

- Que el usuario haya iniciado sesión.
- Que la votación esté abierta.
- Que la fecha permita participar.
- Que la categoría y la edición estén activas.
- Que la opción seleccionada pertenezca a la votación.
- Que el usuario no haya votado previamente en esa categoría y edición.

### Control de concurrencia

Se utiliza `transaction.atomic()` y manejo de `IntegrityError` para controlar solicitudes simultáneas y evitar registros duplicados.

### Protección de datos

- Protección CSRF en formularios.
- Contraseñas almacenadas mediante hash.
- Variables sensibles configuradas mediante `.env`.
- Consultas a la base de datos mediante el ORM de Django.
- Protección de registros históricos mediante claves foráneas con `PROTECT`.

---

## 9. Resultados de votaciones

El sistema permite consultar resultados de votaciones publicadas.

La visualización contempla:

- Participantes y sus respectivos votos.
- Total de votos registrados.
- Porcentaje de participación por participante.
- Información relacionada con la categoría y edición.

Los resultados están sujetos al estado de publicación de cada votación.

---

## 10. Estructura del proyecto

```text
festival_votaciones_entrega/
│
├── manage.py
├── requirements.txt
├── .env.example
├── .gitignore
│
├── config/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── festival/
│   ├── models.py
│   ├── views.py
│   ├── servicios.py
│   ├── forms.py
│   ├── api.py
│   ├── admin.py
│   ├── urls.py
│   ├── tests_votos.py
│   ├── migrations/
│   └── management/
│
├── cuentas/
│
├── templates/
│   ├── base.html
│   ├── festival/
│   │   ├── inicio.html
│   │   ├── edicion.html
│   │   ├── categoria.html
│   │   ├── votacion.html
│   │   └── resultados.html
│   └── registration/
│
└── static/
    ├── css/
    │   └── estilos.css
    ├── js/
    └── vendor/
        └── bootstrap/
```

---

## 11. Pruebas del proyecto

Para ejecutar todas las pruebas:

```powershell
python manage.py test
```

Para ejecutar las pruebas del módulo de votaciones:

```powershell
python manage.py test festival.tests_votos -v 2
```

El proyecto cuenta con pruebas para verificar:

| Caso | Descripción |
|---|---|
| CP-01 | Registro de un voto válido |
| CP-02 | Rechazo de votos duplicados |
| CP-03 | Votación en otra categoría |
| CP-04 | Autenticación obligatoria |
| CP-05 | Validación de votaciones no habilitadas |
| CP-06 | Validación de opciones |
| CP-07 | Consulta de Mis votos |
| CP-08 | Consulta administrativa |
| CP-09 | Control de concurrencia |
| CP-10 | Respuesta de la API GET |
| CSRF | Protección contra solicitudes sin token |

Las pruebas requieren que MySQL tenga permisos para crear y eliminar la base de datos de pruebas.

---

## 12. Rutas principales

| Ruta | Funcionalidad |
|---|---|
| `/` | Página principal |
| `/ediciones/` | Consulta de ediciones |
| `/ediciones/<id>/` | Detalle de edición |
| `/categorias/<id>/` | Categorías y votaciones |
| `/votaciones/<id>/` | Opciones de votación |
| `/votaciones/<id>/votar/` | Proceso de registro del voto |
| `/mis-votos/` | Historial de participación |
| `/api/categorias/` | API GET |
| `/admin/` | Administración de Django |
| `/cuentas/registro/` | Registro de usuarios |
| `/cuentas/ingresar/` | Inicio de sesión |
| `/cuentas/salir/` | Cierre de sesión |

---

## 13. Perfiles y permisos

El sistema contempla usuarios y personal administrativo.

**Usuario votante:**
- Consultar ediciones y categorías.
- Consultar opciones disponibles.
- Registrar votos.
- Consultar su historial de participación.

**Administrador:**
- Gestionar ediciones.
- Gestionar categorías.
- Gestionar votaciones.
- Gestionar opciones y participantes.
- Consultar los votos registrados.
- Administrar la información del festival.

La administración se realiza desde Django Admin.

---

## 14. Seguridad y recomendaciones para producción

- Las contraseñas de MySQL y las claves secretas se gestionan mediante variables de entorno.
- El archivo `.env` está excluido del control de versiones.
- Las contraseñas de los usuarios se almacenan mediante mecanismos de hash de Django.
- Los formularios cuentan con protección CSRF.
- La emisión de votos requiere autenticación.
- Las reglas de negocio se validan en el servidor.
- Los datos de prueba son ficticios y no representan participantes reales.

Antes de desplegar en producción:

1. Configurar `DJANGO_DEBUG=False`.
2. Establecer una `DJANGO_SECRET_KEY` propia y segura.
3. Definir correctamente `DJANGO_ALLOWED_HOSTS`.
4. Configurar HTTPS.
5. Configurar `DJANGO_CSRF_TRUSTED_ORIGINS` cuando corresponda.
6. Ejecutar:

```powershell
python manage.py check --deploy
```

---

## 15. Limitaciones conocidas y mejoras futuras

- Implementar verificación de identidad del votante mediante correo confirmado y documento, según los requisitos institucionales.
- Incorporar limitación de intentos (rate limiting) en el inicio de sesión y en el endpoint de votación.
- Evaluar la implementación de una API de votación mediante POST si se requiere en futuras versiones.
- Realizar pruebas de despliegue en un servidor de producción.

---

## 16. Autoría académica

**Proyecto:** Sistema Web de Votaciones – Festival de la Leyenda Vallenata 2027.

**Institución:** Fundación Universitaria del Área Andina.

**Asignatura:** Desarrollo Web.

**Entrega:** Entrega 3 – Módulo de votaciones, validaciones y API GET.

**Tecnologías principales:** Python, Django, MySQL, HTML, CSS, JavaScript y Bootstrap.

---

**Repositorio de entrega:**  
https://github.com/ericaurte3-cpu/festival_votaciones_entrega

**Repositorio de trabajo original:**  
https://github.com/ericaurte3-cpu/festival_votaciones
