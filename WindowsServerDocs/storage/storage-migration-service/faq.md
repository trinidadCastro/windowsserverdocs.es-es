---
title: Preguntas más frecuentes sobre el servicio de migración de almacenamiento (p + f)
description: Preguntas más frecuentes sobre el servicio de migración de almacenamiento, como qué archivos se excluyen de las transferencias al migrar de un servidor a otro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/02/2020
ms.topic: article
ms.openlocfilehash: e8e327fcf2f9173c7fb571580280ba4d5b7389fe
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997494"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Preguntas más frecuentes sobre el servicio de migración de almacenamiento (p + f)

Este tema contiene respuestas a las preguntas más frecuentes (p + f) sobre el uso del [servicio de migración de almacenamiento](overview.md) para migrar servidores.

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>¿Qué archivos y carpetas se excluyen de las transferencias?

El servicio de migración de almacenamiento no transferirá los archivos o carpetas que sabemos que podrían interferir con el funcionamiento de Windows. En concreto, aquí se indica lo que no se transferirá ni se moverá a la carpeta PreExistingData en el destino:

- Windows, archivos de programa, archivos de programa (x86), datos de programa, usuarios
- $Recycle. bin, Recycler, reciclado, información del volumen del sistema, $UpgDrv $, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. Old, boot, Recovery, Documents and Settings
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, Bootsect. bak, BOOTMGR, bootnxt
- Cualquier archivo o carpeta del servidor de origen que esté en conflicto con las carpetas excluidas en el destino. <br>Por ejemplo, si hay una carpeta N:\Windows en el origen y se asigna a C:\ volumen en el destino, no se transferirá, independientemente de lo que contenga, ya que interferiría con la carpeta del sistema C:\Windows. en el destino.

## <a name="are-locked-files-migrated"></a>¿Se han migrado los archivos bloqueados?

El servicio de migración de almacenamiento no migra los archivos que las aplicaciones bloquean exclusivamente. El servicio lo reintenta automáticamente tres veces con un retraso de sesenta segundos entre los intentos y puede controlar el número de intentos y el retraso. También puede volver a ejecutar las transferencias para copiar solo los archivos que se omitieron anteriormente debido a infracciones de uso compartido.

## <a name="are-domain-migrations-supported"></a>¿Se admiten las migraciones de dominio?

El servicio de migración de almacenamiento no permite la migración entre dominios Active Directory. Las migraciones entre servidores siempre unirán el servidor de destino al mismo dominio. Puede utilizar las credenciales de migración de distintos dominios en el bosque de Active Directory. El servicio de migración de almacenamiento admite la migración entre grupos de trabajo.

## <a name="are-clusters-supported-as-sources-or-destinations"></a>¿Se admiten clústeres como orígenes o destinos?

El servicio de migración de almacenamiento admite la migración desde y hacia clústeres después de la instalación de la actualización acumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) o actualizaciones posteriores. Esto incluye la migración de un clúster de origen a un clúster de destino, así como la migración desde un servidor de origen independiente a un clúster de destino para fines de consolidación de dispositivos. Sin embargo, no puede migrar un clúster a un servidor independiente.

## <a name="do-local-groups-and-local-users-migrate"></a>¿Migran los grupos locales y los usuarios locales?

El servicio de migración de almacenamiento admite la migración de usuarios y grupos locales después de la instalación de la actualización acumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) o actualizaciones posteriores.

## <a name="is-domain-controller-migration-supported"></a>¿Se admite la migración del controlador de dominio?

El servicio de migración de almacenamiento no migra actualmente controladores de dominio en Windows Server 2019. Como solución alternativa, siempre que tenga más de un controlador de dominio en el dominio Active Directory, disminuya el nivel del controlador de dominio antes de migrarlo y, a continuación, promueva el destino después de que se complete el corte. Si decide migrar el origen o el destino de un controlador de dominio, no podrá cambiar. Nunca debe migrar usuarios y grupos al migrar desde o a un controlador de dominio.

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>¿Qué atributos migra el servicio de migración de almacenamiento?

El servicio de migración de almacenamiento migra todas las marcas, la configuración y la seguridad de los recursos compartidos de SMB. La lista de marcas que migra el servicio de migración de almacenamiento incluye:

- Estado compartido
- Tipo de disponibilidad
- Tipo de recurso compartido
- Modo de enumeración *de carpetas (también conocido como enumeración basada en el acceso o Abe)*
- Modo de almacenamiento en caché
- Modo de concesión
- Instancia de SMB
- Tiempo de espera de CA
- Límite de usuarios simultáneos
- Disponible continuamente
- Descripción
- Cifrar datos
- Comunicación remota de identidad
- Infraestructura
- Nombre
- Ruta de acceso
- Con ámbito
- Nombre de ámbito
- Descriptor de seguridad
- Instantánea
- Especial
- Temporales

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>¿Puedo consolidar varios servidores en un solo servidor?

La versión del servicio de migración de almacenamiento incluida en Windows Server 2019 no admite la consolidación de varios servidores en un solo servidor. Un ejemplo de consolidación sería la migración de tres servidores de origen independientes, que pueden tener los mismos nombres de recurso compartido y rutas de acceso de archivo local, en un solo servidor nuevo que virtualizaba esas rutas y recursos compartidos para evitar cualquier superposición o colisión y, a continuación, respondió a los tres nombres de servidores anteriores y la dirección IP. Sin embargo, puede migrar servidores independientes en varios recursos del servidor de archivos en un solo clúster.

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>¿Puedo migrar desde orígenes distintos de Windows Server?

El servicio de migración de almacenamiento admite la migración desde servidores Samba Linux después de la instalación de la actualización acumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) o actualizaciones posteriores. Vea los requisitos para obtener una lista de las versiones de samba compatibles y Linux distribuciones.

## <a name="can-i-migrate-previous-file-versions"></a>¿Puedo migrar las versiones anteriores de los archivos?

La versión del servicio de migración de almacenamiento incluida en Windows Server 2019 no admite la migración de versiones anteriores (realizadas con el servicio de instantáneas de volumen) de archivos. Solo se migrará la versión actual.

## <a name="optimizing-inventory-and-transfer-performance"></a>Optimizar el rendimiento del inventario y de la transferencia

El servicio de migración de almacenamiento contiene un motor de lectura y copia multiproceso denominado servicio de proxy de migración de almacenamiento que se ha diseñado para que sea rápido y, además, ofrece una fidelidad de datos perfecta que carece de muchas herramientas de copia de archivos. Aunque la configuración predeterminada será óptima para muchos clientes, hay formas de mejorar el rendimiento de SMS durante el inventario y la transferencia.

- **Use Windows Server 2019 para el sistema operativo de destino.** Windows Server 2019 contiene el servicio de proxy del servicio de migración de almacenamiento. Al instalar esta característica y migrar a destinos de Windows Server 2019, todas las transferencias funcionan como línea directa de visión entre el origen y el destino. Este servicio se ejecuta en el orquestador durante la transferencia si los equipos de destino son Windows Server 2012 R2 o Windows Server 2016, lo que significa que las transferencias de doble salto y serán mucho más lentas. Si hay varios trabajos que se ejecutan con los destinos Windows Server 2012 R2 o Windows Server 2016, el orquestador se convertirá en un cuello de botella.

- **Modifique los subprocesos de transferencia predeterminados.** El servicio del proxy del servicio de migración de almacenamiento copia ocho archivos simultáneamente en un trabajo determinado. Puede aumentar el número de subprocesos de copia simultáneos si ajusta el siguiente registro REG_DWORD nombre de valor en decimal en cada nodo que ejecute el proxy del servicio de migración de almacenamiento:

    HKEY_Local_Machine \Software\Microsoft\SMSProxy

    FileTransferThreadCount

   El intervalo válido es de 1 a 512 en Windows Server 2019. No es necesario reiniciar el servicio para empezar a usar esta opción siempre que cree un nuevo trabajo. Tenga cuidado con esta configuración; Si se establece en un valor superior, es posible que se necesiten núcleos adicionales, rendimiento de almacenamiento y ancho de banda de red. Si se establece en un valor demasiado alto, se puede producir un rendimiento reducido en comparación con la configuración predeterminada.

- **Modifique los subprocesos de recurso compartido paralelo predeterminados.** El servicio del proxy del servicio de migración de almacenamiento copia de 8 recursos compartidos simultáneamente en un trabajo determinado. Puede aumentar el número de subprocesos de recursos compartidos simultáneos ajustando el siguiente registro REG_DWORD nombre de valor en formato decimal en el servidor Orchestrator de servicio de migración de almacenamiento:

    HKEY_Local_Machine \Software\Microsoft\SMS

    EndpointFileTransferTaskCount

   El intervalo válido es de 1 a 512 en Windows Server 2019. No es necesario reiniciar el servicio para empezar a usar esta opción siempre que cree un nuevo trabajo. Tenga cuidado con esta configuración; Si se establece en un valor superior, es posible que se necesiten núcleos adicionales, rendimiento de almacenamiento y ancho de banda de red. Si se establece en un valor demasiado alto, se puede producir un rendimiento reducido en comparación con la configuración predeterminada.

    La suma de FileTransferThreadCount y EndpointFileTransferTaskCount es el número de archivos que el servicio de migración de almacenamiento puede copiar simultáneamente desde un nodo de origen de un trabajo. Para agregar más nodos de origen en paralelo, cree y ejecute trabajos más simultáneos.

- **Agregue núcleos y memoria.**  Se recomienda encarecidamente que los equipos de origen, orquestador y destino tengan al menos dos núcleos de procesador o dos vCPU, y más pueden ayudar significativamente al inventario y a la transferencia de rendimiento, especialmente cuando se combinan con FileTransferThreadCount (arriba). Cuando se transfieren archivos que son mayores que los formatos de oficina habituales (gigabytes o superior), el rendimiento de la transferencia se beneficiará de más memoria que los 2 GB como mínimo.

- **Crear varios trabajos.** Al crear un trabajo con varios orígenes de servidor, se Contacta con cada servidor en serie para el inventario, la transferencia y el traslado. Esto significa que cada servidor debe completar su fase antes de que se inicie otro servidor. Para ejecutar más servidores en paralelo, simplemente cree varios trabajos, con cada trabajo que contenga un solo servidor. SMS admite hasta 100 trabajos que se ejecutan simultáneamente, lo que significa que un único orquestador puede paralelizar muchos equipos de destino de Windows Server 2019. No se recomienda ejecutar varios trabajos paralelos si los equipos de destino son Windows Server 2016 o Windows Server 2012 R2, ya que sin que el servicio de proxy de SMS se ejecute en el destino, el orquestador debe realizar todas las transferencias y puede convertirse en un cuello de botella. La capacidad de los servidores de ejecutarse en paralelo dentro de un solo trabajo es una característica que tenemos previsto agregar en una versión posterior de SMS.

- **Use SMB 3 con redes RDMA.** Si se transfiere desde un equipo de origen con Windows Server 2012 o posterior, SMB 3. x es compatible con la red RDMA y el modo SMB directo. RDMA mueve la mayor parte del costo de la CPU de la transferencia de las CPU de la placa base a la incorporación de procesadores NIC, lo que reduce la latencia y el uso de la CPU Además, las redes RDMA como ROCE y iWARP suelen tener un ancho de banda considerablemente superior al de TCP/Ethernet típico, que incluye 25, 50 y 100 GB de velocidad por interfaz. El uso de SMB directo normalmente mueve el límite de velocidad de transferencia de la red al almacenamiento en sí.

- **Use SMB 3 multicanal.** Si se transfiere desde un equipo de origen con Windows Server 2012 o posterior, SMB 3. x admite copias multicanal que pueden mejorar considerablemente el rendimiento de la copia de archivos. Esta característica funciona automáticamente siempre y cuando el origen y el destino tengan:

   - Adaptadores de red múltiples
   - Uno o más adaptadores de red que admiten el ajuste de escala en lado de recepción (RSS)
   - Uno o más adaptadores de red que se configuran mediante la formación de equipos NIC
   - Un adaptador de red o más que admita RDMA

- **Actualice los controladores.** Según corresponda, instale el firmware y los controladores más recientes de almacenamiento y alojamiento de proveedores, los controladores de HBA de proveedor más recientes, el firmware UEFI o BIOS de proveedor más reciente, los últimos controladores de red de proveedor y los controladores de conjunto de chips de placa base más recientes en servidores de origen, destino y Reinicie los nodos según sea necesario. Consulte la documentación del proveedor de hardware para configurar el almacenamiento compartido y el hardware de red.

- **Habilitar el procesamiento de alto rendimiento.** Asegúrese de que la configuración de BIOS o UEFI para los servidores permite un alto rendimiento; por ejemplo, deshabilite el estado C, establezca la velocidad de QPI, habilite NUMA y configure la frecuencia de la memoria en el valor más elevado. Asegúrese de que la administración de energía en Windows Server está establecida en alto rendimiento. Reinicie si es necesario. No olvide devolverlos a los Estados correspondientes después de completar la migración.

- **Ajustar hardware** Revise las [directrices para la optimización del rendimiento de Windows server 2016](../../administration/performance-tuning/index.md) para optimizar el orquestador y los equipos de destino que ejecutan windows Server 2019 y windows Server 2016. La sección de [ajuste del rendimiento del subsistema de red](../../networking/technologies/network-subsystem/net-sub-performance-tuning-nics.md) contiene información especialmente valiosa.

- **Use un almacenamiento más rápido.** Aunque puede ser difícil actualizar la velocidad de almacenamiento del equipo de origen, debe asegurarse de que el almacenamiento de destino sea al menos tan rápido como el rendimiento de e/s de escritura, ya que el origen está en el rendimiento de e/s de lectura para asegurarse de que no hay ningún cuello de botella innecesario en las transferencias. Si el destino es una máquina virtual, asegúrese de que, al menos para los fines de la migración, se ejecute en la capa de almacenamiento más rápida de los hosts del hipervisor, como en el nivel de Flash o en los clústeres de HCI de Espacios de almacenamiento directo que usan espacios de ejecución o de todo el flash reflejados. Una vez completada la migración de SMS, la máquina virtual se puede migrar en vivo a un nivel o host más lento.

- **Actualizar antivirus.** Asegúrese siempre de que el origen y el destino ejecutan la última versión de revisión del software antivirus para garantizar una sobrecarga de rendimiento mínima. Como prueba, puede excluir *temporalmente* el examen de las carpetas de las que está realizando un inventario o de la migración en los servidores de origen y de destino. Si se mejora el rendimiento de la transferencia, póngase en contacto con el proveedor del software antivirus para obtener instrucciones o una versión actualizada del software antivirus o una explicación de la degradación del rendimiento esperado.

## <a name="can-i-migrate-from-ntfs-to-refs"></a>¿Puedo migrar de NTFS a REFS?

La versión del servicio de migración de almacenamiento incluida en Windows Server 2019 no admite la migración desde los sistemas de archivos NTFS a REFS. Puede migrar de NTFS a NTFS y REFS a ReFS. Esto es así por diseño, debido a las numerosas diferencias en la funcionalidad, los metadatos y otros aspectos que ReFS no duplica de NTFS. ReFS está pensado como un sistema de archivos de carga de trabajo de la aplicación, no como un sistema de archivos general. Para obtener más información, vea [información general sobre el sistema de archivos resistente (ReFS)](../refs/refs-overview.md) .

## <a name="can-i-move-the-storage-migration-service-database"></a>¿Puedo trasladar la base de datos del servicio de migración de almacenamiento?

El servicio de migración de almacenamiento utiliza una base de datos del motor de almacenamiento extensible (ESE) que se instala de forma predeterminada en la carpeta c:\programdata\microsoft\storagemigrationservice oculta. Esta base de datos crecerá a medida que se agreguen los trabajos y se completen las transferencias, y puede consumir mucho espacio en la unidad después de migrar millones de archivos si no se eliminan los trabajos. Si la base de datos debe moverse, realice los pasos siguientes:

1. Detenga el servicio de "servicio de migración de almacenamiento" en el equipo de Orchestrator.
2. Tomar posesión de la `%programdata%/Microsoft/StorageMigrationService` carpeta
3. Agregue su cuenta de usuario para tener control total sobre ese recurso compartido y todos sus archivos y subcarpetas.
4. Mueva la carpeta a otra unidad del equipo del orquestador.
5. Establezca el siguiente valor del REG_SZ del registro:

    HKEY_Local_Machine \Software\Microsoft\SMS DatabasePath = *path a la nueva carpeta de base de datos en un volumen diferente*

6. Asegúrese de que el sistema tenga control total sobre todos los archivos y subcarpetas de la carpeta.
7. Quite los permisos de sus propias cuentas.
8. Inicie el servicio de "servicio de migración de almacenamiento".

## <a name="does-the-storage-migration-service-migrate-locally-installed-applications-from-the-source-computer"></a>¿Migra el servicio de migración de almacenamiento las aplicaciones instaladas localmente desde el equipo de origen?

No, el servicio de migración de almacenamiento no migra las aplicaciones instaladas localmente. Después de completar la migración, vuelva a instalar las aplicaciones en el equipo de destino que se estaban ejecutando en el equipo de origen. No es necesario volver a configurar ningún usuario ni sus aplicaciones; el servicio de migración de almacenamiento está diseñado para hacer que el servidor cambie invisible para los clientes.

## <a name="what-happens-with-existing-files-on-the-destination-server"></a>¿Qué ocurre con los archivos existentes en el servidor de destino?

Al realizar una transferencia, el servicio de migración de almacenamiento busca los datos reflejados del servidor de origen. El servidor de destino no debe contener datos de producción ni usuarios conectados, ya que se podrían sobrescribir los datos. De forma predeterminada, la primera transferencia realiza una copia de seguridad de los datos en el servidor de destino como medida de seguridad. De forma predeterminada, en todas las transferencias posteriores, el servicio de migración de almacenamiento reflejará los datos en el destino. Esto significa que no solo se agregan nuevos archivos, sino que también se sobrescribe arbitrariamente cualquier archivo existente y se eliminan los archivos que no están presentes en el origen. Este comportamiento es intencionado y proporciona una fidelidad perfecta con el equipo de origen.

## <a name="what-do-the-error-numbers-mean-in-the-transfer-csv"></a>¿Qué significan los números de error en el CSV de transferencia?

La mayoría de los errores encontrados en el archivo CSV de transferencia son códigos de error del sistema de Windows. Puede averiguar lo que significa cada error revisando la documentación de los [códigos de error de Win32](/windows/win32/debug/system-error-codes).

## <a name="what-are-my-options-to-give-feedback-file-bugs-or-get-support"></a><a name="give-feedback"></a>¿Cuáles son mis opciones para proporcionar comentarios, errores de archivos u obtener soporte técnico?

Para proporcionar comentarios sobre el servicio de migración de almacenamiento:

- Use la herramienta centro de comentarios que se incluye en Windows 10, haciendo clic en "sugerir una característica" y especificando la categoría de "Windows Server" y la subcategoría de "migración de almacenamiento".
- Usar el sitio web de [Windows Server UserVoice](https://windowsserver.uservoice.com)
- Un correo electrónico a smsfeed@microsoft.com

Para archivos errores:

- Use la herramienta centro de comentarios que se incluye en Windows 10, haciendo clic en "Notificar un problema" y especificando la categoría de "Windows Server" y la subcategoría de "migración de almacenamiento".
- Abra un caso de soporte técnico a través de [soporte técnico de Microsoft](https://support.microsoft.com)

Para obtener soporte técnico:

 - Publicar una pregunta en la [comunidad de tecnología de Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Publicar en el [Foro de Windows Server 2019](/answers/topics/windows-server-2019.html)
 - Abra un caso de soporte técnico a través de [soporte técnico de Microsoft](https://support.microsoft.com)

## <a name="additional-references"></a>Referencias adicionales

- [Información general del servicio de migración de almacenamiento](overview.md)