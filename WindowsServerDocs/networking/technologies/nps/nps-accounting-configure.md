---
title: Configurar las cuentas de servidor de directivas de red
description: Este tema proporciona información sobre el archivo de texto y el registro de servidor de directivas de red en Windows Server 2016 de SQL Server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69341e30d90ee1be29c40d835a4f71fe433c11dc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policy-server-accounting"></a>Configurar las cuentas de servidor de directivas de red

Existen tres tipos de registro de servidor de directivas de red \(NPS\):

- **Registro de eventos**. Se usa principalmente para la auditoría y solución de problemas de intentos de conexión. Puede configurar mediante la obtención de las propiedades del servidor NPS en la consola NPS de registro de eventos NPS.

- **Registro de autenticación de usuario y las solicitudes de cuentas en un archivo local**. Se usa principalmente para fines de análisis y la facturación de la conexión. También resulta útil como una herramienta de investigación de seguridad porque proporciona un método de seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configurar cuentas.

- **Registro de autenticación de usuario y las solicitudes de cuentas en una base de datos compatible con Microsoft SQL Server XML**. Se usa para permitir que a varios servidores que ejecuten NPS tengan un origen de datos. También proporciona las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server mediante el asistente Configurar cuentas.

## <a name="use-the-accounting-configuration-wizard"></a>Usar al Asistente para configurar cuentas

Mediante el Asistente de configuración de cuentas, puedes configurar los siguientes valores de cuatro cuentas:

- **Sólo registro SQL**. Con esta opción, puedes configurar un vínculo de datos a un servidor SQL Server que permite a NPS conectarse y enviar datos al servidor SQL. Además, el Asistente para configurar la base de datos de SQL Server para garantizar que la base de datos es compatible con el registro del servidor NPS SQL.
- **Registro de solo texto**. Con esta opción, puede configurar NPS para registrar los datos de cuentas en un archivo de texto.
- **Registro en paralelo**. Con esta opción, puedes configurar el enlace de datos de SQL Server y la base de datos. También puedes configurar el registro de archivos de texto para que NPS simultáneamente registra en el archivo de texto y la base de datos de SQL Server. 
- **Registro de SQL con copia de seguridad**. Con esta opción, puedes configurar el enlace de datos de SQL Server y la base de datos. Además, puede configurar el registro de archivo de texto que usa NPS si se produce un error en el registro de SQL Server.

Además de estas opciones de configuración, registro de SQL Server y registro de texto permiten especificar si NPS continúa procesar las solicitudes de conexión si se produce un error en el registro. Puedes especificar esto en la **sección de la acción de errores de registro** en Propiedades de registro de archivos local, en las propiedades de registro del servidor SQL y mientras se ejecuta el Asistente para configuración de contabilidad.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para ejecutar al Asistente para configuración de cuentas

Para ejecutar al Asistente para configuración de cuentas, completa los siguientes pasos:

1. Abre la consola NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haz clic en **contabilidad**.
3. En el panel de detalles, en **contabilidad**, haz clic en **configurar Contabilidad**.

## <a name="configure-nps-log-file-properties"></a>Configurar las propiedades de archivo de registro NPS

Puede configurar el servidor de directivas de redes (NPS) para administrar cuentas de servicio de autenticación remota telefónica de usuario (RADIUS) para solicitudes de autenticación de usuario, mensajes de aceptación, mensajes de denegación de acceso, solicitudes de cuentas y las respuestas y las actualizaciones de estado periódicas. Puedes usar este procedimiento para configurar los archivos de registro en el que quieres almacenar los datos de cuentas.

Para obtener más información acerca de cómo interpretar los archivos de registro, consulta [interpretar los archivos de registro de formato de base de datos de NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para evitar que los archivos de registro llenar el disco duro, se recomienda encarecidamente que se guarden en una partición independiente de la partición del sistema. Este tema te ofrecemos más información sobre cómo configurar cuentas de NPS:

- Para enviar los datos del archivo de registro de colección otro proceso, puedes configurar NPS para escribir en una canalización con nombre. Para usar canalizaciones con nombre, Establece la carpeta de archivos de registro en \\.\pipe o \\ComputerName\pipe. El programa de servidor de canalización con nombre crea una canalización con nombre denominada \\.\pipe\iaslog.log para aceptar los datos. En el cuadro de diálogo de propiedades de archivos Local, en crear un nuevo archivo de registro, selecciona nunca (tamaño del archivo ilimitado) cuando usas canalizaciones con nombre.

- El directorio de archivos de registro se puede crear mediante el uso de variables de entorno del sistema (en lugar de variables de usuario), como % systemdrive %, % systemroot % y % windir %. Por ejemplo, la ruta de acceso siguiente, con la variable % windir % del entorno, ubica el archivo de registro en el directorio del sistema en la subcarpeta \System32\Logs (es decir, % windir%\System32\Logs\).

- Cambio de formato de archivo de registro no provoca un nuevo registro crearse. Si cambias de formatos de archivo de registro, el archivo que esté activo en el momento del cambio contendrá una mezcla de los dos formatos (registros en el inicio del registro tendrá el formato anterior y registros al final del registro tendrán el nuevo formato).

- Si se produce un error en la administración de cuentas RADIUS debido a una unidad de disco duro completa o a otras causas, NPS detiene el procesamiento de solicitudes de conexión, impide que los usuarios acceso a recursos de red.

- NPS proporciona la capacidad de iniciar sesión en una base de datos de Microsoft (r) SQL Server™ además de, o en lugar de registro en un archivo local.

Pertenencia a la **administradores de dominio** grupo es lo mínimo necesario para realizar este procedimiento.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar las propiedades de archivo de registro NPS

1. Abre la consola NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haz clic en **contabilidad**.
3. En el panel de detalles, en **propiedades de archivo de registro**, haz clic en **propiedades de archivo de registro de cambios**. La **propiedades de archivo de registro** abre el cuadro de diálogo.
4. En **propiedades de archivo de registro**, en la **configuración** pestaña **registrar la siguiente información**, asegúrese de elegir registrar suficiente información para alcanzar sus objetivos de contabilidad. Por ejemplo, si los registros se tienen que realizar correlación de sesión, selecciona las casillas de verificación.
5. En **registrar errores acción**, selecciona **si se produce un error en el registro, descartar las solicitudes de conexión** si desea NPS detenga el procesamiento de los mensajes de solicitud de acceso cuando los archivos de registro se completa o no está disponible por algún motivo. Si quieres NPS para seguir procesando las solicitudes de conexión si se produce un error en el registro, no actives esta casilla.
6. En la **propiedades de archivo de registro** cuadro de diálogo, haz clic en el **archivo de registro** pestaña.
7. En la **archivo de registro** pestaña **directorio**, escribe la ubicación donde quieres almacenar los archivos de registro NPS. La ubicación predeterminada es la carpeta systemroot\System32\LogFiles.
    >[!NOTE]
    >Si no se proporciona una instrucción de ruta de acceso completa de **directorio de archivos de registro**, se usa la ruta de acceso de forma predeterminada. Por ejemplo, si escribes **archivoDeRegistroNPS** en **directorio de archivos de registro**, el archivo se encuentra en % systemroot%\System32\NPSLogFile.
8. En **formato**, haz clic en **compatible con DTS**. Si lo prefieres, en su lugar, puede seleccionar un formato de archivo heredado como **ODBC \(Legacy\)** o **IAS \(Legacy\)**.
    >[!NOTE]
    >**ODBC** y **IAS** tipos de archivo heredadas contienen un subconjunto de la información que NPS envía a su base de datos de SQL Server. La **compatible con DTS** formato XML del tipo de archivo es idéntico al formato XML que usa NPS para importar datos en su base de datos de SQL Server. Por lo tanto, la **compatible con DTS** formato de archivo proporciona una más eficaz y completa la transferencia de datos en la base de datos de SQL Server estándar para NPS.
9. En **crea un nuevo archivo de registro**, para la configuración de NPS para iniciar nuevos archivos de registro en los intervalos especificados, haz clic en el intervalo que desee usar:
    - Para un volumen de transacciones y actividad de registro, haz clic en **diario**.
    - Para volúmenes de transacciones y actividad de registro menores, haga clic en **semanales** o **mensual**.
    - Para almacenar todas las transacciones en un archivo de registro, haz clic en **nunca \(unlimited file size\)**.
    - Para limitar el tamaño de cada archivo de registro, haz clic en **cuando el archivo de registro alcanza este tamaño**y, a continuación, escribe un tamaño de archivo, después de que se crea un nuevo registro. El tamaño predeterminado es 10 megabytes (MB).
10. Si quieres NPS para eliminar los archivos de registro anterior para crear espacio en disco para los nuevos archivos de registro cuando el disco duro está cerca de capacidad, asegúrate de que **cuando el disco está completa eliminar archivos de registro anteriores** está seleccionado. Esta opción no está disponible, sin embargo, si el valor de **crea un nuevo archivo de registro** es **nunca \(unlimited file size\)**. Además, si el archivo de registro más antiguo es el archivo de registro actual, no se elimine.

## <a name="configure-nps-sql-server-logging"></a>Configurar el registro del servidor NPS SQL

Puedes usar este procedimiento para datos de cuentas de RADIUS de registro en una base de datos local o remota ejecutando Microsoft SQL Server.

>[!NOTE]
>NPS formatos de datos de cuentas como un documento XML que se envía a la **report_event** procedimiento almacenado en la base de datos de SQL Server que designes en NPS. Para SQL Server de registro para que funcione correctamente, debes tener un procedimiento llamado **report_event** en la base de datos de SQL Server que pueda recibir y analizar los documentos XML de NPS.

Pertenencia al grupo Administradores de dominio o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar el registro en NPS de SQL Server

1. Abre la consola NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haz clic en **contabilidad**.
3. En el panel de detalles, en **propiedades del registro de SQL Server**, haz clic en **propiedades del registro cambio SQL Server**. La **propiedades del registro de SQL Server** abre el cuadro de diálogo.
4. En **registrar la siguiente información**, seleccione la información que quieres iniciar sesión: 
    - Para registrar todas las solicitudes de cuentas, haz clic en **solicitudes de cuentas**.
    - Solicitudes de autenticación, haga clic en **solicitudes de autenticación**.
    - Para registrar el estado de contabilidad periódicas, haz clic en **estado periódico de contabilidad**.
    - Para registrar el estado periódica, como solicitudes de cuentas intermedias, haz clic en **estado periódico**.
5. Para configurar el número de sesiones simultáneas permitidas entre el servidor NPS y SQL Server, escribe un número en **número máximo de sesiones simultáneas**.
6. Para configurar el origen de datos de SQL Server en **registro de SQL Server**, haz clic en **configurar**. La **propiedades de enlace de datos** abre el cuadro de diálogo. En la **conexión** pestaña, especificar lo siguiente: 
    - Para especificar el nombre del servidor en el que se almacena la base de datos, escribe o selecciona un nombre en **selecciona o escribe un nombre de servidor**.
    - Para especificar el método de autenticación con el que iniciar sesión en el servidor, haz clic en **seguridad integrada usa Windows NT**. O bien, haz clic en **usar un nombre de usuario específico y una contraseña**y, a continuación, escribe las credenciales en **nombre de usuario** y **contraseña**.
    - Para permitir que una contraseña en blanco, haz clic en **contraseña en blanco**.
    - Para almacenar la contraseña, haz clic en **Permitir guardar contraseña**.
    - Para especificar qué base de datos para conectarse al equipo que ejecuta SQL Server, haz clic en **selecciona la base de datos en el servidor**y, a continuación, selecciona un nombre de base de datos de la lista.
7. Para probar la conexión entre NPS y SQL Server, haz clic en **Probar conexión**. Haz clic en **Aceptar** cerrar **propiedades de enlace de datos**.
8. En **registrar errores acción**, selecciona **habilitar el registro de archivo de texto de conmutación por error** si desea NPS para continuar con el registro de archivos de texto si se produce un error en el registro de SQL Server. 
9. En **registrar errores acción**, selecciona **si se produce un error en el registro, descartar las solicitudes de conexión** si desea NPS detenga el procesamiento de los mensajes de solicitud de acceso cuando los archivos de registro se completa o no está disponible por algún motivo. Si quieres NPS para seguir procesando las solicitudes de conexión si se produce un error en el registro, no actives esta casilla.

## <a name="ping-user-name"></a>Nombre de usuario de ping

Algunos servidores proxy RADIUS y servidores de acceso de red periódicamente envían solicitudes de autenticación y administración de cuentas (conocidas como solicitudes ping) para comprobar que el servidor NPS está presente en la red. Estas solicitudes ping incluyen los nombres de usuario ficticia. Cuando NPS procesa estas solicitudes, rellena los registros de eventos y administración de cuentas con los registros de rechazo de acceso, hacer más difícil realizar un seguimiento de los registros válidos.

Cuando se configura una entrada al registro para **ping el nombre de usuario**, NPS coincide con el valor de la entrada del registro con el valor de nombre de usuario en las solicitudes de ping por otros servidores. Un **ping el nombre de usuario** entrada del registro especifica el nombre de usuario ficticio (o un nombre patrón del usuario, con las variables, que coincide con el nombre de usuario ficticio) enviados por servidores proxy RADIUS y servidores de acceso de red. Cuando NPS recibe solicitudes de ping que coinciden con el **ping el nombre de usuario** valor de la entrada del registro, NPS rechazará las solicitudes de autenticación sin procesar la solicitud. NPS no registra las transacciones relacionadas con el nombre de usuario ficticio en los archivos de registro, lo que facilita la interpretar el registro de eventos.

**Nombre de usuario de ping** no está instalado de manera predeterminada. Debes agregar **ping el nombre de usuario** en el registro. Puedes agregar una entrada al registro mediante el Editor del registro.

>[!CAUTION]
>Edición incorrecta del registro puede dañar gravemente el sistema. Antes de realizar cambios en el registro, debe hacer una copia de los datos importantes en el equipo.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para agregar el comando "ping" nombre de usuario en el registro

Nombre de usuario de ping puede agregarse a la siguiente clave del registro como un valor de cadena por un miembro del grupo de administradores local:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nombre**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Datos**: *nombre de usuario*

>[!TIP]
>Para indicar más de un nombre de usuario para una **ping el nombre de usuario** valor, escribe un patrón de nombre, como un nombre DNS, incluidos los caracteres comodín en **datos**.
