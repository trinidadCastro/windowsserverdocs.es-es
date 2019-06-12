---
title: Servicio de migración de almacenamiento preguntas más frecuentes (P+F)
description: Preguntas más frecuentes acerca del servicio de migración de almacenamiento, por ejemplo, qué archivos se excluyen de las transferencias al migrar desde un servidor a otro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 258f25a7e1ec5c796c15450625397e96db25d693
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501522"
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
    - Nombre
    - Path
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

## <a name="non-windows"></a> ¿Puedo migrar desde orígenes que no sean de Windows Server?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 admite la migración desde Windows Server 2003 y sistemas operativos posteriores. Actualmente no puede migrar desde Linux, Samba, NetApp, EMC u otros dispositivos de almacenamiento SAN y NAS. Tenemos previsto permitir esto en una versión futura del servicio de migración de almacenamiento, a partir de soporte técnico de Linux Samba.

## <a name="previous-versions"></a> ¿Puedo migrar versiones anteriores del archivo?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 no es compatible con migración versiones anteriores (realizadas con el servicio de instantáneas de volumen) de archivos. Solo la versión actual se migrará. 

## <a name="ntfs-refs"></a> ¿Puedo migrar de NTFS a REFS?

La versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 no admite la migración de NTFS a los sistemas de archivos REFS. Puede migrar de NTFS a NTFS y REFS Refs. Esto es así por diseño, debido a las numerosas diferencias de funcionalidad, metadatos y otros aspectos que ReFS no duplique de NTFS. ReFS está diseñado como un sistema de archivos de carga de trabajo de aplicación, no es un sistema de archivos generales. Para obtener más información, consulte [información general del sistema de archivos resistente (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> ¿Puedo consolidar varios servidores en un servidor?

No admite la versión del servicio de migración de almacenamiento se incluye en Windows Server 2019 consolidar varios servidores en un servidor. Muestra un ejemplo de consolidación podría migrar tres servidores de origen independiente - que pueden tener los mismos nombres de recurso compartido y las rutas de acceso de archivo local: en un único servidor nuevo que esas rutas de acceso y recursos compartidos para evitar colisiones, ni se superponen a virtualizar, a continuación, responden las tres los nombres de los servidores anteriores y la dirección IP. Podemos agregar esta funcionalidad en una versión futura del servicio de migración de almacenamiento.  

## <a name="optimize"></a> Optimizar el rendimiento de inventario y la transferencia

El servicio de migración de almacenamiento contiene una lectura multiproceso y el motor de copia llamado servicio de Proxy de servicio de migración de almacenamiento que se ha diseñado para ser rápido así como llevar datos perfectos que carecen de fidelidad en muchas herramientas de copia de archivo. Aunque la configuración predeterminada sea óptima para muchos clientes, hay formas de mejorar el rendimiento de SMS durante el inventario y la transferencia.

- **Usar Windows Server 2019 para el sistema operativo de destino.** Windows Server 2019 contiene el servicio de Proxy de servicio de migración de almacenamiento. Al instalar esta característica y migrar a Windows Server 2019 destinos, todas las transferencias funcionan como línea de visión directa entre el origen y destino. Este servicio se ejecuta en el orquestador durante la transferencia si los equipos de destino son de Windows Server 2012 R2 o Windows Server 2016, lo que significa que a las transferencias de salto doble y serán mucho más lentos. Si hay varios trabajos que se ejecutan con Windows Server 2012 R2 o Windows Server 2016 destinos, el orquestador se convertirá en un cuello de botella. 

- **Modificar los subprocesos de transferencia de forma predeterminada.** El servicio de Proxy de servicio de migración de almacenamiento copia 8 archivos simultáneamente en un trabajo determinado. Puede aumentar el número de subprocesos de copia simultáneas ajustando el nombre del valor REG_DWORD del registro siguiente en formato decimal en cada nodo ejecuta al Proxy de SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   El intervalo válido es 1 y 128 en Windows Server 2019. Después de cambiar debe reiniciar el servicio de Proxy de servicio de migración de almacenamiento en todos los equipos que participan en una migración. Sea precavido al usar esta opción. configuración posterior puede requerir núcleos adicionales, el rendimiento de almacenamiento y ancho de banda de red. Si se establece demasiado alto puede provocar un rendimiento reducido en comparación con la configuración predeterminada. La capacidad para cambiar la configuración de subprocesos basada en CPU, memoria, red y almacenamiento de heurísticamente está prevista para una versión posterior de SMS.

- **Agregue los núcleos y memoria.**  Se recomienda encarecidamente que los equipos de origen, orchestrator y destino tienen al menos dos núcleos de procesador o dos vCPU y significativamente más pueden ayudar al rendimiento de inventario y la transferencia, especialmente cuando se combina con FileTransferThreadCount (arriba). Al transferir los archivos que son más grandes que los formatos Office habituales (gigabytes o superior) se beneficiará de rendimiento de la transferencia más memoria que el valor mínimo de 2GB.

- **Crear varios trabajos.** Al crear un trabajo con varios orígenes de servidor, se contacta con cada servidor en modo serie para el inventario de transferencia y el traslado. Esto significa que cada servidor debe completar su fase antes de inicia otro servidor. Para ejecutar más servidores en paralelo, simplemente cree varios trabajos, con cada trabajo que contiene solo uno servidores. SMS admite hasta 100 que se ejecutan simultáneamente los trabajos, lo que significa que una sola orchestrator puede establecer paralelismos en varios equipos de destino de Windows Server 2019. No recomendamos ejecutar varios trabajos paralelos si los equipos de destino son Windows Server 2016 o Windows Server 2012 R2 como sin que el servicio de proxy SMS que se ejecutan en el destino, el orquestador debe realizar todas las transferencias propio y podría convertirse en un cuello de botella. La capacidad de servidores para ejecutarlas en paralelo en un único trabajo es una característica que vamos a agregar en una versión posterior de SMS.

- **Usar SMB 3 con redes RDMA.** Si se transfiere de un Windows Server 2012 o posterior equipo de origen, SMB 3.x admite SMB directo el modo y las redes RDMA. RDMA mueve costo de CPU de la mayoría de transferencia desde la placa base CPU a incorporarlos procesadores NIC, lo que reduce la latencia y el servidor de la CPU. Además, las redes RDMA como ROCE y iWARP suelen tener un ancho de banda mucho más alto que típico TCP/ethernet, incluyendo 25, 50 y velocidades de 100Gb por interfaz. Mediante SMB directo, normalmente, mueve el límite de velocidad de transferencia de la red hasta el almacenamiento de información.   

- **Utiliza 3 SMB multicanal.** Si la transferencia de un Windows Server 2012 o de equipo de origen más adelante, SMB 3.x admite multicanal copias que pueden mejorar considerablemente el archivo rendimiento de la copia. Esta característica funciona automáticamente siempre que tengan el origen y destino:

   - Adaptadores de red múltiples
   - Uno o más adaptadores de red que admiten el escalado de lado de recepción (RSS)
   - Uno de varios adaptadores de red que se configuran mediante la formación de equipos NIC
   - Un adaptador de red o más que admita RDMA

- **Actualizar los controladores.** Según corresponda, instale el almacenamiento de proveedor más reciente y firmware de alojamiento y controladores, controladores HBA más recientes de proveedor, firmware de BIOS o UEFI proveedor más reciente, controladores de red de proveedor más recientes y controladores más recientes de conjunto de chips de placa base en el origen, destino y de orchestrator servidores. Reinicie los nodos según sea necesario. Consulte la documentación del proveedor de hardware para configurar el almacenamiento compartido y el hardware de red.

- **Habilitar el procesamiento de alto rendimiento.** Asegúrese de que la configuración de BIOS o UEFI para los servidores permite un alto rendimiento; por ejemplo, deshabilite el estado C, establezca la velocidad de QPI, habilite NUMA y configure la frecuencia de la memoria en el valor más elevado. Asegúrese de que la administración de energía en Windows Server se establece en alto rendimiento. Reinicie si es necesario. No olvide devolver estos Estados adecuada después de completar la migración. 

- **Ajustar el hardware** revisión el [Performance Tuning directrices para Windows Server 2016](https://docs.microsoft.com/en-us/windows-server/administration/performance-tuning/) para optimizar el orquestador y equipos de destino que ejecuta Windows Server 2019 y Windows Server 2016. El [ajuste de rendimiento del subsistema de red](https://docs.microsoft.com/en-us/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) sección contiene información especialmente valiosa.

- **Usar el almacenamiento más rápido.** Aunque puede ser difícil de actualizar la velocidad de almacenamiento del equipo de origen, debe asegurarse que el almacenamiento de destino sea al menos tan rápido en el rendimiento de E/S de escritura, como el origen está en el rendimiento de E/S de lectura para asegurarse de que no hay ningún cuello de botella innecesario en las transferencias. Si el destino es una máquina virtual, asegúrese de que, al menos para los fines de migración, se ejecuta en la capa de almacenamiento más rápida de los hosts de hipervisor, como en el nivel de flash o con el uso de memoria flash reflejado o espacios híbrida de clústeres de HCI directa de espacios de almacenamiento. Una vez completada la migración de SMS la máquina virtual puede ser en vivo migra a un host o un nivel más lento.

- **Actualización de antivirus.** Asegúrese siempre de origen y destino están ejecutando la versión de revisión más reciente del software antivirus para asegurar una sobrecarga de rendimiento mínimo. Como una prueba, puede *temporalmente* excluir el examen de carpetas es realizar un inventario o migrar los servidores de origen y destino. Si se ha mejorado el rendimiento de la transferencia, póngase en contacto con su proveedor de software antivirus para obtener instrucciones, o para una versión actualizada del software antivirus o una explicación de degradación del rendimiento esperado.

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
