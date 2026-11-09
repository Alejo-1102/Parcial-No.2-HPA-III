# Parcial # 2 - Login de Usuario en Capas con C# y MySQL

📅 Fecha: [11/08/2026]

## 📋 Contenido del Repositorio

En este laboratorio desarrollé un sistema de inicio de sesión para Windows Forms usando programación en capas. El proyecto permite que un usuario entre al sistema con su usuario y contraseña, y una vez adentro puede registrar nuevos usuarios. Las contraseñas nunca se guardan en texto plano, se convierten con SHA-512 antes de llegar a la base de datos.

Separé el código en cuatro capas para que cada una tenga una sola responsabilidad:

- **Entidades:** las clases que representan los datos, en este caso el usuario.
- **Datos:** la conexión con MySQL y las llamadas a los procedimientos almacenados.
- **Negocio:** las reglas del sistema, como generar y verificar el hash de la contraseña.
- **Presentacion:** los formularios de Windows Forms junto con sus presentadores, siguiendo el patrón MVP.

También incluí una carpeta con el script de la base de datos, que crea las tablas, los procedimientos almacenados y un usuario de conexión con permisos mínimos.

## 🛠️ Tecnologías Utilizadas

<img src="https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white" alt="C#">
<img src="https://img.shields.io/badge/.NET_10-512BD4?style=for-the-badge&logo=dotnet&logoColor=white" alt=".NET 10">
<img src="https://img.shields.io/badge/Windows_Forms-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="Windows Forms">
<img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL">
<img src="https://img.shields.io/badge/Visual_Studio-5C2D91?style=for-the-badge&logo=visualstudio&logoColor=white" alt="Visual Studio">
<img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" alt="Git">

- **Lenguaje / Framework:** C# con .NET 10 y Windows Forms
- **Base de datos:** MySQL, con acceso mediante el paquete NuGet MySql.Data
- **Herramientas:** Visual Studio, MySQL Workbench, Git

## 🖥️ Capturas de Pantalla y Problemas

### Interfaz Principal

> 📸 **Aquí va la captura de la pantalla principal del sistema, con la sesión iniciada, la hora y la fecha.**

- **Problema 1, Inicio de sesión:** el formulario de login pide usuario y contraseña. La capa de negocio convierte lo que escribe el usuario a SHA-512 y lo compara con el hash que devuelve la base de datos. Si algo no coincide, solo muestra un aviso genérico para no dar pistas de qué campo falló.

> 📸 **Aquí va la captura del formulario de login.**

> 📸 **Aquí va la captura del mensaje de error con usuario o contraseña incorrectos.**

- **Problema 2, Pantalla principal:** al entrar correctamente se abre el formulario principal, que muestra quién inició sesión y un reloj con la fecha que se actualiza solo con un temporizador.

> 📸 **Aquí va la captura de la pantalla principal mostrando el usuario conectado.**

- **Problema 3, Registro de usuarios:** desde el botón de registrar se abre un formulario hijo. Valida que los campos no estén vacíos, que la contraseña tenga mínimo 6 caracteres y que la confirmación coincida. Si el usuario ya existe, el sistema avisa en lugar de romperse.

> 📸 **Aquí va la captura del formulario de registro.**

> 📸 **Aquí va la captura del mensaje de usuario registrado correctamente.**

> 📸 **Aquí va la captura del mensaje de que ese usuario ya existe.**

- **Problema 4, Base de datos y permisos:** la base de datos usa dos procedimientos almacenados, uno para obtener un usuario y otro para registrarlo. El sistema se conecta con un usuario que solo tiene permiso de ejecutar esos dos procedimientos, así que no puede leer ni modificar tablas directamente.

> 📸 **Aquí va la captura de la tabla usuarios en MySQL Workbench.**

> 📸 **Aquí va la captura del resultado de SHOW GRANTS para el usuario de conexión.**

## 📁 Estructura de Carpetas o Directorios

```plaintext
Laboratorio Capas/
├── BaseDatos/                  # Script SQL con tabla, procedimientos y usuario de conexion
│   └── script_bd.sql
├── Entidades/                  # Clases que representan los datos
│   └── E_Usuario.cs
├── Datos/                      # Acceso a MySQL
│   ├── D_Conexion.cs           # Cadena de conexion
│   └── D_Usuario.cs            # Llamadas a los procedimientos almacenados
├── Negocio/                    # Reglas del sistema
│   ├── N_Usuario.cs            # Login y registro
│   ├── HashService.cs          # Genera el hash SHA-512
│   ├── VerificarHashService.cs # Compara contraseña con el hash guardado
│   ├── IHashService.cs
│   └── IVerificarHashService.cs
├── Presentacion/               # Formularios y presentadores MVP
│   ├── FrmLogin.cs
│   ├── FrmPrincipal.cs
│   ├── FrmRegistro.cs
│   ├── LoginPresentador.cs
│   ├── PrincipalPresentador.cs
│   ├── RegistroPresentador.cs
│   ├── IVistaLogin.cs
│   ├── IVistaPrincipal.cs
│   ├── IVistaRegistro.cs
│   └── Program.cs              # Punto de entrada
├── Laboratorio Capas.slnx      # Solucion de Visual Studio
└── README.md                   # Documentación del proyecto
```

## 🚀 Instrucciones de Ejecución / Uso

### Requisitos previos

- Visual Studio con la carga de trabajo de escritorio de .NET
- SDK de .NET 10
- MySQL Server corriendo en localhost

### Pasos

1. Clonar el repositorio:

   ```bash
   git clone [URL del repositorio]
   ```

2. Abrir MySQL Workbench y ejecutar el archivo `BaseDatos/script_bd.sql`. Esto crea la base de datos `db_sistema_login`, la tabla, los procedimientos y el usuario de conexión.

> 📸 **Aquí va la captura de la ejecución del script en MySQL Workbench.**

3. Abrir la solución `Laboratorio Capas.slnx` en Visual Studio y esperar a que se restauren los paquetes NuGet. El único paquete externo es MySql.Data.

4. Revisar que la cadena de conexión en `Datos/D_Conexion.cs` coincida con tu MySQL local. Si cambiaste el usuario o la contraseña del script, hay que actualizarlos ahí también.

5. Poner `Presentacion` como proyecto de inicio y presionar F5 para ejecutar.

6. Ingresar con el usuario de prueba que crea el script:

   - **Usuario:** admin
   - **Contraseña:** 123456

Si aparece el mensaje de que no se pudo conectar con la base de datos, lo más probable es que MySQL no esté encendido o que el script no se haya ejecutado completo.

## 👤 Autor y Contexto

- **Nombre:** [Tu nombre]
- **Institución:** Universidad Tecnológica de Panamá, UTP
- **Fecha de Realización:** [DD/MM/AAAA]

## 📚 Referencias

- Guía del laboratorio, Lb_de_Capas.docx
- Video de Apoyo: [pega aquí el enlace del video]
- Paquete MySql.Data en NuGet: <https://www.nuget.org/packages/MySql.Data>
