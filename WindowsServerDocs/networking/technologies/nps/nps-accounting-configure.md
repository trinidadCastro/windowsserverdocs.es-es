---
title: Configurar las cuentas de servidor de directivas de redes
description: En este tema se proporciona información acerca del archivo de texto y el registro de SQL Server para el servidor de directivas de redes en Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: lizross
author: eross-msft
ms.date: 05/25/2018
ms.openlocfilehash: 26edf4d1ae4a30ccd9219392c7c4ee3604dcdad9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316363"
---
# <a name="configure-network-policy-server-accounting"></a>Configurar las cuentas de servidor de directivas de redes

Hay tres tipos de registro para el servidor de directivas de redes \(\)NPS:

- **Registro de eventos**. Se usa principalmente para la auditoría y la solución de problemas de intentos de conexión. Puede configurar el registro de eventos de NPS mediante la obtención de las propiedades de NPS en la consola de NPS.

- **Registro de solicitudes de autenticación y cuentas de usuario en un archivo local**. Se usa principalmente para realizar el análisis de las conexiones y para la facturación. Además, resulta útil como herramienta de investigación de seguridad, ya que le proporciona un método para realizar un seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configuración de cuentas.

- **Registro de solicitudes de autenticación y cuentas de usuario en una base de datos compatible con XML Microsoft SQL Server**. Se usa para permitir que varios servidores que ejecuten NPS tengan un origen de datos. Además, ofrece las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server mediante el Asistente para configuración de cuentas.

## <a name="use-the-accounting-configuration-wizard"></a>Usar el Asistente para configuración de cuentas

Mediante el Asistente para configuración de cuentas, puede configurar las cuatro siguientes opciones de configuración de cuentas:

- **Solo registro de SQL**. Al usar esta opción, puede configurar un vínculo de datos a un SQL Server que permita a NPS conectarse a SQL Server y enviar datos de cuentas. Además, el asistente puede configurar la base de datos en el SQL Server para asegurarse de que la base de datos es compatible con el registro de SQL Server de NPS.
- **Solo registro de texto**. Mediante esta opción, puede configurar NPS para registrar los datos de cuentas en un archivo de texto.
- **Registro paralelo**. Con esta configuración, puede configurar el vínculo de datos de SQL Server y la base de datos. También puede configurar el registro de archivos de texto para que NPS Registre simultáneamente en el archivo de texto y en la base de datos de SQL Server. 
- **Registro de SQL con copia de seguridad**. Con esta configuración, puede configurar el vínculo de datos de SQL Server y la base de datos. Además, puede configurar el registro de archivos de texto que NPS usa si se produce un error en el registro de SQL Server.

Además de estas opciones, el registro de SQL Server y el registro de texto permiten especificar si NPS sigue procesando las solicitudes de conexión si se produce un error en el registro. Puede especificar esto en la **sección acción de registro de errores** de propiedades de registro de archivos locales, en propiedades de registro de SQL Server y mientras se ejecuta el Asistente para configuración de cuentas.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para ejecutar el Asistente para configuración de cuentas

Para ejecutar el Asistente para configuración de cuentas, complete los pasos siguientes:

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **cuentas**.
3. En el panel de detalles, en **cuentas**, haga clic en **configurar cuentas**.

## <a name="configure-nps-log-file-properties"></a>Configurar las propiedades del archivo de registro de NPS

Puede configurar el servidor de directivas de redes (NPS) para realizar cuentas de Servicio de autenticación remota telefónica de usuario (RADIUS) para solicitudes de autenticación de usuario, mensajes de aceptación de acceso, mensajes de rechazo de acceso, solicitudes de cuentas y respuestas y periódicas actualizaciones de estado. Puede usar este procedimiento para configurar los archivos de registro en los que desea almacenar los datos de cuentas.

Para obtener más información sobre cómo interpretar los archivos de registro, vea [interpretar los archivos de registro de formato de base de datos NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para evitar que los archivos de registro llenen el disco duro, se recomienda mantenerlos en una partición diferente de la del sistema. A continuación se proporciona más información sobre la configuración de cuentas para NPS:

- Para enviar los datos del archivo de registro para que sean recopilados por otro proceso, puede configurar NPS para que escriba en una canalización con nombre. Para usar canalizaciones con nombre, establezca la carpeta de archivos de registro en \\.\pipe o \\ComputerName\pipe. El programa de servidor de canalización con nombre crea una canalización con nombre denominada \\.\pipe\iaslog.log para aceptar los datos. En el cuadro de diálogo de propiedades de Archivo local, en Crear un nuevo archivo de registro, seleccione Nunca (tamaño de archivo ilimitado) al utilizar canalizaciones con nombre.

- El directorio de archivo de registro se puede crear mediante variables de entorno del sistema (en lugar de variables de usuario), como %unidadDelSistema%, %raízDelSistema% y %windir%. Por ejemplo, la siguiente ruta de acceso, con la variable de entorno% WINDIR%, busca el archivo de registro en el directorio del sistema en la subcarpeta \System32\Logs (es decir,%windir%\System32\Logs\).

- El cambio de los formatos de archivo de registro no hace que se cree otro registro. Si cambia los formatos de archivo de registro, el archivo que está activo en el momento del cambio contendrá una mezcla de ambos formatos (los registros al principio del registro tendrán el formato anterior, mientras que los que están en el final del registro tendrán el formato nuevo).

- Si la cuenta RADIUS presenta un error debido a que el disco duro está lleno o por otros motivos, NPS deja de procesar las solicitudes de conexión, con lo que impide que los usuarios tengan acceso a los recursos de red.

- NPS proporciona la capacidad de registro en una base de datos de Microsoft® SQL Server™ además de, o en lugar de, registro en un archivo local.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins** . del dominio.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar las propiedades del archivo de registro NPS

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **cuentas**.
3. En el panel de detalles, en **propiedades del archivo de registro**, haga clic en **Cambiar propiedades del archivo de registro**. Se abrirá el cuadro de diálogo **propiedades del archivo de registro** .
4. En **propiedades del archivo de registro**, en la pestaña **configuración** , en **registrar la información siguiente**, asegúrese de que elige registrar suficiente información para lograr los objetivos de contabilidad. Por ejemplo, si los registros necesitan realizar la correlación de la sesión, active todas las casillas.
5. En **acción de registro de errores**, seleccione **si se produce un error en el registro, descartar solicitudes de conexión** si desea que NPS detenga el procesamiento de mensajes de solicitud de acceso cuando los archivos de registro estén llenos o no estén disponibles por algún motivo. Si desea que NPS siga procesando solicitudes de conexión si se produce un error en el registro, no active esta casilla.
6. En el cuadro de diálogo **propiedades del archivo de registro** , haga clic en la pestaña archivo de **registro** .
7. En la pestaña **archivo de registro** , en **directorio**, escriba la ubicación donde desea almacenar los archivos de registro de NPS. La ubicación predeterminada es la carpeta raízDelSistema\System32\archivosDeRegistro.<br>Si no proporciona una instrucción de ruta de acceso completa en el **directorio del archivo de registro**, se usa la ruta de acceso predeterminada. Por ejemplo, si escribe **archivoderegistronps** en el **directorio del archivo de registro**, el archivo se encuentra en%systemroot%\System32\NPSLogFile.
8. En **formato**, haga clic en **compatible con DTS**. Si lo prefiere, puede seleccionar un formato de archivo heredado, como **ODBC \(heredado\)** o **IAS \(\)heredado** .<br>Los tipos de archivos heredados de **ODBC** e **IAS** contienen un subconjunto de la información que NPS envía a su base de datos de SQL Server. El formato XML del tipo de archivo **compatible con DTS** es idéntico al formato XML que usa NPS para importar datos en su SQL Server base de datos. Por lo tanto, el formato de archivo **compatible con DTS** proporciona una transferencia más eficaz y completa de los datos en la base de datos de SQL Server estándar para NPS.
9. En **crear un nuevo archivo de registro**, para configurar NPS para que inicie nuevos archivos de registro a intervalos especificados, haga clic en el intervalo que desea usar:
    - Para una actividad de registro y volumen de transacciones pesadas, haga clic en **diariamente**.
    - En el caso de los volúmenes de transacciones menores y la actividad de registro, haga clic en **semanal** o **mensualmente**.
    - Para almacenar todas las transacciones en un archivo de registro, haga clic en **nunca \(tamaño de archivo ilimitado\)** .
    - Para limitar el tamaño de cada archivo de registro, haga clic en **cuando el archivo de registro alcance este tamaño**y, a continuación, escriba un tamaño de archivo después del cual se crea un nuevo registro. El tamaño predeterminado es 10 megabytes (MB).
10. Si desea que NPS elimine los archivos de registro antiguos con el fin de crear espacio en disco para los nuevos archivos de registro cuando el disco duro esté cerca de la capacidad, asegúrese de que esté seleccionado el **disco eliminar archivos de registro más antiguos** . Sin embargo, esta opción no está disponible si el valor de **crear un nuevo archivo** de registro **nunca es \(tamaño de archivo ilimitado\)** . Además, si el archivo de registro más antiguo es el archivo de registro actual, no se elimina.

## <a name="configure-nps-sql-server-logging"></a>Configuración del registro de SQL Server de NPS

Puede usar este procedimiento para registrar datos de cuentas RADIUS en una base de datos local o remota que ejecute Microsoft SQL Server.

>[!NOTE]
>NPS da formato a los datos de cuentas como un documento XML que envía al **report_event** procedimiento almacenado en la base de datos SQL Server que se designa en NPS. Para que el registro de SQL Server funcione correctamente, debe tener un procedimiento almacenado denominado **report_event** en la base de datos SQL Server que pueda recibir y analizar los documentos XML de NPS.

El requisito mínimo necesario para completar este procedimiento es ser miembro del grupo Administradores del equipo u otro equivalente.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar el registro de SQL Server en NPS

1. Abra la consola de NPS o el complemento Microsoft Management Console (MMC) de NPS.
2. En el árbol de consola, haga clic en **cuentas**.
3. En el panel de detalles, en **SQL Server propiedades de registro**, haga clic en **cambiar SQL Server propiedades de registro**. Se abre el cuadro de diálogo **propiedades de registro de SQL Server** .
4. En **registrar la información siguiente**, seleccione la información que desea registrar: 
    - Para registrar todas las solicitudes de cuentas, haga clic en **solicitudes de cuentas**.
    - Para registrar solicitudes de autenticación, haga clic en **solicitudes de autenticación**.
    - Para registrar el estado de cuentas periódicas, haga clic en **Estado de cuentas periódicas**.
    - Para registrar el estado periódico, como las solicitudes de cuentas provisionales, haga clic en **Estado periódico**.
5. Para configurar el número de sesiones simultáneas permitidas entre el servidor que ejecuta NPS y el SQL Server, escriba un número en **número máximo de sesiones simultáneas**.
6. Para configurar el origen de datos de SQL Server, en **registro de SQL Server**, haga clic en **configurar**. Se abre el cuadro de diálogo **propiedades de vínculo de datos** . En la pestaña **conexión** , especifique lo siguiente: 
    - Para especificar el nombre del servidor en el que se almacena la base de datos, escriba o seleccione un nombre en **seleccionar o escriba un nombre de servidor**.
    - Para especificar el método de autenticación con el que se va a iniciar sesión en el servidor, haga clic en **usar seguridad integrada de Windows NT**. O bien, haga clic en **usar un nombre de usuario y una contraseña específicos**y, a continuación, escriba las credenciales en **nombre de usuario** y **contraseña**.
    - Para permitir una contraseña en blanco, haga clic en **contraseña en blanco**.
    - Para almacenar la contraseña, haga clic en **Permitir guardar contraseña**.
    - Para especificar la base de datos a la que se va a conectar en el equipo que ejecuta SQL Server, haga clic en **seleccionar la base de datos en el servidor**y, a continuación, seleccione un nombre de base de datos de la lista.
7. Para probar la conexión entre NPS y SQL Server, haga clic en **probar conexión**. Haga clic en **Aceptar** para cerrar **propiedades de vínculo de datos**.
8. En **acción de registro de errores**, seleccione **Habilitar el registro de archivos de texto para conmutación por error** si desea que NPS continúe con el registro de archivos de texto si se produce un error en el registro de SQL Server. 
9. En **acción de registro de errores**, seleccione **si se produce un error en el registro, descartar solicitudes de conexión** si desea que NPS detenga el procesamiento de mensajes de solicitud de acceso cuando los archivos de registro estén llenos o no estén disponibles por algún motivo. Si desea que NPS siga procesando solicitudes de conexión si se produce un error en el registro, no active esta casilla.

## <a name="ping-user-name"></a>Ping user-name

Algunos servidores proxy RADIUS y servidores de acceso a la red envían periódicamente solicitudes de autenticación y cuentas (conocidas como solicitudes ping) para comprobar que el NPS está presente en la red. Estas solicitudes ping incluyen nombres de usuario ficticios. Cuando NPS procesa dichas solicitudes, los archivos de registro de eventos y cuentas se llenan de registros de rechazos de acceso, lo que hace más difícil realizar un seguimiento de los registros válidos.

Cuando se configura una entrada del registro para **ping User-Name**, NPS compara el valor de la entrada del registro con el valor de nombre de usuario en las solicitudes ping de otros servidores. Una entrada de registro de **ping User-Name** especifica el nombre de usuario ficticio (o un patrón de nombre de usuario, con variables, que coincide con el nombre de usuario ficticio) enviado por servidores proxy RADIUS y servidores de acceso a la red. Cuando NPS recibe solicitudes de ping que coinciden con el valor de la entrada del registro **ping User-Name** , NPS rechaza las solicitudes de autenticación sin procesar la solicitud. NPS no registra las transacciones relacionadas con el nombre de usuario ficticio de los archivos de registro, lo que facilita la interpretación del registro de eventos.

**Ping User-Name** no se instala de forma predeterminada. Debe agregar **ping User-Name** al registro. Puede agregar una entrada al Registro mediante el Editor del Registro.

>[!CAUTION]
>Si se modifica incorrectamente el Registro, se puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para agregar ping User-Name al registro

Un miembro del grupo local de administradores puede Agregar el nombre de usuario de ping a la siguiente clave del registro como valor de cadena:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nombre**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Datos**: *nombre de usuario*

>[!TIP]
>Para indicar más de un nombre de usuario para un valor de **nombre de usuario de ping** , escriba un patrón de nombre, como un nombre DNS, incluidos los caracteres comodín, en **datos**.
