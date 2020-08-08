---
title: Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V
description: Describe las características que afectan a los componentes principales como redes, almacenamiento y memoria cuando se usa Linux y FreeBSD en una máquina virtual.
manager: dongill
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: b5ffb10feaa32b7dce2c491d2e7bdd1d467818fa
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989091"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

En este artículo se describen las características disponibles en componentes como núcleo, redes, almacenamiento y memoria cuando se usa Linux y FreeBSD en una máquina virtual.

## <a name="core"></a>Principal

|**Característica**|**Descripción**|
|-|-|
|Apagado integrado|Con esta característica, un administrador puede apagar las máquinas virtuales desde el administrador de Hyper-V. Para obtener más información, consulte [apagado del sistema operativo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_Shutdown).|
|Sincronización de hora|Esta característica garantiza que el tiempo de mantenimiento dentro de una máquina virtual se mantiene sincronizado con el tiempo de mantenimiento del host. Para obtener más información, consulte [sincronización de hora](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_time).|
|Windows Server 2016 hora precisa|Esta característica permite al invitado usar la funcionalidad de hora exacta de Windows Server 2016, lo que mejora la sincronización de la hora con el host a una precisión de 1 ms. Para obtener más información, consulte [hora exacta de Windows Server 2016](../../networking/windows-time-service/accurate-time.md)|
|Compatibilidad con multiproceso|Con esta característica, una máquina virtual puede usar varios procesadores en el host mediante la configuración de varias CPU virtuales.|
|Latido|Con esta característica, el host para puede realizar un seguimiento del estado de la máquina virtual. Para obtener más información, consulte [latido](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_heartbeat).|
|Compatibilidad integrada con el mouse|Con esta característica, puede usar un mouse en el escritorio de una máquina virtual y también usar el mouse sin problemas entre el escritorio de Windows Server y la consola de Hyper-V de la máquina virtual.|
|Dispositivo de almacenamiento específico de Hyper-V|Esta característica concede acceso de alto rendimiento a los dispositivos de almacenamiento que están conectados a una máquina virtual.|
|Dispositivo de red específico de Hyper-V|Esta característica concede acceso de alto rendimiento a los adaptadores de red que están conectados a una máquina virtual.|

## <a name="networking"></a>Redes

|**Característica**|**Descripción**|
|-|-|
|Tramas gigantes|Con esta característica, un administrador puede aumentar el tamaño de los fotogramas de red más allá de 1500 bytes, lo que da lugar a un aumento significativo del rendimiento de la red.|
|Etiquetado y Troncalización de VLAN|Esta característica permite configurar el tráfico de LAN virtual (VLAN) para las máquinas virtuales.|
|Migración en vivo|Con esta característica, puede migrar una máquina virtual de un host a otro. Para obtener más información, vea [información general sobre migración en vivo de máquinas virtuales](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831435(v=ws.11)).|
|Inyección de IP estática|Con esta característica, puede replicar la dirección IP estática de una máquina virtual después de que se haya conmutado por error a su réplica en otro host. Dicha replicación de IP garantiza que las cargas de trabajo de red sigan funcionando sin problemas tras un evento de conmutación por error.|
|vRSS (ajuste de escala en lado de recepción virtual)|Propaga la carga de un adaptador de red virtual entre varios procesadores virtuales en una máquina virtual. Para obtener más información, vea [ajuste de escala en lado de recepción virtual en Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383582(v=ws.11)).|
|Segmentación y descarga de sumas de comprobación TCP|Transfiere el trabajo de segmentación y suma de comprobación de la CPU invitada al conmutador virtual del host o al adaptador de red durante las transferencias de datos de red.|
|Descarga de recepción de gran tamaño (LRO)|Aumenta el rendimiento de entrada de las conexiones de ancho de banda alto al agregar varios paquetes en un búfer mayor, lo que reduce la sobrecarga de la CPU.|
|SR-IOV|Los dispositivos de e/s de raíz única usan DDA para permitir el acceso de los invitados a partes de tarjetas NIC específicas, lo que permite reducir la latencia y aumentar el rendimiento. SR-IOV requiere controladores de función física (PF) actualizados en los controladores de host y de función virtual (VF) del invitado.|

## <a name="storage"></a>Storage

|**Característica**|**Descripción**|
|-|-|
|Cambiar el tamaño de VHDX|Con esta característica, un administrador puede cambiar el tamaño de un archivo. vhdx de tamaño fijo que está conectado a una máquina virtual. Para obtener más información, vea [información general sobre el cambio de tamaño de los discos duros virtuales en línea](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282286(v=ws.11)).|
|Canal de fibra virtual|Con esta característica, las máquinas virtuales pueden reconocer un dispositivo de canal de fibra y montarlo de forma nativa. Para más información, vea [Información general del canal de fibra virtual de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831413(v=ws.11)).|
|Copia de seguridad de máquinas virtuales en vivo|Esta característica facilita la copia de seguridad de tiempo de inactividad de las máquinas virtuales en vivo.<p>Tenga en cuenta que la operación de copia de seguridad no se realiza correctamente si la máquina virtual tiene discos duros virtuales (VHD) hospedados en almacenamiento remoto, como un recurso compartido de bloque de mensajes del servidor (SMB) o un volumen iSCSI. Además, asegúrese de que el destino de copia de seguridad no resida en el mismo volumen que el volumen del que se realiza la copia de seguridad.|
|Compatibilidad con TRIM|Las sugerencias de recorte notifican a la unidad que ciertos sectores asignados previamente ya no son necesarios para la aplicación y se pueden purgar. Este proceso se suele usar cuando una aplicación realiza asignaciones de espacio grande a través de un archivo y, a continuación, administra automáticamente las asignaciones en el archivo, por ejemplo, en los archivos de disco duro virtual.|
|WWN SCSI|El controlador storvsc extrae la información del nombre World Wide Name (WWN) del puerto y el nodo de los dispositivos conectados a la máquina virtual y crea los archivos sysfs adecuados. |

## <a name="memory"></a>Memory

|**Característica**|**Descripción**|
|-|-|
|Compatibilidad con el kernel PAE|La tecnología de extensión de dirección física (PAE) permite que un kernel de 32 bits tenga acceso a un espacio de direcciones física superior a 4 GB. Las distribuciones de Linux anteriores, como RHEL 5. x, se usan para enviar un kernel independiente que estaba habilitado con PAE. Las distribuciones más recientes, como RHEL 6. x, tienen compatibilidad con PAE pregenerada.|
|Configuración de la brecha de MMIO|Con esta característica, los fabricantes de dispositivos pueden configurar la ubicación de la brecha de e/s de memoria asignada (MMIO). La brecha de MMIO se usa normalmente para dividir la memoria física disponible entre los sistemas operativos que son suficientes para el dispositivo (JeOS) y la infraestructura de software real que impulsa el dispositivo.|
|Memoria dinámica: agregar en caliente|El host puede aumentar o disminuir dinámicamente la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento. Antes del aprovisionamiento, el administrador habilita Memoria dinámica en el panel de configuración de la máquina virtual y especifica la memoria de inicio, la memoria mínima y la memoria máxima para la máquina virtual. Cuando la máquina virtual está en funcionamiento Memoria dinámica no se puede deshabilitar y solo se puede cambiar la configuración mínima y máxima. (Se recomienda especificar estos tamaños de memoria como múltiplos de 128 MB).<p>Cuando la máquina virtual se inicia por primera vez, la memoria disponible es igual a la **memoria de inicio**. A medida que aumenta la demanda de memoria debido a cargas de trabajo de aplicaciones, Hyper-V puede asignar dinámicamente más memoria a la máquina virtual a través del mecanismo de adición en caliente, si es compatible con esa versión del kernel. La cantidad máxima de memoria asignada está limitada por el valor del parámetro de **memoria máxima** .<p>La pestaña memoria del administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria de la máquina virtual mostrarán la mayor cantidad de memoria asignada.<p>Para obtener más información, vea [información general sobre memoria dinámica de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11)).<p>|
|Memoria dinámica: globos|El host puede aumentar o disminuir dinámicamente la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento. Antes del aprovisionamiento, el administrador habilita Memoria dinámica en el panel de configuración de la máquina virtual y especifica la memoria de inicio, la memoria mínima y la memoria máxima para la máquina virtual. Cuando la máquina virtual está en funcionamiento Memoria dinámica no se puede deshabilitar y solo se puede cambiar la configuración mínima y máxima. (Se recomienda especificar estos tamaños de memoria como múltiplos de 128 MB).<p>Cuando la máquina virtual se inicia por primera vez, la memoria disponible es igual a la **memoria de inicio**. A medida que aumenta la demanda de memoria debido a cargas de trabajo de aplicaciones, Hyper-V puede asignar dinámicamente más memoria a la máquina virtual a través del mecanismo de adición en caliente (anterior). A medida que la demanda de memoria disminuye, es posible que Hyper-V desaprovisione automáticamente la memoria de la máquina virtual a través del mecanismo de globo. Hyper-V no desaprovisiona la memoria por debajo del parámetro de **memoria mínima** .<p>La pestaña memoria del administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria de la máquina virtual mostrarán la mayor cantidad de memoria asignada.<p>Para obtener más información, vea [información general sobre memoria dinámica de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11)).<p>|
|Tamaño de memoria en tiempo de ejecución|Un administrador puede establecer la cantidad de memoria disponible para una máquina virtual mientras está en funcionamiento, ya sea aumentando la memoria ("adición en caliente") o disminuirá ("eliminación en caliente"). La memoria se devuelve a Hyper-V a través del controlador de globo (vea "Memoria dinámica-Balloon"). El controlador de globo mantiene una cantidad mínima de memoria libre después de los globos, denominada "FLOOR", por lo que la memoria asignada no se puede reducir por debajo de la demanda actual más esta cantidad de piso. La pestaña memoria del administrador de Hyper-V mostrará la cantidad de memoria asignada a la máquina virtual, pero las estadísticas de memoria de la máquina virtual mostrarán la mayor cantidad de memoria asignada. (Se recomienda especificar valores de memoria como múltiplos de 128 MB).|

## <a name="video"></a>Vídeo

|**Característica**|**Descripción**|
|-|-|
|Dispositivo de vídeo específico de Hyper-V|Esta característica proporciona gráficos de alto rendimiento y una resolución superior para las máquinas virtuales. Este dispositivo no proporciona las funcionalidades de RemoteFX o el modo de sesión mejorada.|

## <a name="miscellaneous"></a>Varios

|**Característica**|**Descripción**|
|-|-|
|Intercambio de KVP (par de clave-valor)|Esta característica proporciona un servicio de intercambio de pares clave-valor (KVP) para las máquinas virtuales. Normalmente, los administradores usan el mecanismo KVP para realizar operaciones de lectura y escritura de datos personalizados en una máquina virtual. Para obtener más información, vea [intercambio de datos: uso de pares de clave y valor para compartir información entre el host y el invitado en Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798287(v=ws.11)).|
|Interrupción no enmascarable|Con esta característica, un administrador puede emitir interrupciones no Enmascarables (NMI) a una máquina virtual. NMIs son útiles para obtener volcados de memoria de sistemas operativos que han dejado de responder debido a errores en la aplicación. Estos volcados de memoria se pueden diagnosticar después de reiniciar.|
|Copia de archivos de host a invitado|Esta característica permite copiar archivos del equipo físico del host a las máquinas virtuales invitadas sin usar el adaptador de red. Para obtener más información, vea [servicios invitados](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_guest).|
|comando lsvmbus|Este comando obtiene información acerca de los dispositivos del bus de máquina virtual (VMBus) de Hyper-V similar a comandos de información como lspci.|
|Sockets de Hyper-V|Se trata de un canal de comunicación adicional entre el host y el sistema operativo invitado. Para cargar y usar el módulo de kernel de sockets de Hyper-V, consulte [crear sus propios servicios de integración](/virtualization/hyper-v-on-windows/user-guide/make-integration-service).|
|Acceso directo/DDA de PCI|Con Windows Server 2016, los administradores pueden pasar a través de dispositivos PCI Express a través del mecanismo de asignación de dispositivos discretos. Los dispositivos comunes son tarjetas de red, tarjetas de gráficos y dispositivos de almacenamiento especiales. La máquina virtual necesitará que el controlador adecuado use el hardware expuesto. El hardware debe estar asignado a la máquina virtual para que se pueda usar.<p>Para obtener más información, consulte [asignación de dispositivos discretos: Descripción y fondo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<p>DDA es un requisito previo para las redes de SR-IOV. Los puertos virtuales deben asignarse a la máquina virtual y la máquina virtual debe usar los controladores de función virtual (VF) correctos para la multiplexación de dispositivos.|

## <a name="generation-2-virtual-machines"></a>Máquinas virtuales de generación 2

|**Característica**|**Descripción**|
|-|-|
|Arranque mediante UEFI|Esta característica permite que las máquinas virtuales arranquen mediante Unified Extensible Firmware Interface (UEFI).<p>Para obtener más información, vea el artículo de [información general acerca de las máquinas virtuales de generación 2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282285(v=ws.11)).|
|Arranque seguro|Esta característica permite que las máquinas virtuales usen el modo de arranque seguro basado en UEFI. Cuando una máquina virtual arranca en modo seguro, se comprueban varios componentes del sistema operativo mediante firmas presentes en el almacén de datos UEFI.<p>Para obtener más información, consulta [Arranque seguro](/previous-versions/windows/it-pro/windows-8.1-and-8/dn486875(v=ws.11)).|

## <a name="see-also"></a>Consulte también

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)