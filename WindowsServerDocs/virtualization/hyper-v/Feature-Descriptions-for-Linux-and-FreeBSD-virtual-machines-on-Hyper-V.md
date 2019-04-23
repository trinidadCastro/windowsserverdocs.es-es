---
title: Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V
description: Describe las características que afectan a los componentes principales, como redes, almacenamiento, la memoria al usar Linux y FreeBSD en una máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 944f8e9d902953ab4d6da0750603a2c40fa9e96d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844896"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este artículo describe las características disponibles en los componentes, como memoria, redes, almacenamiento y los núcleos al usar Linux y FreeBSD en una máquina virtual.

## <a name="BKMK_core"></a>Core

|**Característica**|**Descripción**|
|-|-|
|Cierre integrado|Con esta característica, un administrador puede apagar las máquinas virtuales desde el Administrador de Hyper-V. Para obtener más información, consulte [apagado de sistema operativo](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Sincronización de hora|Esta característica garantiza que el tiempo de mantenimiento en una máquina virtual se mantiene sincronizado con el tiempo de mantenimiento en el host. Para obtener más información, consulte [sincronización de hora](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Hora exacta de Windows Server 2016|Esta característica permite que el invitado usar la funcionalidad de la hora exacta de Windows Server 2016, lo que mejora la sincronización de hora con el host a 1 ms precisión. Para obtener más información, consulte [hora exacta de Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Compatibilidad con Multiprocessing|Con esta característica, una máquina virtual puede usar varios procesadores en el host mediante la configuración de varias CPU virtuales.|
|Latido|Con esta característica, el host para hacer un seguimiento del estado de la máquina virtual. Para obtener más información, consulte [latido](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Compatibilidad con el mouse integrado|Con esta característica, puede usar un mouse en el escritorio de una máquina virtual y también usar el mouse sin problemas entre el escritorio de Windows Server y la consola de Hyper-V para la máquina virtual.|
|Dispositivo de almacenamiento específico de Hyper-V|Esta característica concede acceso de alto rendimiento a dispositivos de almacenamiento que estén conectados a una máquina virtual.|
|Dispositivo de red de Hyper-V|Esta característica concede acceso de alto rendimiento para los adaptadores de red que están conectados a una máquina virtual.|

## <a name="BKMK_Networking"></a>Funciones de red

|**Característica**|**Descripción**|
|-|-|
|Tramas gigantes|Con esta característica, un administrador puede aumentar el tamaño de los marcos de red más allá de 1500 bytes, lo que conduce a un aumento significativo del rendimiento de red.|
|Etiquetado de VLAN y enlace troncal|Esta característica permite configurar virtual tráfico LAN (VLAN) para las máquinas virtuales.|
|Migración en vivo|Con esta característica, puede migrar una máquina virtual de un host a otro host. Para obtener más información, consulte [información general de Virtual Machine Live Migration](https://technet.microsoft.com/library/hh831435.aspx).|
|Inserción de la dirección IP estática|Con esta característica, puede replicar la dirección IP estática de una máquina virtual después de ha conmutado por error a su réplica en un host diferente. La replicación de dicha IP garantiza que continúen funcionando sin problemas después de un evento de conmutación por error las cargas de trabajo de la red.|
|vRSS (Virtual escala de recepción)|Se distribuye la carga de un adaptador de red virtual entre varios procesadores virtuales en una máquina virtual. Para obtener más información, consulte [ajuste de escala en lado de recepción Virtual en Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Segmentación de TCP y las descargas de suma de comprobación|Segmentación de transferencias y la suma de comprobación trabajan desde la CPU de invitado para el adaptador host de red o de conmutador virtual durante las transferencias de datos de red.|
|Grandes recibir descarga (LRO)|Aumenta el rendimiento entrante de conexiones de gran ancho de banda mediante la agregación de varios paquetes en un búfer mayor, lo que reduce la sobrecarga de CPU.|
|SR-IOV|Dispositivos de E/S de raíz únicos use DDA para permitir el acceso de invitados a las partes de tarjetas NIC específicas que permite reducir la latencia y el aumento del rendimiento. SR-IOV requiere controladores de función físico al día (PF) en el host y la función virtual (VF) en el invitado.|

## <a name="BKMK_Storage"></a>Almacenamiento de información

|**Característica**|**Descripción**|
|-|-|
|Cambio de tamaño de VHDX|Con esta característica, un administrador puede cambiar el tamaño de un archivo .vhdx de tamaño fijo que se adjunta a una máquina virtual. Para obtener más información, consulte [Online Virtual Hard Disk el cambio de tamaño Overview](https://technet.microsoft.com/library/dn282286.aspx).|
|Canal de fibra virtual|Con esta característica, las máquinas virtuales puede reconocer un dispositivo de canal de fibra y montarlo de forma nativa. Para más información, vea [Información general del canal de fibra virtual de Hyper-V](https://technet.microsoft.com/library/hh831413.aspx).|
|Copia de seguridad de máquina virtual en vivo|Esta característica facilita cero hacia abajo de la copia de seguridad de máquinas virtuales activas.<br /><br />Tenga en cuenta que la operación de copia de seguridad no se realiza correctamente si la máquina virtual tiene discos duros virtuales (VHD) que se hospedan en el almacenamiento remoto como un recurso compartido de bloque de mensajes del servidor (SMB) o un volumen iSCSI. Además, asegúrese de que el destino de copia de seguridad no reside en el mismo volumen que el volumen de copia de seguridad.|
|RECORTAR el soporte técnico|Sugerencias de RECORTE notificar a la unidad que determinados sectores que se asignaron previamente ya no son necesarios para la aplicación y se pueden purgar. Este proceso se suele usar cuando una aplicación realiza las asignaciones de espacio de gran tamaño a través de un archivo y, a continuación, automática administra las asignaciones en el archivo, por ejemplo, para los archivos de disco duro virtual.|
|SCSI WWN|El controlador storvsc extrae información de World Wide Name (WWN) desde el puerto y el nodo de los dispositivos conectados a la máquina virtual y crea los archivos sysfs adecuado. |

## <a name="BKMK_Memory"></a>Memoria

|**Característica**|**Descripción**|
|-|-|
|Soporte técnico de núcleo PAE|La tecnología de extensión de dirección (PAE) física permite un kernel de 32 bits tener acceso a un espacio de dirección física es mayor que 4GB. Las distribuciones de Linux anteriores, como RHEL 5.x que se usa para enviar un kernel independiente que era la característica PAE habilitada. Distribuciones más recientes, como RHEL 6.x previamente ha integrado la compatibilidad con PAE.|
|Configuración de gap MMIO|Con esta característica, los fabricantes del dispositivo pueden configurar la ubicación de la brecha de memoria asignado E/S (MMIO). La brecha MMIO normalmente se usa para dividir la memoria física disponible entre simplemente lo suficientemente sistemas operativos de un dispositivo (JeOS) y la infraestructura de software reales que se utiliza en el dispositivo.|
|Memoria dinámica - agregado en caliente|El host de forma dinámica puede aumentar o disminuir la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento. Antes de aprovisionar, el administrador habilita la memoria dinámica en el panel de configuración de máquina Virtual y especificar la memoria de inicio, cantidad mínima de memoria y memoria máxima para la máquina virtual. Puede cambiarse cuando la máquina virtual está en la operación que no se puede deshabilitar la memoria dinámica y la configuración mínima y máxima. (Es una práctica recomendada para especificar estos tamaños de memoria como múltiplos de 128MB).<br /><br />Cuando la máquina virtual se inicia por primera vez disponible memoria es igual a la **memoria de inicio**. A medida que aumenta la demanda de memoria debido a las cargas de trabajo de aplicación Hyper-V dinámicamente puede asignar más memoria a la máquina virtual a través del mecanismo de adición sin interrupción, si es compatible con esa versión del kernel. Se limita la cantidad máxima de memoria asignada por el valor de la **memoria máxima** parámetro.<br /><br />La pestaña de la memoria del Administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria dentro de la máquina virtual mostrará la cantidad máxima de memoria asignada.<br /><br />Para obtener más información, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Memoria dinámica - incremento|El host de forma dinámica puede aumentar o disminuir la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento. Antes de aprovisionar, el administrador habilita la memoria dinámica en el panel de configuración de máquina Virtual y especificar la memoria de inicio, cantidad mínima de memoria y memoria máxima para la máquina virtual. Puede cambiarse cuando la máquina virtual está en la operación que no se puede deshabilitar la memoria dinámica y la configuración mínima y máxima. (Es una práctica recomendada para especificar estos tamaños de memoria como múltiplos de 128MB).<br /><br />Cuando la máquina virtual se inicia por primera vez disponible memoria es igual a la **memoria de inicio**. A medida que aumenta la demanda de memoria debido a las cargas de trabajo de aplicación Hyper-V dinámicamente puede asignar más memoria a la máquina virtual a través del mecanismo de adición sin interrupción (arriba). Medida que disminuye la demanda de memoria Hyper-V puede desaprovisionar automáticamente la memoria de la máquina virtual a través del mecanismo de globo. Hyper-V no desaprovisionará memoria siguiente el **cantidad mínima de memoria** parámetro.<br /><br />La pestaña de la memoria del Administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria dentro de la máquina virtual mostrará la cantidad máxima de memoria asignada.<br /><br />Para obtener más información, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Cambio de tamaño de memoria en tiempo de ejecución|Un administrador puede establecer la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento, el aumento de memoria ("caliente") o disminuirlo ("Hot quitar"). Memoria se devuelve a Hyper-V a través del controlador de globo (vea "Dinámica memoria – incremento"). El controlador de globo mantiene una cantidad mínima de memoria libre después de globos, llamado "floor", por lo que asigna memoria no puede reducirse por debajo de la demanda actual más esta cantidad de piso. La pestaña de la memoria del Administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria dentro de la máquina virtual mostrará la cantidad máxima de memoria asignada. (Es una práctica recomendada para especificar los valores de memoria como múltiplos de 128MB).|

## <a name="BKMK_Video"></a>Video

|**Característica**|**Descripción**|
|-|-|
|Dispositivo de vídeo específico de Hyper-V|Esta característica proporciona gráficos con alto rendimiento y una resolución superior para las máquinas virtuales. Este dispositivo no proporciona capacidades de modo de sesión mejorada o RemoteFX.|

## <a name="BKMK_Misc"></a>Varios

|**Característica**|**Descripción**|
|-|-|
|Exchange KVP (par de clave y valor)|Esta característica proporciona a un par clave/valor de servicio de exchange (KVP) para máquinas virtuales. Normalmente, los administradores utilizan el mecanismo KVP para operaciones de lectura y escritura de las operaciones de datos personalizados en una máquina virtual. Para obtener más información, consulte [intercambio de datos: Uso de pares clave-valor para compartir información entre el host e invitado de Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interrupción no enmascarable|Con esta característica, un administrador puede emitir interrumpe interrupción no enmascarable (NMI) a una máquina virtual. NMI es útiles para obtener los volcados de memoria de los sistemas operativos que han dejado de responder debido a errores de la aplicación. Después de reiniciar, se pueden diagnosticar estos volcados de memoria.|
|Copia de archivos de host a invitado|Esta característica permite los archivos que se copiarán sin usar el adaptador de red del equipo host físico para las máquinas virtuales invitadas. Para obtener más información, consulte [servicios invitados](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|comando lsvmbus|Este comando obtiene información acerca de los dispositivos en el bus de máquina virtual de Hyper-V (VMBus) similares a los comandos de información como lspci.|
|Sockets de Hyper-V|Se trata de un canal de comunicación adicional entre el sistema operativo host e invitado. Para cargar y usar el módulo de kernel de Sockets de Hyper-V, consulte [ofrecer sus propios servicios de integración](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Acceso directo/DDA de PCI|Con Windows Server 2016, los administradores pueden pasar a través de dispositivos PCI Express a través del mecanismo de asignación discreta de dispositivos. Dispositivos comunes son las tarjetas de red, las tarjetas gráficas y dispositivos de almacenamiento especiales. La máquina virtual requerirá que el controlador adecuado para usar el hardware expuesto. El hardware debe asignarse a la máquina virtual para que pueda usarse.<br /><br />Para obtener más información, consulte [discretos asignación de dispositivo, descripción y en segundo plano](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA es un requisito previo para las redes de SR-IOV. Puertos virtuales necesitará que se asignará a la máquina virtual y la máquina virtual debe usar los controladores de función Virtual (VF) correctos para la multiplexación de dispositivo.|

## <a name="BKMK_gen2"></a>Máquinas virtuales de generación 2

|**Característica**|**Descripción**|
|-|-|
|Arrancar con UEFI|Esta característica permite que las máquinas virtuales para que arranque con Unified Extensible Firmware Interface (UEFI).<br /><br />Para obtener más información, vea el artículo de [información general acerca de las máquinas virtuales de generación 2](https://technet.microsoft.com/library/dn282285.aspx).|
|Arranque seguro|Esta característica permite que las máquinas virtuales usar el modo de arranque seguro de UEFI que se basa. Cuando una máquina virtual se arranca en modo seguro, diversos componentes del sistema operativo se comprueban con firmas presentes en el almacén de datos UEFI.<br /><br />Para obtener más información, consulta [Arranque seguro](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Vea también

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Admite máquinas virtuales de Debian en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)