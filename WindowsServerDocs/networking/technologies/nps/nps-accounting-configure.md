---
title: Configurar las cuentas de servidor de directivas de redes
description: Este tema proporciona información sobre el archivo de texto y registro para el servidor de directivas de redes en Windows Server 2016 de SQL Server.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: c732a9f42d942ad579468d1dd15d30324d6fea87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839706"
---
# <a name="configure-network-policy-server-accounting"></a>Configurar las cuentas de servidor de directivas de redes

Hay tres tipos de registro para el servidor de directivas de red \(NPS\):

- **Registro de eventos**. Se utiliza principalmente para supervisar y solucionar los intentos de conexión. Puede configurar el registro mediante la obtención de las propiedades NPS en la consola de NPS de eventos NPS.

- **Registro de autenticación de usuario y las solicitudes de cuentas en un archivo local**. Se utiliza principalmente para fines de facturación y análisis de la conexión. También resulta útil como una herramienta de investigación de seguridad porque proporciona un método de seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configuración de cuentas.

- **Registro de autenticación de usuario y las solicitudes de cuentas en una base de datos de Microsoft SQL Server compatible con XML**. Se usa para permitir que a varios servidores que ejecuten NPS tengan un origen de datos. También proporciona las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server utilizando el Asistente para configuración de cuentas.

## <a name="use-the-accounting-configuration-wizard"></a>Utilice al Asistente para configuración de cuentas

Al usar al Asistente para configuración de contabilidad, puede configurar las siguientes cuatro configuraciones de cuentas:

- **El registro de SQL sólo**. Con esta configuración, puede configurar un vínculo de datos a SQL Server que permite a NPS conectarse y enviar datos a SQL server. Además, el asistente puede configurar la base de datos en SQL Server para asegurarse de que la base de datos es compatible con el registro del servidor SQL de NPS.
- **Registro de sólo texto**. Con esta configuración, puede configurar NPS para registrar datos de cuentas a un archivo de texto.
- **Registro en paralelo**. Con esta configuración, puede configurar el vínculo de datos de SQL Server y base de datos. También puede configurar el registro de archivo de texto para que NPS registre de forma simultánea en el archivo de texto y la base de datos de SQL Server. 
- **Registro de SQL con la copia de seguridad**. Con esta configuración, puede configurar el vínculo de datos de SQL Server y base de datos. Además, puede configurar el registro de archivo de texto que usa NPS si se produce un error en el registro de SQL Server.

Además de estos valores, tanto el registro de SQL Server y el registro de texto le permiten especificar si NPS continúa procesando las solicitudes de conexión si se produce un error en el registro. Puede especificar esto en el **sección de acción de error de registro** en Propiedades de registro de archivo local, en las propiedades de registro del servidor SQL, y mientras se ejecuta el Asistente para configuración de contabilidad.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para ejecutar al Asistente para configuración de cuentas

Para ejecutar al Asistente para configuración de contabilidad, complete los pasos siguientes:

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **contabilidad**.
3. En el panel de detalles, en **contabilidad**, haga clic en **configurar Contabilidad**.

## <a name="configure-nps-log-file-properties"></a>Configurar las propiedades de archivo de registro NPS

Puede configurar el servidor de directivas de redes (NPS) para realizar el servicio autenticación remota telefónica de usuario (RADIUS), periódico y contabilidad para las solicitudes de autenticación de usuario, mensajes de aceptación de acceso, los mensajes de rechazo de acceso, las solicitudes de cuentas y las respuestas actualizaciones de estado. Puede usar este procedimiento para configurar los archivos de registro en el que desea almacenar los datos de cuentas.

Para obtener más información acerca de cómo interpretar los archivos de registro, consulte [interpretar los archivos de registro de formato de base de datos de NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para evitar que los archivos de registro llenen el disco duro, se recomienda mantenerlos en una partición independiente de la partición del sistema. El siguiente proporciona más información acerca de cómo configurar cuentas de NPS:

- Para enviar los datos del archivo de registro para la recopilación por otro proceso, puede configurar NPS para escribir en una canalización con nombre. Para usar canalizaciones con nombre, establezca la carpeta de archivos de registro en \\. \pipe o \\ComputerName\pipe. El programa de canalización con nombre de servidor crea una canalización con nombre denominada \\.\pipe\iaslog.log para aceptar los datos. En el cuadro de diálogo Propiedades de archivo Local, en crear un nuevo archivo de registro, seleccione nunca (tamaño de archivo ilimitado) al utilizar canalizaciones con nombre.

- El directorio de archivos de registro se puede crear mediante variables de entorno del sistema (en lugar de variables de usuario), como % systemdrive %, % systemroot % y % windir %. Por ejemplo, la ruta de acceso siguiente, mediante la variable % windir % del entorno, busca el archivo de registro en el directorio del sistema en la subcarpeta \System32\Logs (es decir, %windir%\System32\Logs\).

- Cambio de formato de archivo de registro no hace que se cree un registro nuevo. Si cambia los formatos de archivo de registro, el archivo que está activo en el momento del cambio contendrá una mezcla de los dos formatos (registros al principio del registro tendrán el formato anterior y registros al final del registro tendrán el formato nuevo).

- Si se produce un error en la administración de cuentas RADIUS debido a una unidad de disco duro completa u otras causas, NPS deja de procesar las solicitudes de conexión, impide que los usuarios tener acceso a los recursos de red.

- NPS proporciona la capacidad de iniciar sesión en una base de datos de Microsoft® SQL Server™ además o en lugar de un registro en un archivo local.

Pertenencia a la **Admins. del dominio** grupo es el requisito mínimo para llevar a cabo este procedimiento.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar las propiedades de archivo de registro NPS

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **contabilidad**.
3. En el panel de detalles, en **propiedades del archivo de registro**, haga clic en **cambiar propiedades de archivo de registro**. El **propiedades del archivo de registro** abre el cuadro de diálogo.
4. En **propiedades del archivo de registro**, en el **configuración** ficha **la siguiente información de registro**, asegúrese de elegir registrar información suficiente para lograr sus objetivos de contabilidad. Por ejemplo, si los registros se necesitan realizar la correlación de sesiones, seleccione todas las casillas de verificación.
5. En **acción de error de registro**, seleccione **si se produce un error en el registro, las solicitudes de conexión de descartar** si desea que NPS para detener el procesamiento de mensajes de solicitud de acceso cuando los archivos de registro están lleno o no disponible por algún motivo. Si desea que NPS para continuar procesando las solicitudes de conexión si se produce un error en el registro, no seleccione esta casilla de verificación.
6. En el **propiedades del archivo de registro** cuadro de diálogo, haga clic en el **archivo de registro** ficha.
7. En el **archivo de registro** ficha **directorio**, escriba la ubicación donde desea almacenar los archivos de registro NPS. La ubicación predeterminada es la carpeta systemroot\System32\LogFiles.<br>Si no se proporciona una instrucción de ruta de acceso completa en **directorio archivos de registro**, se usa la ruta de acceso predeterminada. Por ejemplo, si escribe **archivoDeRegistroNPS** en **directorio archivos de registro**, el archivo se encuentra en % systemroot%\System32\NPSLogFile.
8. En **formato**, haga clic en **compatible con DTS**. Si lo prefiere, en su lugar, puede seleccionar un formato de archivo heredado, como **ODBC \(heredado\)**  o **IAS \(heredado\)**.<br>**ODBC** y **IAS** tipos de archivo heredado contienen un subconjunto de la información que NPS envía a su base de datos de SQL Server. El **compatible con DTS** formato XML del tipo de archivo es idéntico al formato XML que usa NPS para importar datos en su base de datos de SQL Server. Por lo tanto, el **compatible con DTS** formato de archivo proporciona una transferencia de datos más eficaz y completa en la base de datos de SQL Server standard para NPS.
9. En **crear un nuevo archivo de registro**, para configurar NPS para que inicie nuevos archivos de registro a intervalos especificados, haga clic en el intervalo que desea usar:
    - Para grandes volúmenes de transacciones y actividad de registro, haga clic en **diario**.
    - Para volúmenes de transacciones y actividad de registro menores, haga clic en **semanal** o **mensual**.
    - Para almacenar todas las transacciones en un archivo de registro, haga clic en **nunca \(tamaño de archivo ilimitado\)**.
    - Para limitar el tamaño de cada archivo de registro, haga clic en **al archivo de registro alcanza este tamaño**y, a continuación, escriba un tamaño de archivo, después del cual se crea un nuevo registro. El tamaño predeterminado es 10 megabytes (MB).
10. Si desea que NPS para eliminar los archivos de registro antiguos para crear espacio en disco para nuevos archivos de registro cuando el disco duro esté cerca de la capacidad, asegúrese de que **cuando el disco está llena Elimine los archivos más antiguos** está seleccionada. Esta opción no está disponible, sin embargo, si el valor de **crear un nuevo archivo de registro** es **nunca \(tamaño de archivo ilimitado\)**. Además, si el archivo de registro más antiguo es el archivo de registro actual, no se elimina.

## <a name="configure-nps-sql-server-logging"></a>Configurar el registro del servidor SQL de NPS

Puede usar este procedimiento para datos de cuentas de RADIUS de registro a una base de datos local o remoto que ejecuta Microsoft SQL Server.

>[!NOTE]
>NPS da formato a los datos de cuentas como un documento XML que envía a la **report_event** procedimiento almacenado en la base de datos de SQL Server designada en NPS. Para que funcione correctamente el registro de SQL Server, debe tener un procedimiento almacenado denominado **report_event** en la base de datos de SQL Server que pueda recibir y analizar los documentos XML procedentes de NPS.

Pertenencia al grupo Admins. del dominio, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar el registro de NPS de SQL Server

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **contabilidad**.
3. En el panel de detalles, en **propiedades del registro de SQL Server**, haga clic en **propiedades del registro de cambio SQL Server**. El **propiedades del registro de SQL Server** abre el cuadro de diálogo.
4. En **la siguiente información de registro**, seleccione la información que desea iniciar: 
    - Para registrar todas las solicitudes de cuentas, haga clic en **las solicitudes de cuentas**.
    - Para registrar las solicitudes de autenticación, haga clic en **las solicitudes de autenticación**.
    - Para registrar el estado periódico de cuentas, haga clic en **estado periódico de contabilidad**.
    - Para registrar el estado periódico, como solicitudes temporales de cuentas, haga clic en **estado periódico**.
5. Para configurar el número de sesiones simultáneas permitidas entre el servidor que ejecuta NPS y el servidor SQL Server, escriba un número en **número máximo de sesiones simultáneas**.
6. Para configurar el origen de datos de SQL Server en **registro de SQL Server**, haga clic en **configurar**. El **propiedades de vínculo de datos** abre el cuadro de diálogo. En el **conexión** ficha, especifique lo siguiente: 
    - Para especificar el nombre del servidor en el que se almacena la base de datos, escriba o seleccione un nombre en **seleccione o escriba un nombre de servidor**.
    - Para especificar el método de autenticación que se va a iniciar sesión en el servidor, haga clic en **seguridad integrada de uso Windows NT**. O bien, haga clic en **utilizar un nombre de usuario específico y una contraseña**y, a continuación, escriba las credenciales en **nombre de usuario** y **contraseña**.
    - Para permitir que una contraseña en blanco, haga clic en **contraseña en blanco**.
    - Para almacenar la contraseña, haga clic en **Permitir guardar contraseña**.
    - Para especificar qué base de datos al que conectarse en el equipo que ejecuta SQL Server, haga clic en **seleccione la base de datos en el servidor**y, a continuación, seleccione un nombre de base de datos de la lista.
7. Para probar la conexión entre NPS y SQL Server, haga clic en **Probar conexión**. Haga clic en **Aceptar** para cerrar **propiedades de vínculo de datos**.
8. En **acción de error de registro**, seleccione **habilitar el registro de archivo de texto para la conmutación por error** si desea que NPS para continuar con el registro de archivo de texto si se produce un error en el registro de SQL Server. 
9. En **acción de error de registro**, seleccione **si se produce un error en el registro, las solicitudes de conexión de descartar** si desea que NPS para detener el procesamiento de mensajes de solicitud de acceso cuando los archivos de registro están lleno o no disponible por algún motivo. Si desea que NPS para continuar procesando las solicitudes de conexión si se produce un error en el registro, no seleccione esta casilla de verificación.

## <a name="ping-user-name"></a>Nombre de usuario de ping

Algunos servidores proxy RADIUS y servidores de acceso a la red envían periódicamente solicitudes de autenticación y cuentas (conocidas como solicitudes ping) para comprobar que el NPS está presente en la red. Estas solicitudes ping incluyen nombres de usuario ficticios. Cuando NPS procesa dichas solicitudes, los registros de eventos y las cuentas estén llenas de registros de rechazos de acceso, lo que dificulta a realizar un seguimiento de los registros válidos.

Al configurar una entrada de registro **hacer ping en nombre de usuario**, NPS coincide con el valor de entrada del registro con el valor de nombre de usuario de las solicitudes de ping por otros servidores. Un **hacer ping en nombre de usuario** entrada del registro especifica el nombre de usuario ficticio (o un usuario patrón de nombre, con variables, que coincide con el nombre de usuario ficticio) enviado por los servidores proxy RADIUS y servidores de acceso de red. Cuando NPS recibe solicitudes ping que coinciden con el **hacer ping en nombre de usuario** valor de entrada del registro, NPS rechaza las solicitudes de autenticación sin procesar la solicitud. NPS no registra las transacciones relacionadas con el nombre de usuario ficticio en los archivos de registro, lo que hace más fácil de interpretar el registro de eventos.

**Haga ping a nombre de usuario** no está instalado de forma predeterminada. Debe agregar **hacer ping en nombre de usuario** en el registro. Puede agregar una entrada en el registro mediante el Editor del registro.

>[!CAUTION]
>la modificación incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para agregar el nombre de usuario de ping en el registro

Nombre de usuario de ping se puede agregar a la siguiente clave del registro como un valor de cadena por un miembro del grupo Administradores local:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nombre**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Datos**:  *Nombre de usuario*

>[!TIP]
>Para indicar más de un nombre de usuario para un **hacer ping en nombre de usuario** valor, escriba un patrón de nombre, por ejemplo, un nombre DNS, incluidos los caracteres comodín, en **datos**.
