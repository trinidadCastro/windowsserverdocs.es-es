---
title: Administrar el Registro de acceso de usuarios
description: Describe cómo administrar el registro de acceso de usuario
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d65a40e229fe4b0a1b27db496523dfe7a9419752
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886796"
---
# <a name="manage-user-access-logging"></a>Administrar el Registro de acceso de usuarios

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este documento se describe cómo administrar el registro de acceso de usuarios (UAL).  
  
UAL es una característica que ayuda a los administradores del servidor a cuantificar las solicitudes de cliente único de roles y servicios en un servidor local.  
  
UAL se instala y habilita de forma predeterminada y recopila datos prácticamente en tiempo real. UAL solo presenta unas cuantas opciones de configuración, que se detallan en este documento, así como la finalidad de cada una de ellas.  
  
Para obtener más información acerca de las ventajas de UAL, consulte el [Introducción a User Access Logging](get-started-with-user-access-logging.md).  
  
**En este documento**  
  
Las opciones de configuración que se abordan en este documento son:  
  
-   Deshabilitar y habilitar el servicio UAL  
  
-   Recopilar y eliminar datos  
  
-   Eliminar los datos que UAL registra  
  
-   Administrar UAL en entornos de gran volumen  
  
-   Recuperarse de un estado dañado  
  
-   Habilitar el seguimiento de licencias de uso de Carpetas de trabajo   
  
## <a name="BKMK_Step1"></a>Deshabilitar y habilitar el servicio UAL  
UAL se habilita y ejecuta de forma predeterminada cuando un equipo que ejecuta Windows Server 2012, o posterior, instalado e iniciado por primera vez. Puede que los administradores prefieran desactivar y deshabilitar UAL para cubrir requisitos de privacidad o necesidades operativas. Puede desactivar UAL mediante la consola de servicios, desde la línea de comandos, o mediante cmdlets de PowerShell. Sin embargo, para asegurarse de que UAL no se ejecute la próxima vez que se inicia el equipo, también deberá deshabilitar el servicio. Los procedimientos siguientes se describe cómo desactivar y deshabilitar UAL.  
  
> [!NOTE]  
> Puede usar el cmdlet de PowerShell `Get-Service UALSVC` para recuperar información sobre el servicio UAL, como, por ejemplo, si está en funcionamiento, detenido, habilitado o deshabilitado.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Para detener y deshabilitar el servicio UAL mediante la consola de Servicios  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  En el Administrador del servidor, seleccione **Herramientas** y haga clic en **Servicios**.  
  
3.  Desplácese hacia abajo y seleccione **Servicio de registro de acceso de usuarios**. Haga clic en **Detener el servicio**.  
  
4.  Derecha\-haga clic en el nombre del servicio y seleccione **propiedades**. En la pestaña **General** , cambie **Tipo de inicio** a **Deshabilitado**y haga clic en **Aceptar**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Para detener y deshabilitar UAL desde la línea de comandos  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  Presione las teclas de logotipo de Windows+R y, después, escriba **cmd** para abrir una ventana del símbolo del sistema.  
  
    > [!IMPORTANT]  
    > Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
3.  Escriba **net stop ualsvc** y presione ENTRAR.  
  
4.  Escriba **netsh ualsvc set opmode mode=disable** y presione ENTRAR.  
   
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  

UAL también se puede detener y deshabilitar si se usan los comandos de Windows PowerShell Stop-service y Disable-Ual.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Si posteriormente desea reiniciar y volver a habilitar UAL puede hacerlo con los siguientes procedimientos.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Para iniciar y habilitar el servicio UAL mediante la consola de Servicios  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  En el Administrador del servidor, seleccione **Herramientas** y haga clic en **Servicios**.  
  
3.  Desplácese hacia abajo y seleccione **Servicio de registro de acceso de usuarios**. Haga clic en **Iniciar el servicio**.  
  
4.  Haga clic con el botón secundario en el nombre del servicio y seleccione **Propiedades**. En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Para iniciar y habilitar UAL desde la línea de comandos  
  
1.  Inicie sesión en el servidor con credenciales de administrador local.  
  
2.  Presione las teclas de logotipo de Windows+R y, después, escriba **cmd** para abrir una ventana del símbolo del sistema.  
  
    > [!IMPORTANT]  
    > Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
3.  Escriba **net start ualsvc** y presione ENTRAR.  
  
4.  Escriba **netsh ualsvc set opmode mode=enable**y presione ENTRAR.  

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.
  
UAL también se puede iniciar y volver a habilitar si se usan los comandos de Windows PowerShell Start-service y Enable-Ual.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="BKMK_Step2"></a>Recopilar datos de UAL  
Además de los cmdlets de PowerShell que se describe en la sección anterior, 12 cmdlets adicionales puede usarse para recopilar datos de UAL:  
  
-   **Get-UalOverview**: proporciona detalles de UAL y un historial de los roles y productos instalados.  
  
-   **Get-UalServerUser**: proporciona datos de acceso del usuario del cliente relativos al servidor local o de destino.  
  
-   **Get-UalServerDevice**:  proporciona datos de acceso del dispositivo cliente relativos al servidor local o de destino.  
  
-   **Get-UalUserAccess**: proporciona datos de acceso del usuario del cliente relativos a cada rol o producto instalado en el servidor local o de destino.  
  
-   **Get-UalDeviceAccess**: proporciona datos de acceso del dispositivo cliente relativos a cada rol o producto instalado en el servidor local o de destino.  
  
-   **Get-UalDailyUserAccess**: proporciona datos de acceso del usuario del cliente relativos a cada día del año.  
  
-   **Get-UalDailyDeviceAccess**: proporciona datos de acceso del dispositivo cliente relativos a cada día del año.  
  
-   **Get-UalDailyAccess**: proporciona datos de acceso tanto del usuario como del dispositivo cliente relativos a cada día del año.  
  
-   **Get-UalHyperV**: proporciona datos de equipo virtual relevantes para el servidor local o de destino.  
  
-   **Get-UalDns**: proporciona datos específicos de cliente DNS del servidor DNS local o de destino.  
  
-   **Get-UalSystemId**: proporciona datos específicos del sistema con los que se identifica de forma única al servidor local o de destino.  
  
`Get-UalSystemId` está pensado para proporcionar un perfil único de un servidor para establecer una correlación con el resto de datos de dicho servidor.  Si un servidor experimenta algún cambio en uno de los parámetros de `Get-UalSystemId` se crea un nuevo perfil.  `Get-UalOverview` está pensado para proveer al administrador de una lista de los roles que hay instalados y en uso en el servidor.  
  
> [!NOTE]  
> Características básicas de servicios de impresión y servicios de documentos y archivos se instalan de forma predeterminada. Por lo tanto, los administradores pueden esperar ver información sobre estos servicios siempre que quieran, como si los roles completos estuvieran instalados. Se incluyen cmdlets de UAL por separado para Hyper-V y DNS debido a los datos únicos que UAL recopila para estos roles de servidor.  
  
Un escenario de caso de uso típico de los cmdlets de UAL sería un administrador que consulta a UAL acerca de los accesos de cliente único que han tenido lugar entre dos fechas. Esto se logra de diversas maneras. A continuación se detalla el método recomendado para consultar los accesos de dispositivo único entre dos fechas.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Con este procedimiento se obtendrá una lista exhaustiva de todos los dispositivos cliente únicos (identificados por su dirección IP) que han enviado solicitudes al servidor durante dicho periodo de tiempo.  
  
El parámetro ‘ActivityCount’ de cada cliente único se limita a 65.535 al día. Asimismo, solo es necesario llamar a WMI desde PowerShell cuando se realiza la consulta por fecha.  El resto de parámetros de cmdlet de UAL se puede usar en consultas de PS del modo previsto, como en el siguiente ejemplo:  
  
```  
PS C:\Windows\system32> Get-UalDeviceAccess -IPAddress "10.36.206.112"  
  
ActivityCount    : 1  
FirstSeen        : 6/23/2012 5:06:50 AM  
IPAddress        : 10.36.206.112  
LastSeen         : 6/23/2012 5:06:50 AM  
ProductName      : Windows Server 2012 Datacenter  
RoleGuid         : 10a9226f-50ee-49d8-a393-9a501d47ce04  
RoleName         : File Server  
TenantIdentifier : 00000000-0000-0000-0000-000000000000  
PSComputerName  
  
```  
  
UAL conserva un historial de hasta dos años atrás. Para permitir que un administrador pueda recuperar datos de UAL cuando el servicio esté ejecutándose, UAL hace una copia del archivo de base de datos activo (current.mdb) denominada *GUID.mdb* cada 24 horas para su uso por parte del proveedor de WMI.  
  
El primer día del año, UAL creará una base de datos *GUID.mdb*nueva, y la antigua *GUID.mdb* se conserva como un archivo para que el proveedor pueda usarlo.  Transcurridos dos años, el archivo *GUID.mdb* original se sobrescribirá.  
  
> [!IMPORTANT]  
> El siguiente procedimiento solo debe realizarlo un usuario avanzado y, por lo general, está pensado para un desarrollador que realiza pruebas en su propio instrumental de interfaces de programación de aplicación de UAL.  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Para ajustar el intervalo de 24 horas para hacer que los datos estén visibles para el proveedor de WMI  
  
1.  Inicie sesión en el servidor con una cuenta que tenga privilegios de administrador local.  
  
2.  Presione las teclas de logotipo de Windows+R y, después, escriba **cmd** para abrir una ventana del símbolo del sistema.  
  
3.  Agregue el valor de Registro:  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)**.  
  
    > [!WARNING]  
    > La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer y guardar una copia de seguridad de los datos valiosos en el equipo.  
  
    En el siguiente ejemplo se indica cómo agregar un intervalo de dos minutos (esto no se recomienda como estado de ejecución a largo plazo): **REG ADD HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum /v PollingInterval /t REG\_DWORD /d 120000 /F**  
  
    Los valores de tiempo se expresan en milisegundos. El valor mínimo son 60 segundos, el máximo son siete día y el valor predeterminado es 24 horas.  
  
4.  Use la consola de Servicios para detener y reiniciar el servicio de registro de acceso de usuarios.  
  
## <a name="deleting-data-logged-by-ual"></a>Eliminar los datos que UAL registra  
UAL no está pensado para ser un componente crítico del sistema. Está diseñado para tener un impacto mínimo en las operaciones del sistema local, si bien manteniendo un gran nivel de confiabilidad. Esto también permite al administrador eliminar manualmente la base de datos UAL y compatibilidad con archivos (todos los archivos en el directorio \Windows\System32\LogFilesSUM\) para satisfacer las necesidades operativas.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Para eliminar los datos que UAL registra  
  
1.  Detenga el servicio de registro de acceso de usuarios  
  
2.  Abra el Explorador de Windows.  
  
3.  Vaya a **\Windows\System32\Logfiles\SUM\**.  
  
4.  Elimine todos los archivos de esa carpeta.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Administrar UAL en entornos de gran volumen  
En esta sección se explica lo que un administrador puede esperar cuando UAL se usa en un servidor con un gran volumen de clientes:  
  
El número máximo de accesos que se puede registrar con UAL es de 65.535 al día.  El uso de UAL no es aconsejable en servidores que se conecten directamente a Internet (como los servidores web que se conectan directamente a Internet) o en escenarios donde la función primordial del servidor es mantener un rendimiento extremadamente alto (como en entornos de carga de trabajo HPC). UAL se destina principalmente a pequeño, mediano y escenarios de intranet empresarial donde se espera un gran volumen, pero no tan alto como muchos implementación que sirven de volumen de tráfico a través de Internet de forma periódica.  
  
**UAL en memoria**: como UAL usa el motor de almacenamiento extensible (ESE), los requisitos de memoria que plantea irán en aumento con el tiempo (o según la cantidad de solicitudes de cliente). Sin embargo, se renunciará a la memoria, ya que el sistema la necesita para reducir el impacto en el rendimiento del sistema.  
  
**UAL en disco**: los requisitos de disco duro de UAL son más o menos los siguientes:  
  
-   0 registros de cliente único: 22 M  
  
-   50.000 registros de cliente único: 80 M  
  
-   500.000 registros de cliente único: 384 M  
  
-   1.000.000 de registros de cliente único: 729 M  
  
## <a name="recovering-from-a-corrupt-state"></a>Recuperarse de un estado dañado  
En esta sección se trata el uso de UAL del motor de almacenamiento extensible (ESE) a un alto nivel, así como lo que un administrador puede hacer si los datos de UAL están dañados o no se pueden recuperar.  
  
UAL usa ESE para optimizar el uso de los recursos del sistema y para mostrar resistencia a posibles daños.  Para obtener más información sobre las ventajas de ESE, consulte el tema sobre el [motor de almacenamiento extensible](https://msdn.microsoft.com/library/windows/desktop/gg269259(v=exchg.10).aspx) en MSDN.  
  
Cada vez que el servicio UAL se inicia, ESE realiza una recuperación de software. Para obtener más información, consulte el tema sobre los [archivos del motor de almacenamiento extensible](https://msdn.microsoft.com/library/windows/desktop/gg294069(v=exchg.10).aspx) en MSDN.  
  
Si existe un problema con la recuperación de software, ESE llevará a cabo una recuperación tras bloqueo. Para obtener más información, consulte el tema sobre la [función JetInit](https://msdn.microsoft.com/library/windows/desktop/gg294068(v=exchg.10).aspx) en MSDN.  
  
Si UAL sigue sin poder iniciarse con el conjunto de archivos de ESE existentes, se eliminarán todos los archivos del directorio \Windows\System32\LogFiles\SUM\. Tras ello, el servicio de registro de acceso de usuarios se reiniciará y se crearán archivos nuevos. Así, el servicio UAL se reanudará como si lo hiciera en un equipo recién instalado.  
  
> [!IMPORTANT]  
> Los archivos en el directorio de base de datos de UAL nunca se deben mover o modificar. De hacerlo, se iniciarían los pasos de recuperación, incluida la rutina de limpieza descrita en esta sección. En caso de que se necesiten copias de seguridad del directorio \Windows\System32\LogFiles\SUM\, se debe hacer una copia de seguridad de cada archivo contenido en este directorio para que una posible operación de restauración funcione según lo previsto.  
  
## <a name="enable-work-folders-usage-license-tracking"></a>Habilitar el seguimiento de licencias de uso de Carpetas de trabajo  
El servidor Carpetas de trabajo puede usar UAL para informar del uso del cliente. A diferencia de UAL, el registro de Carpetas de trabajo no está activado de forma predeterminada. Puede habilitarlo con el cambio de clave del Registro siguiente:  
  
```  
Reg add HKLM\Software\Microsoft\Windows\CurrentVersion\SyncShareSrv /v EnableWorkFoldersUAL /t REG_DWORD /d 1  
```  
  
Después de agregarse la clave del Registro, debe reiniciar el servicio SyncShareSvc en el servidor para habilitar el registro.  
  
Una vez habilitado el registro, se registran dos eventos informativos en el canal de Registros de Windows\Aplicación cada vez que un cliente se conecta al servidor. En Carpetas de trabajo, cada usuario puede tener uno o varios dispositivos cliente que se conectan al servidor y comprueban si hay actualizaciones de datos cada 10 minutos. Si el servidor recibe 1000 usuarios, cada uno de ellos con dos dispositivos, los registros de aplicación se sobrescribirán cada 70 minutos, lo que dificultará la solución de problemas no relacionados. Para evitar esto, puede deshabilitar temporalmente el servicio de registro de acceso de usuario, o aumentar el tamaño del canal de Windows Logs\Application del servidor.  
  
## <a name="BKMK_Links"></a>Vea también  

- [Empezar a trabajar con usuario registro de acceso](get-started-with-user-access-logging.md)
  

