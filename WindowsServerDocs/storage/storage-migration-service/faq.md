---
title: Servicio de migración de almacenamiento preguntas más frecuentes (P+F)
description: Preguntas más frecuentes acerca del servicio de migración de almacenamiento, por ejemplo, qué archivos se excluyen de las transferencias al migrar desde un servidor a otro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860916"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Servicio de migración de almacenamiento preguntas más frecuentes (P+F)

Este tema contiene respuestas a las preguntas más frecuentes (P+f) sobre el uso de [Storage Migration Service](overview.md) para migrar los servidores.

## <a name="excluded-files"></a> ¿Qué archivos y carpetas se excluirán de las transferencias?

Servicio de migración de almacenamiento no transferencia de archivos o carpetas que sabemos que podría interferir con la operación de Windows. En concreto, aquí es lo que nos no transferir o mover a la carpeta PreExistingData en el destino:

- Programa de Windows, archivos, archivos de programa (x86), datos de programa, los usuarios
- $Recycle.bin, recycler, la Papelera de reciclaje, la información de volumen del sistema, $UpgDrv$, $SysReset, Windows $. ~ BT, Windows $. ~ LS, Windows.old, arranque, documentos, configuración y recuperación
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, bootsect.bak, bootmgr, bootnxt
- Los archivos o carpetas en el servidor de origen que entra en conflicto con las carpetas excluidas de la de destino. <br>Por ejemplo, si hay una carpeta N:\Windows en el origen y se asignan a la C:\ volumen en el destino, no se transfieren, independientemente de lo que lo contiene, ya que podría interferir con la carpeta C:\Windows del sistema en el destino.

## <a name="domain-migration"></a> ¿Se admiten las migraciones de dominio?

Servicio de migración de almacenamiento no permite migrar entre dominios de Active Directory. Las migraciones entre servidores siempre unirá el servidor de destino al mismo dominio. Puede usar las credenciales de la migración desde diferentes dominios del bosque de Active Directory. El servicio de migración de almacenamiento es compatible con migración entre grupos de trabajo.  

## <a name="cluster-support"></a> ¿Se admiten como orígenes o destinos clústeres?

Servicio de migración de almacenamiento no actualmente la migración entre clústeres en Windows Server 2019. Tenemos previsto agregar compatibilidad con clústeres en una versión futura del servicio de migración de almacenamiento.

## <a name="local-principals"></a> ¿No grupos locales y migran los usuarios locales?

Servicio de migración de almacenamiento actualmente no migrar usuarios locales o grupos locales de Windows Server 2019. Tenemos previsto agregar compatibilidad con usuario local y migración de grupo local en una versión futura del servicio de migración de almacenamiento.

## <a name="domain-controller"></a> ¿Se admite la migración del controlador de dominio?

Servicio de migración de almacenamiento actualmente no migrar controladores de dominio de Windows Server 2019. Como alternativa, siempre que tengan más de un controlador de dominio en el dominio de Active Directory, disminuir de nivel el controlador de dominio antes de migrarlos, debe promoverla el destino una vez completada la implantación. Tenemos previsto agregar compatibilidad con la migración de controlador de dominio en una versión futura del servicio de migración de almacenamiento.

## <a name="share-attributes"></a> ¿Los atributos que se migran mediante el servicio de migración de almacenamiento?

Servicio de migración de almacenamiento migra todos los indicadores, configuración y seguridad de recursos compartidos de SMB. Esa lista de marcas que migra el servicio de migración de almacenamiento incluye:

    - Estado del recurso compartido
    - Tipo de disponibilidad
    - Tipo de recurso compartido
    - Modo de enumeración de la carpeta *(también conocido como Access según la enumeración o ABE)*
    - Modo de almacenamiento en caché
    - Modo de concesión
    - Instancia de SMB
    - Tiempo de espera de CA
    - Límite de usuarios simultáneos
    - Disponibilidad continua
    - Descripción           
    - Cifrar datos
    - Comunicación remota de identidad
    - Infraestructura
    - Name
    - Ruta de acceso
    - El ámbito
    - Nombre de ámbito
    - Descriptor de seguridad
    - Instantánea
    - Especial
    - Temporal

## <a name="move-db"></a> ¿Puedo mover la base de datos del servicio de migración de almacenamiento?

El servicio de migración de almacenamiento usa una base de datos de almacenamiento extensible (ESE) del motor que se instala de forma predeterminada en la carpeta c:\programdata\microsoft\storagemigrationservice oculto. Esta base de datos crecerá a medida que se agregan los trabajos y las transferencias se completan y pueden consumir el espacio en disco significativo después de migrar millones de archivos si no elimina los trabajos. Si necesita mover la base de datos, realice los pasos siguientes:

1. Detenga el servicio "Servicio de migración de almacenamiento" en el equipo de orchestrator.
2. Tomar posesión de la `%programdata%/Microsoft/StorageMigrationService` carpeta
3. Agregue su cuenta de usuario tiene control total sobre el se comparten y todos sus archivos y subcarpetas.
4. Mover la carpeta a otra unidad en el equipo de orchestrator.
5. Establezca el valor REG_SZ de registro siguiente:

    DatabasePath HKEY_Local_Machine\Software\Microsoft\SMS = *ruta de acceso a la nueva carpeta de base de datos en un volumen diferente* . 
6. Asegúrese de que el sistema tenga control total a todos los archivos y subcarpetas de la carpeta
7. Quitar sus propios permisos de cuentas.
8. Inicie el servicio "Servicio de migración de almacenamiento".

## <a name="transfer-threads"></a> ¿Aumentar el número de archivos que copiar al mismo tiempo?

El servicio de Proxy de servicio de migración de almacenamiento copia 8 archivos simultáneamente en un trabajo determinado. Este servicio se ejecuta en el orquestador durante la transferencia si los equipos de destino están Windows Server 2012 R2 o Windows Server 2016, pero también se ejecuta en todos los nodos de destino de Windows Server 2019. Puede aumentar el número de subprocesos de copia simultáneas ajustando el nombre del valor REG_DWORD del registro siguiente en formato decimal en cada nodo ejecuta al Proxy de SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 El intervalo válido es 1 y 128 en Windows Server 2019. 

 Después de cambiar debe reiniciar el servicio de Proxy de servicio de migración de almacenamiento en partipating de todos los equipos en una migración. Tenemos previsto aumentar este número en una versión futura del servicio de migración de almacenamiento.

## <a name="non-windows"></a> ¿Puedo migrar desde orígenes que no sean de Windows Server?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 admite la migración desde Windows Server 2003 y sistemas operativos posteriores. Actualmente no puede migrar desde Linux, Samba, NetApp, EMC u otros dispositivos de almacenamiento SAN y NAS. Tenemos previsto permitir esto en una versión futura del servicio de migración de almacenamiento, a partir de soporte técnico de Linux Samba.

## <a name="previous-versions"></a> ¿Puedo migrar versiones anteriores del archivo?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 no es compatible con migración versiones anteriores (realizadas con el servicio de instantáneas de volumen) de archivos. Solo la versión actual se migrará. 

## <a name="ntfs-refs"></a> ¿Puedo migrar de NTFS a REFS?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 no admite la migración de NTFS a los sistemas de archivos REFS. Puede migrar de NTFS a NTFS y REFS Refs. Esto es así por diseño, debido a las numerosas diferencias de funcionalidad, metadatos y otros aspectos que ReFS no duplique de NTFS. ReFS está diseñado como un sistema de archivos de carga de trabajo de aplicación, no es un sistema de archivos generales. Para obtener más información, consulte [información general del sistema de archivos resistente (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> ¿Puedo consolidar varios servidores en un servidor?

No admite la versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 consolidar varios servidores en un servidor. Muestra un ejemplo de consolidación podría migrar tres servidores de origen independiente - que pueden tener los mismos nombres de recurso compartido y las rutas de acceso de archivo local: en un único servidor nuevo que esas rutas de acceso y recursos compartidos para evitar colisiones, ni se superponen a virtualizar, a continuación, responden las tres los nombres de los servidores anteriores y la dirección IP. Podemos agregar esta funcionalidad en una versión futura del servicio de migración de almacenamiento.  


## <a name="give-feedback"></a> ¿Qué opciones tengo para proporcionar comentarios, archivar errores, u obtener soporte técnico?

Para enviar comentarios sobre el servicio de migración de almacenamiento:

- Use la herramienta Centro de comentarios incluida en Windows 10, haga clic en "Sugerir una característica" y especificar la categoría de "Windows Server" y subcategoría de "Migración de almacenamiento"
- Use la [UserVoice de Windows Server](https://windowsserver.uservoice.com) sitio
- Correo electrónico smsfeed@microsoft.com

Errores de archivo:

- Use la herramienta Centro de comentarios incluida en Windows 10, haga clic en "Notificar un problema" y especificar la categoría de "Windows Server" y subcategoría de "Migración de almacenamiento"
- Abra una incidencia de soporte técnico a través de [Microsoft Support](https://support.microsoft.com)

Para obtener soporte técnico:

 - Publicar una pregunta en el [comunidad tecnológica de Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Publicar en el [foro de Technet de Windows Server de 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Abra una incidencia de soporte técnico a través de [Microsoft Support](https://support.microsoft.com)


## <a name="see-also"></a>Vea también

- [Información general sobre el servicio de migración de almacenamiento](overview.md)
