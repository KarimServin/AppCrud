# App CRUD Empleados

Sistema de gestión de empleados desarrollado con ASP.NET Core MVC que permite realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) sobre registros de empleados con interfaz web responsiva.

## Descripción

Esta aplicación web proporciona una solución completa para la gestión de información de empleados en una organización. Implementa el patrón arquitectónico MVC (Model-View-Controller) y utiliza Entity Framework Core como ORM para la persistencia de datos en SQL Server.

El sistema está diseñado siguiendo las mejores prácticas de desarrollo web con ASP.NET Core, incluyendo inyección de dependencias, operaciones asíncronas y un diseño de interfaz moderno y responsivo.

## Stack Tecnológico

### Backend
- **.NET 10** - Framework de desarrollo principal
- **ASP.NET Core MVC** - Framework web para el patrón Model-View-Controller
- **Entity Framework Core** - ORM (Object-Relational Mapping) para acceso a datos
- **SQL Server LocalDB** - Motor de base de datos para desarrollo local

### Frontend
- **Razor Pages** - Motor de plantillas del lado del servidor
- **Bootstrap 5** - Framework CSS para diseño responsivo
- **Bootstrap Icons** - Librería de iconografía
- **HTML5 / CSS3** - Tecnologías web estándar

### Herramientas de Desarrollo
- **Visual Studio 2022** - IDE principal
- **Git** - Control de versiones
- **NuGet** - Gestor de paquetes .NET

## Funcionalidades

### Operaciones CRUD Completas

**Listar Empleados**
- Visualización en tabla responsiva de todos los empleados registrados
- Indicadores visuales de estado (Activo/Inactivo)
- Formato de fechas localizado (dd/MM/yyyy)
- Botones de acción para editar y eliminar

**Crear Empleado**
- Formulario con validación de campos obligatorios
- Campos: Nombre Completo, Correo Electrónico, Fecha de Contrato, Estado Activo
- Validación de tipo de datos en el cliente y servidor
- Redirección automática a la lista después de crear

**Editar Empleado**
- Carga de datos existentes del empleado seleccionado
- Formulario prellenado con información actual
- Actualización atómica en base de datos
- Confirmación visual de cambios guardados

**Eliminar Empleado**
- Confirmación mediante diálogo JavaScript antes de eliminar
- Eliminación mediante petición POST segura
- Actualización automática de la lista después de eliminar

### Características Adicionales

- Interfaz de usuario responsiva compatible con dispositivos móviles, tablets y escritorio
- Operaciones asíncronas para no bloquear el servidor
- Mensajes informativos cuando no hay registros
- Navegación intuitiva entre vistas

## Requisitos del Sistema

### Software Requerido

- **Visual Studio 2022** (versión 17.0 o superior)
  - Workload: ASP.NET and web development
  - Componente: SQL Server Express LocalDB

- **.NET 10 SDK**
  - Descargar desde: https://dotnet.microsoft.com/download/dotnet/10.0

- **SQL Server LocalDB**
  - Incluido con Visual Studio 2022
  - Versión mínima: SQL Server 2019 Express LocalDB

### Requisitos Opcionales

- **Git** para control de versiones
- **Visual Studio Code** como editor alternativo
- **SQL Server Management Studio (SSMS)** para administración de base de datos

## Instalación y Configuración

### Paso 1: Clonar el Repositorio

```bash
git clone https://github.com/TU-USUARIO/app-crud-empleados.git
cd app-crud-empleados
```

### Paso 2: Configurar la Cadena de Conexión

Crear el archivo `AppCrud/appsettings.Development.json` con el siguiente contenido:

```json
{
  "ConnectionStrings": {
    "CadenaSQL": "Server=(localdb)\\MSSQLLocalDB;Database=DbCrud;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

**Nota de Seguridad:** Este archivo NO debe incluirse en el control de versiones y está listado en `.gitignore`.

### Paso 3: Restaurar Dependencias

```bash
dotnet restore
```

Este comando descargará todos los paquetes NuGet necesarios especificados en el archivo `.csproj`.

### Paso 4: Aplicar Migraciones de Base de Datos

```bash
dotnet ef database update
```

Si no tienes la herramienta Entity Framework Core CLI instalada:

```bash
dotnet tool install --global dotnet-ef
```

Este comando creará la base de datos `DbCrud` y aplicará el esquema definido en las migraciones.

### Paso 5: Ejecutar la Aplicación

**Opción A: Desde Visual Studio**
1. Abrir la solución `AppCrud.sln` en Visual Studio
2. Presionar `F5` o hacer clic en el botón "Start Debugging"
3. El navegador se abrirá automáticamente

**Opción B: Desde la Terminal**
```bash
cd AppCrud
dotnet run
```

La aplicación estará disponible en:
- HTTPS: `https://localhost:5001`
- HTTP: `http://localhost:5000`

Navegar a `/Empleado/Lista` para ver la lista de empleados.

## Estructura del Proyecto

```
AppCrud/
│
├── Controllers/
│   └── EmpleadoController.cs          # Controlador principal con lógica de negocio
│
├── Models/
│   └── Empleado.cs                    # Entidad del modelo de datos
│
├── Views/
│   ├── Empleado/
│   │   ├── Lista.cshtml               # Vista de listado de empleados
│   │   ├── Nuevo.cshtml               # Vista de formulario para crear
│   │   └── Editar.cshtml              # Vista de formulario para editar
│   └── Shared/
│       └── _Layout.cshtml             # Plantilla principal de layout
│
├── Data/
│   └── AppDBContext.cs                # Contexto de Entity Framework Core
│
├── Migrations/                         # Migraciones de base de datos
│
├── wwwroot/                           # Archivos estáticos públicos
│   ├── css/                           # Hojas de estilo personalizadas
│   ├── js/                            # Scripts JavaScript
│   └── lib/                           # Librerías de terceros
│
├── appsettings.json                   # Configuración general de la aplicación
├── appsettings.Development.json       # Configuración de desarrollo (no versionado)
├── Program.cs                         # Punto de entrada y configuración de servicios
└── AppCrud.csproj                     # Archivo de proyecto .NET
```

## Modelo de Datos

### Entidad: Empleado

La aplicación maneja una única entidad `Empleado` con la siguiente estructura:

| Campo | Tipo de Dato | Restricciones | Descripción |
|-------|--------------|---------------|-------------|
| `IdEmpleado` | `int` | Primary Key, Identity | Identificador único autoincremental |
| `NombreCompleto` | `string` | NOT NULL, MaxLength(100) | Nombre completo del empleado |
| `Correo` | `string` | NOT NULL, MaxLength(100) | Dirección de correo electrónico |
| `FechaContrato` | `DateOnly` | NOT NULL | Fecha de inicio del contrato laboral |
| `Activo` | `bool` | NOT NULL, Default(true) | Estado actual del empleado en la organización |

### Esquema de Base de Datos

```sql
CREATE TABLE Empleados (
    IdEmpleado INT PRIMARY KEY IDENTITY(1,1),
    NombreCompleto NVARCHAR(100) NOT NULL,
    Correo NVARCHAR(100) NOT NULL,
    FechaContrato DATE NOT NULL,
    Activo BIT NOT NULL DEFAULT 1
);
```

## Arquitectura de la Aplicación

### Patrón MVC

La aplicación sigue el patrón arquitectónico Model-View-Controller:

**Model (Modelo)**
- Define la estructura de datos (`Empleado.cs`)
- Contexto de base de datos (`AppDBContext.cs`)
- Lógica de acceso a datos con Entity Framework Core

**View (Vista)**
- Plantillas Razor (.cshtml) para renderizar HTML
- Uso de Tag Helpers para formularios y navegación
- Bootstrap para estilo y diseño responsivo

**Controller (Controlador)**
- `EmpleadoController.cs` gestiona todas las peticiones HTTP
- Métodos para cada operación CRUD
- Comunicación entre modelo y vista

### Flujo de Datos

```
[Cliente] → HTTP Request → [Controller] → [DbContext] → [SQL Server]
                                ↓                            ↓
[Cliente] ← HTML Response ← [View] ← [Model/Data] ← [Database]
```

## Endpoints de la API

### Rutas y Controladores

| Ruta | Método HTTP | Acción del Controlador | Descripción |
|------|-------------|------------------------|-------------|
| `/Empleado/Lista` | GET | `Lista()` | Retorna vista con lista de todos los empleados |
| `/Empleado/Nuevo` | GET | `Nuevo()` | Retorna vista con formulario vacío para nuevo empleado |
| `/Empleado/Nuevo` | POST | `Nuevo(Empleado empleado)` | Procesa datos del formulario y crea nuevo empleado |
| `/Empleado/Editar/{id}` | GET | `Editar(int id)` | Retorna vista con formulario prellenado del empleado especificado |
| `/Empleado/Editar` | POST | `Editar(Empleado empleado)` | Procesa datos del formulario y actualiza empleado existente |
| `/Empleado/Eliminar/{id}` | POST | `Eliminar(int id)` | Elimina el empleado especificado y redirige a la lista |

### Ejemplo de Implementación del Controlador

```csharp
[HttpGet]
public async Task<IActionResult> Lista()
{
    List<Empleado> lista = await _appDBContext.Empleados.ToListAsync();
    return View(lista);
}

[HttpPost]
public async Task<IActionResult> Nuevo(Empleado empleado)
{
    await _appDBContext.Empleados.AddAsync(empleado);
    await _appDBContext.SaveChangesAsync();
    return RedirectToAction(nameof(Lista));
}
```

## Configuración de Base de Datos

### Cadena de Conexión

La aplicación utiliza SQL Server LocalDB, configurado mediante cadena de conexión en `appsettings.Development.json`:

```json
{
  "ConnectionStrings": {
    "CadenaSQL": "Server=(localdb)\\MSSQLLocalDB;Database=DbCrud;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

### Entity Framework Core

**DbContext Configuration:**

```csharp
public class AppDBContext : DbContext
{
    public AppDBContext(DbContextOptions<AppDBContext> options) : base(options)
    {
    }

    public DbSet<Empleado> Empleados { get; set; }
}
```

### Comandos de Migración

**Crear nueva migración:**
```bash
dotnet ef migrations add NombreDeLaMigracion
```

**Aplicar migraciones pendientes:**
```bash
dotnet ef database update
```

**Revertir a migración específica:**
```bash
dotnet ef database update NombreDeMigracion
```

**Eliminar última migración (si no se aplicó):**
```bash
dotnet ef migrations remove
```

**Ver estado de migraciones:**
```bash
dotnet ef migrations list
```

## Seguridad y Mejores Prácticas

### Archivos Sensibles

Los siguientes archivos y carpetas NO deben incluirse en el control de versiones:

**Configuración:**
- `appsettings.Development.json` - Contiene cadenas de conexión con credenciales
- `appsettings.Production.json` - Configuración de producción
- `*.user` - Configuración específica del usuario de Visual Studio

**Base de Datos:**
- `*.mdf` - Archivos de base de datos SQL Server
- `*.ldf` - Archivos de log de SQL Server
- `*.db` - Archivos SQLite
- `*.sqlite` / `*.sqlite3` - Variantes de SQLite

**Build Artifacts:**
- `bin/` - Archivos binarios compilados
- `obj/` - Archivos objeto intermedios
- `.vs/` - Configuración de Visual Studio

### Archivo .gitignore

El proyecto incluye un archivo `.gitignore` completo que protege información sensible:

```gitignore
## Visual Studio artifacts
.vs/
*.user

## Build results
[Bb]in/
[Oo]bj/

## Database files
*.mdf
*.ldf
*.db

## Connection Strings
appsettings.Development.json
appsettings.Production.json
```

### Recomendaciones de Seguridad

1. **Nunca** commitear cadenas de conexión con credenciales reales
2. Utilizar variables de entorno en producción
3. Implementar Azure Key Vault para secretos en la nube
4. Habilitar HTTPS en producción
5. Implementar autenticación y autorización para proteger endpoints
6. Validar entrada de usuario tanto en cliente como servidor
7. Utilizar SQL parameterizado (Entity Framework lo hace automáticamente)

## Desarrollo y Contribución

### Flujo de Trabajo con Git

**Configuración inicial:**
```bash
git init
git add .
git commit -m "Initial commit: Aplicación CRUD de empleados"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/app-crud-empleados.git
git push -u origin main
```

**Crear rama de desarrollo:**
```bash
git checkout -b develop
git push -u origin develop
```

**Trabajar en una nueva funcionalidad:**
```bash
git checkout develop
git checkout -b feature/nombre-funcionalidad
# Realizar cambios
git add .
git commit -m "feat: descripción de la funcionalidad"
git push origin feature/nombre-funcionalidad
# Crear Pull Request en GitHub hacia develop
```

### Estándares de Código

- Utilizar convenciones de nomenclatura de C# (PascalCase para clases y métodos, camelCase para variables)
- Escribir métodos asincrónicos con sufijo `Async`
- Documentar métodos públicos con comentarios XML
- Mantener controladores delgados, mover lógica compleja a servicios
- Seguir principios SOLID

## Roadmap y Mejoras Futuras

### Corto Plazo

- Implementar paginación en la lista de empleados (10-20 registros por página)
- Agregar funcionalidad de búsqueda por nombre o correo
- Implementar filtros por estado (Activo/Inactivo)
- Añadir ordenamiento de columnas en la tabla

### Mediano Plazo

- Agregar validaciones completas del lado del servidor usando Data Annotations
- Implementar ViewModels/DTOs para desacoplar vistas de entidades de base de datos
- Añadir manejo de errores centralizado con páginas de error personalizadas
- Implementar logging con Serilog
- Agregar pruebas unitarias con xUnit
- Implementar pruebas de integración

### Largo Plazo

- Implementar autenticación con ASP.NET Core Identity
- Agregar autorización basada en roles (Admin, Usuario)
- Implementar patrón Repository y Unit of Work
- Agregar capa de servicios para lógica de negocio
- Crear API RESTful para consumo de clientes externos
- Implementar versionado de API
- Dockerizar la aplicación
- Configurar CI/CD con GitHub Actions o Azure DevOps
- Migrar a Azure SQL Database para producción

## Solución de Problemas

### Error: No se puede conectar a LocalDB

**Síntoma:** `Cannot connect to (localdb)\MSSQLLocalDB`

**Solución:**
```bash
# Verificar instancias de LocalDB
sqllocaldb info

# Iniciar instancia si está detenida
sqllocaldb start MSSQLLocalDB

# Recrear instancia si está corrupta
sqllocaldb delete MSSQLLocalDB
sqllocaldb create MSSQLLocalDB
```

### Error: Las migraciones no se aplican

**Síntoma:** `A migration for DbContext has already been applied`

**Solución:**
```bash
# Ver migraciones aplicadas
dotnet ef migrations list

# Eliminar base de datos y recrear
dotnet ef database drop
dotnet ef database update
```

### Error: Puertos en uso

**Síntoma:** `Address already in use`

**Solución:**
Modificar `launchSettings.json` para usar puertos diferentes:
```json
{
  "applicationUrl": "https://localhost:5051;http://localhost:5050"
}
```

## Recursos de Aprendizaje

### Documentación Oficial

- [Documentación de ASP.NET Core MVC](https://docs.microsoft.com/aspnet/core/mvc)
- [Guía de Entity Framework Core](https://docs.microsoft.com/ef/core)
- [Tutorial de Razor Pages](https://docs.microsoft.com/aspnet/core/razor-pages)
- [Documentación de Bootstrap 5](https://getbootstrap.com/docs/5.0)

### Tutoriales Recomendados

- [ASP.NET Core MVC Tutorial - Microsoft Learn](https://docs.microsoft.com/learn/paths/aspnet-core-web-app/)
- [Entity Framework Core Tutorial](https://www.entityframeworktutorial.net/efcore/entity-framework-core.aspx)
- [Patrones de Diseño en C#](https://refactoring.guru/design-patterns/csharp)

### Comunidad y Soporte

- [Stack Overflow - ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)
- [GitHub - ASP.NET Core](https://github.com/dotnet/aspnetcore)
- [.NET Community](https://dotnet.microsoft.com/platform/community)

## Licencia

Este proyecto es de código abierto y está disponible bajo la Licencia MIT.

## Autor

**Tu Nombre**
- GitHub: [@TU-USUARIO](https://github.com/TU-USUARIO)
- LinkedIn: [Tu LinkedIn](https://linkedin.com/in/tu-perfil)
- Email: tu.email@ejemplo.com

## Agradecimientos

- Comunidad de ASP.NET Core por las herramientas y frameworks
- Bootstrap Team por el framework CSS
- Entity Framework Core Team por el ORM

---

**Última actualización:** Enero 2025  
**Versión:** 1.0.0
