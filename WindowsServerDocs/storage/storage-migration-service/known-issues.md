---
title: Servicio de migración de almacenamiento problemas conocidos
description: Problemas conocidos y solución de problemas de soporte técnico para servicio de migración de almacenamiento, como cómo recopilar registros de Microsoft Support.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 05/14/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: e1cfd2b0ea3bc4d7802cb4a6d2a8c1493d5511a1
ms.sourcegitcommit: 0099873d69bd23495d275d7bcb464594de09ee3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65699695"
---
# <a name="storage-migration-service-known-issues"></a>Servicio de migración de almacenamiento problemas conocidos

Este tema contiene respuestas a problemas conocidos al usar [Storage Migration Service](overview.md) para migrar los servidores.

## <a name="collecting-logs"></a> Cómo recopilar los archivos de registro cuando se trabaja con Microsoft Support

El servicio de migración de almacenamiento contiene los registros de eventos para el servicio de Orchestrator y el servicio de Proxy. El servidor urchestrator siempre contiene dos registros de eventos y los servidores de destino con el servicio de proxy instalado contienen los registros de proxy. Estos registros se encuentran en:

- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService
- Registros de aplicaciones y servicios \ Microsoft \ Windows \ StorageMigrationService Proxy

Si necesita recopilar estos registros para visualizarlo sin conexión o enviar a Microsoft Support, hay un script de PowerShell está disponible en GitHub de código abierto:

 [Aplicación auxiliar de servicio de migración de almacenamiento](https://aka.ms/smslogs) 

Revise el archivo Léame de uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Servicio de migración de almacenamiento no aparece en Windows Admin Center, a menos que la administración de Windows Server 2019

Cuando se usa la versión 1809 de Windows Admin Center para administrar un orquestador de 2019 de Windows Server, no ve la opción de herramienta para el servicio de migración de almacenamiento. 

La extensión de servicio de migración de almacenamiento de Windows Admin Center está enlazada a versión para administrar solo la versión de Windows Server 2019 1809 o sistemas operativos posteriores. Si desea usarla para administrar sistemas operativos de Windows Server anteriores o vistas previas de insider, la herramienta no aparecerá. Este comportamiento es así por diseño. 

Para resolver, use o actualice a Windows Server 2019 compilación 1809 o posterior.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Servicio de migración de almacenamiento no le permiten elegir dirección IP estática en el traslado

Cuando se usa la versión 0.57 de extensión en Windows Admin Center y llegar a la fase de puesta en marcha el servicio de migración de almacenamiento, no puede seleccionar una dirección IP estática para una dirección. Se ve obligado a usar DHCP.

Para resolver este problema, en Windows Admin Center, mire en **configuración** > **extensiones** para una alerta que indica que el servicio de migración de almacenamiento 0.57.2 versión actualizada está disponible para su instalación. Es posible que deba reiniciar la pestaña del explorador para el centro de administración de Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Validación de traslado de servicio de migración de almacenamiento se produce un error con el error "Acceso denegado para la directiva de filtro de token en el equipo de destino"

Al ejecutar validación traslado, recibes el error "Error: Acceso denegado para la directiva de filtro de token en el equipo de destino." Esto ocurre aunque proporcione credenciales de administrador local correcto para los equipos de origen y destino.

Este problema se debe a un defecto de código en Windows Server 2019. El problema se producirá cuando se usa el equipo de destino como un orquestador de servicios de migración de almacenamiento.

Para solucionar este problema, instale el servicio de migración de almacenamiento en un equipo de Windows Server 2019 que no es el destino previsto de migración, a continuación, conectarse a dicho servidor con Windows Admin Center y realizar la migración.

Se ha corregido en una versión posterior de Windows Server. Abra una incidencia a través de [Microsoft Support](https://support.microsoft.com) para solicitar un restituir de esta revisión se cree.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Servicio de migración de almacenamiento no se incluye en la edición de evaluación de Windows Server de 2019

Cuando se usa Windows Admin Center para conectarse a un [versión de evaluación de Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) no hay una opción para administrar el servicio de migración de almacenamiento. Servicio de migración de almacenamiento no se incluye también en las funciones y características.

Este problema está causado por un problema de mantenimiento en los medios de evaluación de Windows Server 2019. 

Para solucionar este problema, instale un minorista, MSDN, OEM o licencias por volumen de versión de Windows Server 2019 y no lo activo. Sin activación, todas las ediciones de Windows Server funcionan en modo de evaluación durante 180 días. 

Este problema se ha corregido en una versión posterior de Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Servicio de migración de almacenamiento agota el tiempo de descarga el error de transferencia de CSV

Cuando se usa Windows Admin Center o PowerShell para descargar la transferencia operaciones solo errores CSV registro detallado, recibirá un error:

 >   Transferencia de registros: Compruebe que se permite el uso compartido de archivos en el firewall. : Esta operación de solicitud enviada a NET. TCP://localhost: 28940/sms/service/1/transferencia no recibió una respuesta en el tiempo de espera configurado (00: 01:00). El tiempo asignado a esta operación puede haber sido una parte de un tiempo de espera mayor. Esto puede ser porque el servicio sigue procesando la operación o porque el servicio no pudo enviar un mensaje de respuesta. Por favor, considere la posibilidad de aumentar el tiempo de espera de la operación (convirtiendo al canal/proxy en IContextChannel y estableciendo la propiedad OperationTimeout) y asegúrese de que el servicio es capaz de conectarse al cliente.

Este problema está causado por un número sumamente grande de archivos transferidos que no se pueden filtrar en el tiempo de espera de un minuto predeterminado permitido por el servicio de migración de almacenamiento. 

Para solucionar este problema:

1. En el equipo de orchestrator, modifique la *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* archivos con Notepad.exe para cambiar el "sendTimeout" a su valor predeterminado de 1 minuto a 10 minutos

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie el servicio "Servicio de migración de almacenamiento" en el equipo de orchestrator. 
3. En el equipo de orchestrator, inicie Regedit.exe
4. Busque y haga clic en la siguiente subclave del Registro: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. En el menú Edición, seleccione Nuevo y haga clic en Valor DWORD. 
6. Escriba "WcfOperationTimeoutInMinutes" para el nombre del valor DWORD y, a continuación, presione ENTRAR.
7. Haga clic en "WcfOperationTimeoutInMinutes" y, a continuación, haga clic en modificar. 
8. En el cuadro de la Base de datos, haga clic en "Decimal"
9. En el cuadro de datos valor, escriba "10" y, a continuación, haga clic en Aceptar.
10. Salga del Editor del registro.
11. Intente volver a descargar el archivo CSV sólo errores. 

Tenemos previsto cambiar este comportamiento en una versión posterior de Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Migración completa se produce un error al migrar entre redes

Al migrar a un equipo de destino que se ejecuta en una red diferente de origen, como una instancia de IaaS de Azure, traslado no puede completarse cuando el origen utiliza una dirección IP estática. 

Este comportamiento es así por diseño, para evitar problemas de conectividad después de la migración de usuarios, aplicaciones y scripts que se conectan a través de la dirección IP. Cuando se mueve la dirección IP del equipo de origen antiguo a nuevo objetivo de destino, no coincidirá con la nueva información de subred de red y quizás DNS y WINS.

Para solucionar este problema, realice una migración a un equipo en la misma red. A continuación, mover ese equipo a una nueva red y volver a asignar su información de IP. Por ejemplo, si la migración a IaaS de Azure, primero migre a una máquina virtual local y luego usar Azure Migrate para desplazar la máquina virtual en Azure.  

Este problema se ha corregido en una versión posterior de Windows Admin Center. Ahora nos permitirá especificar las migraciones que no alteran la configuración de red del servidor de destino. La extensión actualizada se mostrará aquí cuando suelta. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Advertencias de validación para los privilegios administrativos de proxy y las credenciales de destino

Al validar un trabajo de transferencia, consulte las siguientes advertencias:

 > **La credencial con privilegios administrativos.**
 > Advertencia: Acción no están disponible de forma remota.
 > **El proxy de destino está registrado.**
 > Advertencia: No se encontró el proxy de destino.

Si no ha instalado el servicio de Proxy de servicio de migración de almacenamiento en el equipo de destino de Windows Server 2019, o el equipo de destino es Windows Server 2016 o Windows Server 2012 R2, este comportamiento es así por diseño. Se recomienda migrar a un equipo Windows Server 2019 con el proxy instalado para el rendimiento considerablemente mejorado de transferencia.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Algunos archivos no inventario o transferir, 5 de error "acceso denegado"

Al realizar un inventario o transferencia de archivos de origen a los equipos de destino, los archivos desde el que un usuario ha eliminado los permisos del grupo Administradores no pueden migrar. Examen de la depuración de Proxy de servicio de almacenamiento migración muestra:

  Nombre de registro:      Origen de Microsoft-Windows-StorageMigrationService-Proxy/Debug:        Microsoft-Windows-StorageMigrationService-Proxy Date:          26/2/2019 9:00:04 A.M. Id. de evento:      Categoría de tarea 10000: Ninguno de nivel:         Palabras clave de error:      
  Usuario:          Equipo de servicio de red: descripción srv1.contoso.com:

  02/26/2019-09:00:04.860 [ERROR] error de transferencia para \\srv1.contoso.com\public\indy.png: (5) acceso denegado.
Seguimiento de la pila: En Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) en Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (ruta de acceso de cadena) en Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (archivo FileInfo) en Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo() en Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() en Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


Este problema se debe a un defecto de código en el servicio de migración de almacenamiento donde no se invocó el privilegio de copia de seguridad. 

Para resolver este problema, instale [Windows Update 2 de abril de 2019: KB4490481 (17763.404 de compilación del sistema operativo)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) en el equipo de orchestrator y el equipo de destino si está instalado el servicio de proxy no existe. Asegúrese de que la cuenta de usuario de la migración de código fuente es un administrador local en el equipo de origen y el orquestador del servicio de migración de almacenamiento. Asegúrese de que la cuenta de usuario de la migración de destino es un administrador local en el equipo de destino y el orquestador del servicio de migración de almacenamiento. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Error de coincidencia de DFSR hashes cuando se usa el servicio de migración de almacenamiento para inicializar previamente los datos

Cuando se usa el servicio de almacenamiento de migración para transferir archivos a un nuevo destino, a continuación, configurar la replicación DFS (DFSR) para replicar los datos con un servidor existente de DFSR a través de la replicación presseded o DFSR base de datos de clonación, todos los archivos experiemce un hash Error de coincidencia y se vuelvan a replicarán. Los flujos de datos, secuencias de seguridad, tamaños y atributos aparecen para que coincida perfectamente después de usar SMS para transferirlos. Examinar los archivos con ICACLS o el registro de depuración de clonación de base de datos de DFSR revela:

Archivo de origen:

  icacls d:\test\Source:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1200a9;;;DD)(A;;0x1301bf;;;DU)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)

Archivo de destino:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1301bf;;;DU)(A;;0x1200a9;;;DD)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)**S:PAINO_ACCESS_CONTROL**

Registro de depuración DFSR:

  Se encontró el registro de error de coincidencia de DBClone::IDTableImportUpdate DBCL 4045 [advertencia] 3948 de 20190308 10:18:53.116. 

  Hash ACL local: 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Atributos: 32 

  Clonar el hash ACL:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Atributos: 32 

Este problema se debe a un defecto de código en una biblioteca que usa el servicio de migración de almacenamiento para establecer la auditoría de seguridad de ACL (SACL). Una SACL no null se establece involuntariamente cuando estaba vacía, la SACL iniciales DFSR para identificar correctamente un error de coincidencia de hash. 

Para solucionar este problema, siga usando Robocopy para [DFSR inicialización previa y las operaciones de clonación de base de datos de DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) en lugar del servicio de migración de almacenamiento. Se está investigando este problema y se va a resolver este problema en una versión posterior de Windows Server y, posiblemente, una actualización de Windows usado. 

## <a name="error-404-when-downloading-csv-logs"></a>Los registros de errores 404 al descargar CSV

Cuando se intenta descargar los registros de error o la transferencia al final de una operación de transferencia, recibirá un error:

  $jobname: Transferencia de registros: error 404 de ajax

Se espera que este error si no ha habilitado la regla de firewall "Compartir archivos e impresoras (SMB de entrada)" en el servidor de orchestrator. Las descargas de archivos de Windows Admin Center requieren el puerto TCP/445 (SMB) en equipos conectados.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>Error "no pudo transferir almacenamiento en cualquiera de los puntos de conexión" al transferir desde Windows Server 2008 R2

Cuando se intenta transferir datos desde un equipo de origen de Windows Server 2008 R2, no trasnfers de datos y se recibe el error:  

  No se pudo transferir el almacenamiento en cualquiera de los puntos de conexión.
0x9044

Se espera que este error si el equipo de Windows Server 2008 R2 no está totalmente al día con todas las críticas e importantes actualizaciones desde Windows Update. Con independencia del servicio de migración de almacenamiento, siempre se recomienda un equipo de Windows Server 2008 R2 por motivos de seguridad de la aplicación de revisiones como ese sistema operativo no contiene las mejoras de seguridad de las versiones más recientes de Windows Server.

## <a name="see-also"></a>Vea también

- [Información general sobre el servicio de migración de almacenamiento](overview.md)
