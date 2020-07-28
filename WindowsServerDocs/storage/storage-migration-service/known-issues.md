---
title: Problemas conocidos del servicio de migración de almacenamiento
description: Problemas conocidos y solución de problemas para el servicio de migración de almacenamiento, como la recopilación de registros para Soporte técnico de Microsoft.
author: nedpyle
ms.author: nedpyle
manager: tiaascs
ms.date: 06/02/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: d7c76413fbc64ce200ca4c442a30e6f804927f68
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182061"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conocidos del servicio de migración de almacenamiento

Este tema contiene respuestas a problemas conocidos al usar el [servicio de migración de almacenamiento](overview.md) para migrar servidores de.

El servicio de migración de almacenamiento se publica en dos partes: el servicio en Windows Server y la interfaz de usuario en el centro de administración de Windows. El servicio está disponible en Windows Server, el canal de mantenimiento a largo plazo, así como el canal semianual de Windows Server. Aunque el centro de administración de Windows está disponible como descarga independiente. También incluimos periódicamente cambios en las actualizaciones acumulativas de Windows Server, que se publican a través de Windows Update.

Por ejemplo, Windows Server, versión 1903 incluye nuevas características y correcciones para el servicio de migración de almacenamiento, que también están disponibles para Windows Server 2019 y Windows Server, versión 1809 mediante la instalación de [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="how-to-collect-log-files-when-working-with-microsoft-support"></a><a name="collecting-logs"></a>Cómo recopilar archivos de registro al trabajar con Soporte técnico de Microsoft

El servicio de migración de almacenamiento contiene registros de eventos para el servicio Orchestrator y el servicio Proxy. El servidor de Orchestrator siempre contiene los registros de eventos y los servidores de destino con el servicio de proxy instalado contienen los registros del proxy. Estos registros se encuentran en:

- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService
- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService-proxy

Si necesita recopilar estos registros para verlos sin conexión o enviarlos a Soporte técnico de Microsoft, hay un script de PowerShell de código abierto disponible en GitHub:

 [Aplicación auxiliar de Storage Migration Service](https://aka.ms/smslogs)

Revise el archivo Léame para su uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>El servicio de migración de almacenamiento no aparece en el centro de administración de Windows, a menos que se administre Windows Server 2019

Cuando se usa la versión 1809 del centro de administración de Windows para administrar un orquestador de Windows Server 2019, no se ve la opción de la herramienta para el servicio de migración de almacenamiento.

La extensión del servicio de migración de almacenamiento del centro de administración de Windows está enlazada a la versión para administrar solo los sistemas operativos Windows Server 2019 versión 1809 o posterior. Si la usa para administrar sistemas operativos anteriores de Windows Server o versiones preliminares de Insider, la herramienta no aparecerá. Este comportamiento es así por diseño.

Para resolverlo, use o actualice a Windows Server 2019 Build 1809 o posterior.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Se produce el error "acceso denegado para la Directiva de filtro de tokens en el equipo de destino" del servicio de migración de almacenamiento

Al ejecutar la validación de traslado, recibe el error "error: acceso denegado para la Directiva de filtro de tokens en el equipo de destino". Esto ocurre incluso si proporcionó credenciales de administrador local correctas para los equipos de origen y de destino.

Este problema se corrigió en la actualización de [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) .

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>El servicio de migración de almacenamiento no se incluye en la evaluación de Windows Server 2019 o Windows Server 2019 Essentials

Al usar el centro de administración de Windows para conectarse a una [versión de evaluación de Windows server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o a la edición windows Server 2019 Essentials, no existe la opción de administrar el servicio de migración de almacenamiento. El servicio de migración de almacenamiento tampoco se incluye en roles y características.

Este problema se debe a un problema de mantenimiento en el medio de evaluación de Windows Server 2019 y Windows Server 2019 Essentials.

Para solucionar este problema para su evaluación, instale una versión de licencia por volumen, de MSDN, OEM o de Windows Server 2019 y no la active. Sin activación, todas las ediciones de Windows Server funcionan en modo de evaluación durante 180 días.

Hemos corregido este problema en una versión posterior de Windows Server.

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>El servicio de migración de almacenamiento agota el tiempo de espera de descarga del CSV de error de transferencia

Al usar el centro de administración de Windows o PowerShell para descargar el registro CSV de operaciones de transferencia de errores detallados, recibe el error:

 >   Transferir registro: Compruebe que el uso compartido de archivos está permitido en el firewall. : Esta operación de solicitud enviada a net. TCP: psico: 28940/SMS/Service/1/Transfer no recibió una respuesta dentro del tiempo de espera configurado (00:01:00). El tiempo asignado a esta operación puede ser una porción de un tiempo de espera mayor. Esto puede deberse a que el servicio todavía está procesando la operación o a que el servicio no pudo enviar un mensaje de respuesta. Considere la posibilidad de aumentar el tiempo de espera de la operación (convirtiendo el canal o el proxy a IContextChannel y estableciendo la propiedad OperationTimeout) y asegúrese de que el servicio pueda conectarse al cliente.

Este problema se debe a un gran número de archivos transferidos que no se pueden filtrar en el tiempo de espera predeterminado de un minuto permitido por el servicio de migración de almacenamiento.

Para evitar este problema:

1. En el equipo de Orchestrator, edite el archivo *% systemroot% \SMS\Microsoft.StorageMigration.Service.exe.config* con Notepad.exe para cambiar el valor predeterminado de "sendTimeout" de 1 minuto a 10 minutos.

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie el servicio "Storage Migration Service" en el equipo de Orchestrator.
3. En el equipo de Orchestrator, inicie Regedit.exe
4. Busque y haga clic en la siguiente subclave del Registro:

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. En el menú Edición, seleccione Nuevo y haga clic en Valor DWORD.
6. Escriba "WcfOperationTimeoutInMinutes" como nombre de la DWORD y, a continuación, presione Entrar.
7. Haga clic con el botón secundario en "WcfOperationTimeoutInMinutes" y, a continuación, haga clic en modificar.
8. En el cuadro datos base, haga clic en "decimal".
9. En el cuadro información del valor, escriba "10" y, a continuación, haga clic en Aceptar.
10. Salga del Editor del Registro.
11. Intente descargar el archivo CSV de solo errores de nuevo.

Tenemos previsto cambiar este comportamiento en una versión posterior de Windows Server 2019.

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Advertencias de validación para los privilegios de administrador de credenciales y proxy de destino

Al validar un trabajo de transferencia, verá las siguientes advertencias:

 > **La credencial tiene privilegios administrativos.**
 > ADVERTENCIA: la acción no está disponible de forma remota.
 > **El proxy de destino está registrado.**
 > ADVERTENCIA: no se encontró el proxy de destino.

Si no ha instalado el servicio de proxy de migración de almacenamiento en el equipo de destino de Windows Server 2019 o si el equipo de destino es Windows Server 2016 o Windows Server 2012 R2, este comportamiento es así por diseño. Se recomienda migrar a un equipo con Windows Server 2019 con el proxy instalado para mejorar significativamente el rendimiento de la transferencia.

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Determinados archivos no realizan el inventario o la transferencia, error 5 "acceso denegado"

Al realizar un inventario o transferir archivos de los equipos de origen a destino, los archivos de los que un usuario ha quitado los permisos de grupo de administradores no se pueden migrar. Examinar el servicio de migración de almacenamiento: depuración del proxy:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/26/2019 9:00:04 AM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      srv1.contoso.com
    Description:

    02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
    Stack Trace:
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()

Este problema se debe a un defecto de código en el servicio de migración de almacenamiento en el que no se invocó el privilegio de copia de seguridad.

Para resolver este problema, instale [Windows Update el 2 de abril de 2019: KB4490481 (compilación del sistema operativo 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) en el equipo de Orchestrator y en el equipo de destino si el servicio proxy está instalado allí. Asegúrese de que la cuenta de usuario de migración de origen sea un administrador local en el equipo de origen y el orquestador de servicio de migración de almacenamiento. Asegúrese de que la cuenta de usuario de migración de destino es un administrador local en el equipo de destino y el orquestador de servicio de migración de almacenamiento.

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Los hashes de DFSR no coinciden al usar el servicio de migración de almacenamiento para preinicializar datos

Al usar el servicio de migración de almacenamiento para transferir archivos a un nuevo destino, la configuración de Replicación DFS para replicar los datos con un servidor existente a través de la replicación preiniciada o la clonación de la base de datos Replicación DFS, todos los archivos experimentan un error de coincidencia de hash y se vuelven a replicar. Parece que todos los flujos de datos, flujos de seguridad, tamaños y atributos están perfectamente coincidentes después de usar el servicio de migración de almacenamiento para transferirlos. El examen de los archivos con ICACLS o el registro de depuración de la clonación de base de datos Replicación DFS revela:

Archivo de origen:

  d:\test\Source icacls:

  icacls d:\test\thatcher.png/Save out.txt/t thatcher.png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; Bu

Archivo de destino:

  icacls d:\test\thatcher.png/Save out.txt/t thatcher.png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S: PAINO_ACCESS_CONTROL**

Registro de depuración de DFSR:

    20190308 10:18:53.116 3948 DBCL  4045 [WARN] DBClone::IDTableImportUpdate Mismatch record was found.

    Local ACL hash:1BCDFE03-A18BCE01-D1AE9859-23A0A5F6
    LastWriteTime:20190308 18:09:44.876
    FileSizeLow:1131654
    FileSizeHigh:0
    Attributes:32

    Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B**
    LastWriteTime:20190308 18:09:44.876
    FileSizeLow:1131654
    FileSizeHigh:0
    Attributes:32

Este problema se ha corregido con la actualización de [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Error "no se pudo transferir el almacenamiento en ninguno de los puntos de conexión" al transferir desde Windows Server 2008 R2

Al intentar transferir datos de un equipo de origen de Windows Server 2008 R2, no se transfiere ninguna transferencia de datos y recibe el error:

    Couldn't transfer storage on any of the endpoints.
    0x9044

Este error se espera si el equipo con Windows Server 2008 R2 no se ha revisado por completo con todas las actualizaciones críticas e importantes de Windows Update. Independientemente del servicio de migración de almacenamiento, siempre se recomienda aplicar una revisión a un equipo con Windows Server 2008 R2 por motivos de seguridad, ya que ese sistema operativo no contiene las mejoras de seguridad de las versiones más recientes de Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Error "no se pudo transferir el almacenamiento en ninguno de los puntos de conexión" y "comprobar si el dispositivo de origen está en línea-no se pudo obtener acceso a él".

Al intentar transferir datos de un equipo de origen, algunos o todos los recursos compartidos no se transfieren, con un error de Resumen:

    Couldn't transfer storage on any of the endpoints.
    0x9044

Al examinar los detalles de la transferencia de SMB se muestra el error:

    Check if the source device is online - we couldn't access it.

En el examen del registro de eventos StorageMigrationService/admin se muestra:

    Couldn't transfer storage.

    Job: Job1
    ID:
    State: Failed
    Error: 36931
    Error Message:

   Guía: Compruebe el error detallado y asegúrese de que se cumplen los requisitos de transferencia. El trabajo de transferencia no pudo transferir ningún equipo de origen y de destino. Esto puede deberse a que el equipo de Orchestrator no pudo acceder a los equipos de origen o de destino, posiblemente debido a una regla de firewall, o a los permisos que faltan.

El examen del registro StorageMigrationService-proxy/Debug muestra:

    07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)

Se trata de un defecto de código que se manifestaría si la cuenta de migración no tiene al menos permisos de lectura para los recursos compartidos de SMB. Este problema se corrigió por primera vez en la actualización acumulativa [4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062).

## <a name="error-0x80005000-when-running-inventory"></a>Error 0x80005000 al ejecutar el inventario

Después de instalar [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) e intentar ejecutar el inventario, se produce un error en el inventario con errores:

    EXCEPTION FROM HRESULT: 0x80005000

    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2503
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory the computers.
    Job: foo2
    ID: 20ac3f75-4945-41d1-9a79-d11dbb57798b
    State: Failed
    Error: 36934
    Error Message: Inventory failed for all devices
    Guidance: Check the detailed error and make sure the inventory requirements are met. The job couldn't inventory any of the specified source computers. This could be because the orchestrator computer couldn't reach it over the network, possibly due to a firewall rule or missing permissions.

    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2509
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory a computer.
    Job: foo2
    Computer: FS01.TailwindTraders.net
    State: Failed
    Error: -2147463168
    Error Message:
    Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/14/2020 1:18:21 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      2019-rtm-orc.ned.contoso.com
    Description:
    02/14/2020-13:18:21.097 [Erro] Failed device discovery stage SystemInfo with error: (0x80005000) Unknown error (0x80005000)

Este error se debe a un defecto de código en el servicio de migración de almacenamiento cuando se proporcionan credenciales de migración en forma de nombre principal de usuario (UPN), como ' meghan@contoso.com '. El servicio del orquestador de servicio de migración de almacenamiento no puede analizar correctamente este formato, lo que conduce a un error en una búsqueda de dominio que se agregó para la compatibilidad con la migración de clústeres en KB4512534 y 19H1.

Para solucionar este problema, proporcione credenciales con el formato dominio\usuario, como "Contoso\Meghan".

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Error "ServiceError0x9006" o "el proxy no está disponible actualmente". al migrar a un clúster de conmutación por error de Windows Server

Al intentar transferir datos a un servidor de archivos en clúster, recibirá errores como:

    Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
    0x9006
    ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand

Este error se espera si el recurso del servidor de archivos ha pasado del nodo propietario del clúster original de Windows Server 2019 a un nuevo nodo y la característica de proxy del servicio de migración de almacenamiento no estaba instalada en ese nodo.

Como solución alternativa, mueva el recurso del servidor de archivos de destino de nuevo al nodo de clúster de propietario original que estaba en uso la primera vez que configuró emparejamientos de transferencia.

Como alternativa alternativa:

1. Instale la característica proxy de servicio de migración de almacenamiento en todos los nodos de un clúster.
2. Ejecute el siguiente comando de PowerShell del servicio de migración de almacenamiento en el equipo de Orchestrator:

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Error "no se encontró el archivo dll" al ejecutar el inventario desde un nodo de clúster

Al intentar ejecutar el inventario con el servicio de migración de almacenamiento y establecer como destino un clúster de conmutación por error de Windows Server general usar el origen del servidor de archivos, recibirá los siguientes errores:

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)

Para solucionar este problema, instale las "herramientas de administración del clúster de conmutación por error" (RSAT-clustering-MGMT) en el servidor que ejecuta el orquestador del servicio de migración de almacenamiento.

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>Error "no hay más extremos disponibles desde el asignador de extremos" al ejecutar el inventario en un equipo de origen de Windows Server 2003

Al intentar ejecutar el inventario con el orquestador del servicio de migración de almacenamiento en un equipo de origen de Windows Server 2003, recibirá el siguiente error:

    There are no more endpoints available from the endpoint mapper

Este problema se resuelve con la actualización de [KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818) .

## <a name="uninstalling-a-cumulative-update-prevents-storage-migration-service-from-starting"></a>La desinstalación de una actualización acumulativa impide que se inicie el servicio de migración de almacenamiento

La desinstalación de las actualizaciones acumulativas de Windows Server puede impedir que se inicie el servicio de migración de almacenamiento. Para resolver este problema, puede realizar una copia de seguridad y eliminar la base de datos del servicio de migración de almacenamiento:

1.  Abra un símbolo del sistema con privilegios elevados, donde sea miembro de los administradores en el servidor de servicio de migración de almacenamiento y ejecute:

     ```
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService

     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```

2.  Inicie el servicio de migración de almacenamiento, que creará una nueva base de datos.

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>Error "CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO error en el recurso de dirección de servidor" y se produce un error en la transferencia del clúster de Windows Server 2008 R2

Al intentar ejecutar el corte en un origen de clúster de Windows Server 2008 R2, el cambio se bloquea en la fase "cambiar el nombre del equipo de origen..." y recibe el siguiente error:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

Este problema se debe a que falta una API en versiones anteriores de Windows Server. Actualmente no hay ninguna manera de migrar los clústeres de Windows Server 2008 y Windows Server 2003. Puede realizar el inventario y la transferencia sin problemas en los clústeres de Windows Server 2008 R2 y, a continuación, realizar manualmente el traslado mediante el cambio manual de la dirección IP y el servidor de archivos de origen del clúster y, después, cambiar la dirección IP y el número de direcciones del clúster de destino para que coincida con el origen original.

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-static-ips"></a>El total de bloqueos en "38% está asignando interfaces de red en el equipo de origen..." al usar direcciones IP estáticas

Al intentar ejecutar el recorte de un equipo de origen, si establece que el equipo de origen use una nueva dirección IP estática (no DHCP) en una o varias interfaces de red, el cambio se bloqueará en la fase "38% de asignación de interfaces de red en el equipo de origen..." y recibe el siguiente error en el registro de eventos del servicio de migración de almacenamiento:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          11/13/2019 3:47:06 PM
    Event ID:      20494
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      orc2019-rtm.corp.contoso.com
    Description:
    Couldn't set the IP address on the network adapter.

    Computer: fs12.corp.contoso.com
    Adapter: microsoft hyper-v network adapter
    IP address: 10.0.0.99
    Network mask: 16
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

Al examinar el equipo de origen, se muestra que no se puede cambiar la dirección IP original.

Este problema no se produce si seleccionó "usar DHCP" en la pantalla "configurar el traslado" del centro de administración de Windows, solo si especifica una nueva dirección IP estática.

Hay dos soluciones para este problema:

1. Este problema se resolvió por primera vez con la actualización de [KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818) . El defecto de código anterior evitó el uso de direcciones IP estáticas.

2. Si no ha especificado una dirección IP de puerta de enlace predeterminada en las interfaces de red del equipo de origen, este problema se producirá incluso con la actualización de KB4537818. Para solucionar este problema, establezca una dirección IP predeterminada válida en las interfaces de red mediante el applet de conexiones de red (NCPA.CPL) o el cmdlet de PowerShell Set-NetRoute.

## <a name="slower-than-expected-re-transfer-performance"></a>Rendimiento más lento que el rendimiento de retransferencia esperado

Después de completar una transferencia y, a continuación, ejecutar una retransferencia posterior de los mismos datos, es posible que no vea muchas mejoras en el tiempo de transferencia aunque haya pocos datos que hayan cambiado mientras tanto en el servidor de origen.

Este es el comportamiento esperado cuando se transfiere un número muy grande de archivos y carpetas anidadas. El tamaño de los datos no es relevante. Primero hemos realizado mejoras en este comportamiento en [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) y continúan optimizando el rendimiento de la transferencia. Para optimizar aún más el rendimiento, revise [optimización del inventario y rendimiento](./faq.md#optimizing-inventory-and-transfer-performance)de la transferencia.

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>Los datos no se transfieren, el usuario cambia el nombre al migrar a o desde un controlador de dominio.

Después de iniciar la transferencia desde o a un controlador de dominio:

 1. No se migra ningún dato y no se crean recursos compartidos en el destino.
 2. Aparece un símbolo de error rojo en el centro de administración de Windows sin ningún mensaje de error
 3. Uno o varios usuarios de AD y grupos locales de dominio tienen su nombre y/o el atributo de inicio de sesión anterior a Windows 2000 cambiado
 4. Verá el evento 3509 en el orquestador del servicio de migración de almacenamiento:

        Log Name:      Microsoft-Windows-StorageMigrationService/Admin
        Source:        Microsoft-Windows-StorageMigrationService
        Date:          1/10/2020 2:53:48 PM
        Event ID:      3509
        Task Category: None
        Level:         Error
        Keywords:
        User:          NETWORK SERVICE
        Computer:      orc2019-rtm.corp.contoso.com
        Description:
        Couldn't transfer storage for a computer.

        Job: dctest3
        Computer: dc02-2019.corp.contoso.com
        Destination Computer: dc03-2019.corp.contoso.com
        State: Failed
        Error: 53251
        Error Message: Local accounts migration failed with error System.Exception: -2147467259
           at Microsoft.StorageMigration.Service.DeviceHelper.MigrateSecurity(IDeviceRecord sourceDeviceRecord, IDeviceRecord destinationDeviceRecord, TransferConfiguration config, Guid proxyId, CancellationToken cancelToken)

Este es el comportamiento esperado si intenta migrar desde o a un controlador de dominio con el servicio de migración de almacenamiento y usó la opción "migrar usuarios y grupos" para cambiar el nombre o volver a usar cuentas. en lugar de seleccionar "no transferir usuarios y grupos". No se admite la migración [de DC con el servicio de migración de almacenamiento](faq.md). Dado que un controlador de dominio no tiene usuarios y grupos locales verdaderos, el servicio de migración de almacenamiento trata a estas entidades de seguridad como si se realizara la migración entre dos servidores miembro e intenta ajustar las ACL como se indicó, lo que conduce a los errores y a las cuentas alteradas o copiadas.

Si ya ha ejecutado la transferencia una y varias veces:

 1. Use el siguiente comando de PowerShell de AD en un controlador de dominio para buscar los usuarios o grupos modificados (cambiando SearchBase para que coincida con su nombre distintivo de dominio):

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```

 2. En el caso de los usuarios devueltos con su nombre original, edite el "nombre de inicio de sesión de usuario (anterior a Windows 2000)" para quitar el sufijo de carácter aleatorio agregado por el servicio de migración de almacenamiento, de modo que este usuario pueda iniciar sesión.
 3. En el caso de los grupos devueltos con su nombre original, edite el "nombre de grupo (anterior a Windows 2000)" para quitar el sufijo de carácter aleatorio agregado por el servicio de migración de almacenamiento.
 4. En el caso de los usuarios o grupos deshabilitados con nombres que ahora contengan un sufijo agregado por el servicio de migración de almacenamiento, puede eliminar estas cuentas. Puede confirmar que las cuentas de usuario se agregaron posteriormente porque solo contendrán el grupo usuarios del dominio y tendrá una fecha y hora de creación que coincidan con la hora de inicio de la transferencia del servicio de migración de almacenamiento.

 Si desea usar el servicio de migración de almacenamiento con controladores de dominio para la transferencia, asegúrese de seleccionar siempre "no transferir usuarios y grupos" en la página Configuración de transferencia del centro de administración de Windows.

## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>Error 53, "no se pudieron inventariar todos los dispositivos especificados" al ejecutar el inventario.

Al intentar ejecutar el inventario, recibirá lo siguiente:

    Failed to inventory all specified devices

    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          1/16/2020 8:31:17 AM
    Event ID:      2516
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    Couldn't inventory files on the specified endpoint.
    Job: ned1
    Computer: ned.corp.contoso.com
    Endpoint: hithere
    State: Failed
    File Count: 0
    File Size in KB: 0
    Error: 53
    Error Message: Endpoint scan failed
    Guidance: Check the detailed error and make sure the inventory requirements are met. This could be because of missing permissions on the source computer.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/16/2020 8:31:17 AM
    Event ID:      10004
    Task Category: None
    Level:         Critical
    Keywords:
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    01/16/2020-08:31:17.031 [Crit] Consumer Task failed with error:The network path was not found.
    . StackTrace=   at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)
       at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetEnvironmentPathFolders(String ServerName, Boolean IsServerLocal)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.ScanUtils.<ScanSMBEndpoint>d__3.MoveNext()
       at Microsoft.StorageMigration.Proxy.EndpointScanOperation.Run()
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(EndpointScanRequest scanRequest, Guid operationId)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(Object request)
       at Microsoft.StorageMigration.Proxy.Common.ProducerConsumerManager`3.Consume(CancellationToken token)

    01/16/2020-08:31:10.015 [Erro] Endpoint Scan failed. Error: (53) The network path was not found.
    Stack trace:
       at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)

En esta fase, el orquestador del servicio de migración de almacenamiento está intentando lecturas remotas del registro para determinar la configuración de la máquina de origen, pero el servidor de origen rechaza la ruta de acceso del registro. Esto se puede producir por:

 - El servicio de registro remoto no se está ejecutando en el equipo de origen.
 - el Firewall no permite conexiones remotas del registro con el servidor de origen desde el orquestador.
 - La cuenta de migración de origen no tiene permisos de registro remoto para conectarse al equipo de origen.
 - La cuenta de migración de origen no tiene permisos de lectura en el registro del equipo de origen, en "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\CurrentVersion" o en "HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanServer"

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>El total de bloqueos en "38% está asignando interfaces de red en el equipo de origen..."

Al intentar ejecutar el recorte de un equipo de origen, el cambio se detiene en la fase "38% de la asignación de interfaces de red en el equipo de origen..." y recibe el siguiente error en el registro de eventos del servicio de migración de almacenamiento:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/11/2020 8:51:14 AM
    Event ID:      20505
    Task Category: None
    Level:         Error
    Keywords:
    User:          NETWORK SERVICE
    Computer:      nedwardo.contosocom
    Description:
    Couldn't establish a CIM session with the computer.

    Computer: 172.16.10.37
    User Name: nedwardo\MsftSmsStorMigratSvc
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

Este problema se debe a directiva de grupo que establece el siguiente valor del registro en el equipo de origen:

 "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System LocalAccountTokenFilterPolicy = 0"

Esta configuración no forma parte de la directiva de grupo estándar, sino que es un complemento configurado mediante el [Kit de herramientas de cumplimiento de seguridad de Microsoft](https://www.microsoft.com/download/details.aspx?id=55319):

 - Windows Server 2012 R2: "configuración del Equipo\plantillas Templates\SCM: pasar las restricciones de UAC Mitigations\Apply de hash a las cuentas locales en los inicios de sesión de red"
 - Viudas Server 2016: "configuración del Equipo\plantillas Templates\MS de seguridad de Guide\Apply de UAC para cuentas locales en inicios de sesión de red"

También se puede establecer mediante preferencias de directiva de grupo con una configuración de registro personalizada. Puede usar la herramienta GPRESULT para determinar qué directiva está aplicando esta configuración en el equipo de origen.

El servicio de migración de almacenamiento habilita temporalmente [LocalAccountTokenFilterPolicy](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows) como parte del proceso de cortar y, a continuación, lo quita cuando ha terminado. Cuando directiva de grupo aplica un objeto de directiva de grupo en conflicto (GPO), invalida el servicio de migración de almacenamiento e impide el cambio.

Para solucionar este problema, use una de las siguientes opciones:

1. Mueva temporalmente el equipo de origen desde la unidad organizativa Active Directory que aplica este GPO en conflicto.
2. Deshabilitar temporalmente el GPO que aplica esta directiva en conflicto.
3. Cree temporalmente un nuevo GPO que establezca esta opción en deshabilitado y se aplique a una unidad organizativa específica de servidores de origen, con una prioridad más alta que cualquier otro GPO.

## <a name="inventory-or-transfer-fail-when-using-credentials-from-a-different-domain"></a>Error de inventario o transferencia al usar credenciales de un dominio diferente

Al intentar ejecutar el inventario o la transferencia con el servicio de migración de almacenamiento y establecer como destino un servidor de Windows al utilizar las credenciales de migración de un dominio diferente al del servidor de destino, recibirá uno o varios de los siguientes errores:

    Exception from HRESULT:0x80131505

    The server was unable to process the request due to an internal error

    04/28/2020-11:31:01.169 [Error] Failed device discovery stage SystemInfo with error: (0x490) Could not find computer object 'myserver' in Active Directory    [d:\os\src\base\dms\proxy\discovery\discoveryproxy\DeviceDiscoveryOperation.cs::TryStage::1042]

El examen de los registros muestra además que la cuenta de migración y el servidor que se va a migrar desde o dos se encuentran en dominios diferentes:

    ```
    06/25/2020-10:11:16.543 [Info] Creating new job=NedJob user=**CONTOSO**\ned    
    [d:\os\src\base\dms\service\StorageMigrationService.IInventory.cs::CreateJob::133]
    ```
    
    GetOsVersion(fileserver75.**corp**.contoso.com)    [d:\os\src\base\dms\proxy\common\proxycommon\CimSessionHelper.cs::GetOsVersion::66]
06/25/2020-10:20:45.368 [info] equipo ' fileserver75.corp.contoso.com ': versión del sistema operativo 

Este problema se debe a un defecto de código en el servicio de migración de almacenamiento. Para solucionar este problema, use las credenciales de migración del mismo dominio al que pertenecen los equipos de origen y de destino. Por ejemplo, si el equipo de origen y el de destino pertenecen al dominio "corp.contoso.com" del bosque "contoso.com", use "corp\myaccount" para realizar la migración, no una credencial "contoso\myaccount".

## <a name="see-also"></a>Consulte también

- [Información general del servicio de migración de almacenamiento](overview.md)
