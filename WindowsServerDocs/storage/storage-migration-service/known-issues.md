---
title: Problemas conocidos del servicio de migración de almacenamiento
description: Problemas conocidos y solución de problemas para el servicio de migración de almacenamiento, como la recopilación de registros para Soporte técnico de Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: efd92e9f6a199ad901e95b18718f3b448c3207e2
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560586"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conocidos del servicio de migración de almacenamiento

Este tema contiene respuestas a problemas conocidos al usar el [servicio de migración de almacenamiento](overview.md) para migrar servidores de.

## <a name="collecting-logs"></a>Cómo recopilar archivos de registro al trabajar con Soporte técnico de Microsoft

El servicio de migración de almacenamiento contiene registros de eventos para el servicio Orchestrator y el servicio Proxy. El servidor de urchestrator siempre contiene los registros de eventos y los servidores de destino con el servicio de proxy instalado contienen los registros del proxy. Estos registros se encuentran en:

- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService
- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService-proxy

Si necesita recopilar estos registros para verlos sin conexión o enviarlos a Soporte técnico de Microsoft, hay un script de PowerShell de código abierto disponible en GitHub:

 [Aplicación auxiliar de Storage Migration Service](https://aka.ms/smslogs) 

Revise el archivo Léame para su uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>El servicio de migración de almacenamiento no aparece en el centro de administración de Windows, a menos que se administre Windows Server 2019

Cuando se usa la versión 1809 del centro de administración de Windows para administrar un orquestador de Windows Server 2019, no se ve la opción de la herramienta para el servicio de migración de almacenamiento. 

La extensión del servicio de migración de almacenamiento del centro de administración de Windows está enlazada a la versión para administrar solo los sistemas operativos Windows Server 2019 versión 1809 o posterior. Si la usa para administrar sistemas operativos anteriores de Windows Server o versiones preliminares de Insider, la herramienta no aparecerá. Este comportamiento es así por diseño. 

Para resolverlo, use o actualice a Windows Server 2019 Build 1809 o posterior.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>El servicio de migración de almacenamiento no permite elegir una dirección IP estática en el traslado

Cuando se usa la versión 0,57 de la extensión del servicio de migración de almacenamiento en el centro de administración de Windows y se alcanza la fase de traslado, no se puede seleccionar una dirección IP estática para una dirección. Se le obligará a usar DHCP.

Para resolver este problema, en el centro de administración de Windows, busque el tema sobre**extensiones** de **configuración** > para obtener una alerta que indique que la versión actualizada del servicio de migración de almacenamiento de 0.57.2 está disponible para instalarse. Es posible que tenga que reiniciar la pestaña del explorador para el centro de administración de Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Se produce el error "acceso denegado para la Directiva de filtro de tokens en el equipo de destino" del servicio de migración de almacenamiento

Al ejecutar la validación de traslado, recibe el error "FAIL: Se denegó el acceso a la Directiva de filtro de tokens en el equipo de destino. " Esto ocurre incluso si proporcionó credenciales de administrador local correctas para los equipos de origen y de destino.

Este problema se debe a un defecto de código en Windows Server 2019. El problema se produce cuando se usa el equipo de destino como un orquestador de servicio de migración de almacenamiento.

Para solucionar este problema, instale el servicio de migración de almacenamiento en un equipo con Windows Server 2019 que no sea el destino de la migración previsto y, a continuación, conéctese a ese servidor con el centro de administración de Windows y realice la migración.

Lo hemos corregido en una versión posterior de Windows Server. Abra un caso de soporte técnico a través de [soporte técnico de Microsoft](https://support.microsoft.com) para solicitar que se cree un repuerto de esta corrección.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>El servicio de migración de almacenamiento no se incluye en la evaluación de Windows Server 2019 o Windows Server 2019 Essentials

Al usar el centro de administración de Windows para conectarse a una [versión de evaluación de Windows server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o a la edición windows Server 2019 Essentials, no existe la opción de administrar el servicio de migración de almacenamiento. El servicio de migración de almacenamiento tampoco se incluye en roles y características.

Este problema se debe a un problema de mantenimiento en el medio de evaluación de Windows Server 2019 y Windows Server 2019 Essentials. 

Para solucionar este problema para su evaluación, instale una versión de licencia por volumen, de MSDN, OEM o de Windows Server 2019 y no la active. Sin activación, todas las ediciones de Windows Server funcionan en modo de evaluación durante 180 días. 

Hemos corregido este problema en una versión posterior de Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>El servicio de migración de almacenamiento agota el tiempo de espera de descarga del CSV de error de transferencia

Al usar el centro de administración de Windows o PowerShell para descargar el registro CSV de operaciones de transferencia de errores detallados, recibe el error:

 >   Transferir registro: Compruebe que el uso compartido de archivos está permitido en el firewall. : Esta operación de solicitud enviada a net. TCP: psico: 28940/SMS/Service/1/Transfer no recibió una respuesta dentro del tiempo de espera configurado (00:01:00). El tiempo asignado a esta operación puede haber sido una parte de un tiempo de espera mayor. Esto puede deberse a que el servicio sigue procesando la operación o a que el servicio no pudo enviar un mensaje de respuesta. Considere la posibilidad de aumentar el tiempo de espera de la operación (convirtiendo el canal o el proxy a IContextChannel y estableciendo la propiedad OperationTimeout) y asegúrese de que el servicio pueda conectarse al cliente.

Este problema se debe a un gran número de archivos transferidos que no se pueden filtrar en el tiempo de espera predeterminado de un minuto permitido por el servicio de migración de almacenamiento. 

Para solucionar este problema:

1. En el equipo de Orchestrator, edite el archivo *%systemroot%\SMS\Microsoft.StorageMigration.Service.exe.config* con Notepad. exe para cambiar el valor de "sendTimeout" de su valor predeterminado de 1 minuto a 10 minutos.

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie el servicio "Storage Migration Service" en el equipo de Orchestrator. 
3. En el equipo de Orchestrator, inicie regedit. exe.
4. Busque y haga clic en la siguiente subclave del Registro: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. En el menú Edición, seleccione Nuevo y haga clic en Valor DWORD. 
6. Escriba "WcfOperationTimeoutInMinutes" como nombre de la DWORD y, a continuación, presione Entrar.
7. Haga clic con el botón secundario en "WcfOperationTimeoutInMinutes" y, a continuación, haga clic en modificar. 
8. En el cuadro datos base, haga clic en "decimal".
9. En el cuadro información del valor, escriba "10" y, a continuación, haga clic en Aceptar.
10. Salga del editor del registro.
11. Intente descargar el archivo CSV de solo errores de nuevo. 

Tenemos previsto cambiar este comportamiento en una versión posterior de Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Se produce un error de total al migrar entre redes

Al migrar a un equipo de destino que ejecuta en una red diferente que el origen, como una instancia de Azure IaaS, no se puede completar la transferencia cuando el origen estaba usando una dirección IP estática. 

Este comportamiento es así por diseño, para evitar problemas de conectividad después de la migración de los usuarios, las aplicaciones y los scripts que se conectan a través de la dirección IP. Cuando la dirección IP se mueve del equipo de origen anterior al nuevo destino de destino, no coincidirá con la nueva información de subred de red y quizás con DNS y WINS.

Para solucionar este problema, realice una migración a un equipo en la misma red. A continuación, mueva el equipo a una nueva red y reasigne su información de IP. Por ejemplo, si va a migrar a IaaS de Azure, migre primero a una máquina virtual local y luego use Azure Migrate para desplazar la máquina virtual a Azure.  

Hemos corregido este problema en una versión posterior del centro de administración de Windows. Ahora le permitiremos especificar migraciones que no modifiquen la configuración de red del servidor de destino. La extensión actualizada se mostrará aquí cuando se lance. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Advertencias de validación para los privilegios de administrador de credenciales y proxy de destino

Al validar un trabajo de transferencia, verá las siguientes advertencias:

 > **La credencial tiene privilegios administrativos.**
 > Advertencia: La acción no está disponible de forma remota.
 > **El proxy de destino está registrado.**
 > Advertencia: No se encontró el proxy de destino.

Si no ha instalado el servicio de proxy de migración de almacenamiento en el equipo de destino de Windows Server 2019 o si el equipo destino es Windows Server 2016 o Windows Server 2012 R2, este comportamiento es así por diseño. Se recomienda migrar a un equipo con Windows Server 2019 con el proxy instalado para mejorar significativamente el rendimiento de la transferencia.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Determinados archivos no realizan el inventario o la transferencia, error 5 "acceso denegado"

Al realizar un inventario o transferir archivos de los equipos de origen a destino, los archivos de los que un usuario ha quitado los permisos de grupo de administradores no se pueden migrar. Examinar el servicio de migración de almacenamiento: depuración del proxy:

  Nombre de registro:      Microsoft-Windows-StorageMigrationService-proxy/origen de depuración:        Microsoft-Windows-StorageMigrationService-proxy fecha:          2/26/2019 9:00:04 AM ID. de evento:      10000 categoría de tareas: Nivel ninguno:         Palabras clave de error:      
  Usuario:          Equipo de servicio de red: Descripción de srv1.contoso.com:

  02/26/2019-09:00:04.860 [paso] error de transferencia \\de SRV1. contoso. com\public\indy.png: (5) se denegó el acceso.
Seguimiento de la pila: en Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) at Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. GetTargetFile (ruta de acceso de cadena) en Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. GetTargetFile (archivo FileInfo) en Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. InitializeSourceFileInfo () en Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. Transfer () en Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. TryTransfer () [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs:: TryTransfer:: 55]


Este problema se debe a un defecto de código en el servicio de migración de almacenamiento en el que no se invocó el privilegio de copia de seguridad. 

Para resolver este problema, instale [Windows Update el 2 de abril de 2019: KB4490481 (compilación del sistema operativo 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) en el equipo de Orchestrator y en el equipo de destino si el servicio proxy está instalado allí. Asegúrese de que la cuenta de usuario de migración de origen sea un administrador local en el equipo de origen y el orquestador de servicio de migración de almacenamiento. Asegúrese de que la cuenta de usuario de migración de destino es un administrador local en el equipo de destino y el orquestador de servicio de migración de almacenamiento. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Los hashes de DFSR no coinciden al usar el servicio de migración de almacenamiento para preinicializar datos

Al usar el servicio de migración de almacenamiento para transferir archivos a un nuevo destino, configurando el Replicación DFS (DFSR) para replicar los datos con un servidor DFSR existente a través de la replicación presionada o la clonación de la base de datos DFSR, todos los archivos experiemce un hash no coinciden y se vuelven a replicar. Parece que todos los flujos de datos, secuencias de seguridad, tamaños y atributos están exactamente coincidentes después de usar SMS para transferirlos. El examen de los archivos con ICACLS o el registro de depuración de la clonación de la base de datos de DFSR revela:

Archivo de origen:

  d:\test\Source icacls:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; Bu

Archivo de destino:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S:PAINO_ACCESS_CONTROL**

Registro de depuración de DFSR:

  20190308 10:18:53.116 3948 DBCL 4045 [WARN] DBClone:: IDTableImportUpdate se encontró un registro no coincidente. 

  Hash de ACL local: 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Atributos: 32 

  Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Atributos: 32 

Este problema se debe a un defecto de código en una biblioteca utilizada por el servicio de migración de almacenamiento para establecer las ACL de auditoría de seguridad (SACL). Una SACL no nula se establece sin querer cuando la SACL estaba vacía, lo que hizo que DFSR identificara correctamente una discrepancia de hash. 

Para solucionar este problema, siga usando Robocopy para [las operaciones de clonación previa de DFSR y clonación de base de datos de DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) en lugar del servicio de migración de almacenamiento. Estamos investigando este problema y estamos pensando en resolverlo en una versión posterior de Windows Server y, posiblemente, en una Windows Update de migración. 

## <a name="error-404-when-downloading-csv-logs"></a>Error 404 al descargar registros CSV

Al intentar descargar los registros de transferencia o de errores al final de una operación de transferencia, recibe el error:

  $jobname: Registro de transferencia: error de Ajax 404

Este error se espera si no ha habilitado la regla de firewall "uso compartido de archivos e impresoras (SMB-in)" en el servidor de Orchestrator. Las descargas de archivos del centro de administración de Windows requieren el puerto TCP/445 (SMB) en los equipos conectados.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>Error "no se pudo transferir el almacenamiento en ninguno de los puntos de conexión" al transferir desde Windows Server 2008 R2

Al intentar transferir datos de un equipo de origen de Windows Server 2008 R2, no se transfiere ninguna transferencia de datos y recibe el error:  

  No se pudo transferir el almacenamiento en ninguno de los puntos de conexión.
0x9044

Este error se espera si el equipo con Windows Server 2008 R2 no se ha revisado por completo con todas las actualizaciones críticas e importantes de Windows Update. Independientemente del servicio de migración de almacenamiento, siempre se recomienda aplicar una revisión a un equipo con Windows Server 2008 R2 por motivos de seguridad, ya que ese sistema operativo no contiene las mejoras de seguridad de las versiones más recientes de Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Error "no se pudo transferir el almacenamiento en ninguno de los puntos de conexión" y "comprobar si el dispositivo de origen está en línea-no se pudo obtener acceso a él".

Al intentar transferir datos de un equipo de origen, algunos o todos los recursos compartidos no se transfieren, con un error de Resumen:

   No se pudo transferir el almacenamiento en ninguno de los puntos de conexión.
0x9044

Al examinar los detalles de la transferencia de SMB se muestra el error:

   Compruebe si el dispositivo de origen está en línea; no se pudo acceder a él.

En el examen del registro de eventos StorageMigrationService/admin se muestra:

   No se pudo transferir el almacenamiento.

   Trabajo IDENTIFICADOR de Job1:  
   Estado: Error: Mensaje de error 36931: 

   Orientación: Compruebe el error detallado y asegúrese de que se cumplen los requisitos de transferencia. El trabajo de transferencia no pudo transferir ningún equipo de origen y de destino. Esto puede deberse a que el equipo de Orchestrator no pudo acceder a los equipos de origen o de destino, posiblemente debido a una regla de firewall, o a los permisos que faltan.

El examen del registro StorageMigrationService-proxy/Debug muestra:

   07/02/2019-13:35:57.231 [error] no se pudo realizar la validación de transferencia. ErrorCode 40961, el punto de conexión de origen no es accesible o no existe, las credenciales de origen no son válidas o el usuario autenticado no tiene permisos suficientes para acceder a ella.
en Microsoft. StorageMigration. proxy. Service. Transfer. TransferOperation. Validate () en Microsoft. StorageMigration. proxy. Service. Transfer. TransferRequestHandler. ProcessRequest (FileTransferRequest fileTransferRequest, GUID operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Este error se espera si la cuenta de migración no tiene al menos permisos de acceso de lectura para los recursos compartidos de SMB. Para solucionar este error, agregue un grupo de seguridad que contenga la cuenta de migración de origen a los recursos compartidos de SMB en el equipo de origen y concédale la lectura, el cambio o el control total. Una vez finalizada la migración, puede quitar este grupo. Una versión futura de Windows Server puede cambiar este comportamiento para ya no requerir permisos explícitos para los recursos compartidos de origen.

## <a name="see-also"></a>Vea también

- [Información general del servicio de migración de almacenamiento](overview.md)
