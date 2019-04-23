---
title: Servicio de migración de almacenamiento problemas conocidos
description: Problemas conocidos y solución de problemas de soporte técnico para servicio de migración de almacenamiento, como cómo recopilar registros de Microsoft Support.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888876"
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

Tenemos previsto corregir este problema en una versión posterior de Windows Server. Abra una incidencia a través de [Microsoft Support](https://support.microsoft.com) para solicitar un restituir de esta revisión se cree.

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

Al migrar a una máquina virtual de destino que se ejecuta en una red diferente de origen, como una instancia de IaaS de Azure, traslado produce un error en completarse cuando el origen utiliza una dirección IP estática. 

Este comportamiento es así por diseño, para evitar problemas de conectividad después de la migración de usuarios, aplicaciones y scripts que se conectan a través de la dirección IP. Cuando se mueve la dirección IP del equipo de origen antiguo a nuevo objetivo de destino, no coincidirá con la nueva información de subred de red y quizás DNS y WINS.

Para solucionar este problema, realice una migración a un equipo en la misma red. A continuación, mover ese equipo a una nueva red y volver a asignar su información de IP. Por ejemplo, si la migración a IaaS de Azure, primero migre a una máquina virtual local y luego usar Azure Migrate para desplazar la máquina virtual en Azure.  

Este problema se ha corregido en una versión posterior de Windows Server 2019. Ahora nos permitirá especificar las migraciones que no alteran la configuración de red del servidor de destino. Podemos publicamos una actualización de la versión existente de Windows Server 2019 como parte del ciclo de actualización mensual normal. 


## <a name="see-also"></a>Vea también

- [Información general sobre el servicio de migración de almacenamiento](overview.md)