---
title: Planear la escalabilidad de Hyper-V en Windows Server 2016
description: Enumera el máximo número admitido para los componentes que puede agregar o quitar de Hyper-V y máquinas virtuales, como la cantidad de memoria y cuántos procesadores virtuales.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 03a464269c8aea29c226dee776f0dfacfe48743a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850266"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Planear la escalabilidad de Hyper-V en Windows Server 2016

> Se aplica a: Windows Server 2016
  
Este artículo proporcionan detalles sobre la configuración máxima de componentes, puede agregar y quitar en un host de Hyper-V o en sus máquinas virtuales, como procesadores virtuales o los puntos de control. Planear la implementación, tenga en cuenta los valores máximos que se aplican a cada máquina virtual, así como las que se aplican al host de Hyper-V. 

Valores máximos de memoria y procesadores lógicos son los mayores aumentos de Windows Server 2012, en respuesta a solicitudes para admitir escenarios más recientes, como machine learning y análisis de datos. Blog de Windows Server recientemente publicó los resultados de rendimiento de una máquina virtual con la versión 5.5 terabytes de memoria y 128 procesadores virtuales que se ejecuta la base de datos de 4 TB de memoria. Rendimiento era mayor del 95% del rendimiento de un servidor físico. Para obtener más información, consulte [rendimiento de máquina virtual a gran escala de Windows Server 2016 Hyper-V para el procesamiento de transacciones en memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Otros números son similares a las que se aplican a Windows Server 2012. \(Valores máximos para Windows Server 2012 R2 eran el mismo que Windows Server 2012.\) 
  
> [!NOTE]  
> Para obtener información acerca de System Center Virtual Machine Manager (VMM), consulte [de Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). VMM es un producto de Microsoft que permite administrar centros de datos virtualizados que se venden por separado.  
  
## <a name="maximums-for-virtual-machines"></a>Valores máximos para las máquinas virtuales  
Estos valores máximos se aplican a cada máquina virtual. No todos los componentes están disponibles en ambas generaciones de máquinas virtuales. ¿Para obtener una comparación de las generaciones, consulte [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Componente|Máximo|Notas|  
|-------------|-----------|---------|  
|Puntos de comprobación|50|El número real puede ser menor, ya que depende del almacenamiento disponible. Cada punto de comprobación se almacena como un archivo .avhd que usa el almacenamiento físico.|  
|Memoria|12 TB para la generación 2. <br>1 TB para la generación 1|Revise los requisitos para el sistema operativo específico a fin de determinar las cantidades mínimas y recomendadas.|  
|Puertos (COM) serie|2|Ninguno.|  
|Tamaño de los discos físicos conectados directamente a una máquina virtual|Variable|El tamaño máximo viene determinado por el sistema operativo invitado.|  
|Adaptadores de canal de fibra virtual|4|El procedimiento recomendado es conectar cada adaptador de canal de fibra virtual a una SAN virtual diferente.|  
|Unidades de disquetes virtuales|1 unidad de disquete virtual|Ninguno.|
|Capacidad de disco duro virtual|64 TB para el formato VHDX;<br>2040 GB para el formato de disco duro virtual|Cada disco duro virtual se almacena en un medio físico como un archivo .vhdx o un archivo .vhd, según el formato utilizado por el disco duro virtual.|  
|Discos IDE virtuales|4|El disco de inicio (a veces denominado disco de arranque) debe asociarse a uno de los dispositivos IDE. El disco de inicio puede ser un disco duro virtual o un disco físico conectado directamente a la máquina virtual.|  
|Procesadores virtuales|240 para la generación 2.<br>64 para la generación 1;<br>320 disponibles para el host del sistema operativo (la partición raíz)|El número de procesadores virtuales compatibles con un sistema operativo invitado podría incluso ser menor. Para obtener más información, consulte la información publicada para el sistema operativo específico.|
|Controladoras SCSI virtuales|4|Uso de dispositivos SCSI virtuales requiere integration services, que están disponibles para sistemas operativos invitados admitidos. Para obtener más información en el que se admiten sistemas operativos, consulte [máquinas virtuales Linux y FreeBSD](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) y [sistemas operativos invitados de Windows admiten](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Discos SCSI virtuales|256|Cada controladora SCSI admite hasta 64 discos y eso significa que cada máquina virtual se puede configurar con un máximo de 256 discos SCSI virtuales. (4 controladoras x 64 discos por controladora)|  
|Adaptadores de red virtuales|total de 12:<br> -8 adaptadores de red de Hyper-V específico<br>-4 adaptadores de red heredados|El adaptador de red específico de Hyper-V ofrece un mejor rendimiento y requiere un controlador incluido en integration services. Para obtener más información, consulte [Plan para las redes de Hyper-V en Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Valores máximos para los hosts de Hyper-V  
Estos valores máximos se aplican a cada host de Hyper-V.  
  
|Componente|Máximo|Notas|  
|-------------|-----------|---------|  
|Procesadores lógicos|512|Ambas opciones se deben habilitar en el firmware:<br /><br />-Virtualización asistida por hardware<br />-Prevención de ejecución de datos aplicada por hardware (DEP)<br /><br />El host del sistema operativo (la partición raíz) solo verá máximo de 320 procesadores lógicos|  
|Memoria|24 TB|Ninguno.|  
|Equipos de adaptadores de red (formación de equipos NIC)|Hyper-V no impone límites.|Para obtener más información, consulte [formación de equipos NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Adaptadores de red físicos|Hyper-V no impone límites.|Ninguno.|  
|Máquinas virtuales en ejecución por servidor|1024|Ninguno.|  
|Almacenamiento|Limitado por lo que es compatible con el sistema operativo host. Hyper-V no impone límites.|**Nota:** Microsoft admite el almacenamiento conectado a la red (NAS) cuando se usa SMB 3.0. No se admite el almacenamiento basado en NFS.|
|Puertos de conmutador de red virtual por servidor|Varía; Hyper-V no impone límites.|El límite práctico depende de los recursos informáticos disponibles.|  
|Procesadores virtuales por procesador lógico|Hyper-V no impone ninguna relación.|Ninguno.|  
|Procesadores virtuales por servidor|2048|Ninguno.|  
|Redes de área de almacenamiento (SAN) virtuales.|Hyper-V no impone límites.|Ninguno.|  
|Conmutadores virtuales|Varía; Hyper-V no impone límites.|El límite práctico depende de los recursos informáticos disponibles.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Clústeres de conmutación por error e Hyper-V  
Esta tabla enumeran los valores máximos que se aplican al usar Hyper-V y clústeres de conmutación por error. Es importante hacer para asegurarse de que haya suficientes recursos de hardware para ejecutar todas las máquinas virtuales en un entorno agrupado de planeamiento de capacidad.  

Para obtener información acerca de las actualizaciones de clústeres de conmutación por error, incluidas nuevas funciones para las máquinas virtuales, consulte [Novedades en los clústeres de conmutación por error en Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Componente|Máximo|Notas|  
|-------------|-----------|---------|  
|Nodos por clúster|64|Piense en el número de nodos que desea reservar para la conmutación por error y también para las tareas de mantenimiento, como la aplicación de actualizaciones. Recomendamos que planee los recursos suficientes para permitir que el nodo 1 quede reservado para la conmutación por error, lo que significa que permanece inactivo hasta que otro nodo conmute por error. (Algunas veces, esto se denomina nodo pasivo). Puede aumentar este número, si desea reservar más nodos. No hay ninguna relación recomendada ni un multiplicador de nodos reservados y nodos activos; el único requisito es que el número total de nodos de un clúster no puede superar el número máximo de 64.|  
|Ejecución de máquinas virtuales por clúster y por nodo|8000 por clúster|Varios factores pueden afectar el número real de máquinas virtuales que pueden ejecutar al mismo tiempo en un nodo, tales como:<br />-Cantidad de memoria física usada por cada máquina virtual.<br />-Funciones de red y el ancho de banda de almacenamiento.<br />-El número de ejes de disco, lo que afecta al rendimiento de E/S de disco.|  
  

