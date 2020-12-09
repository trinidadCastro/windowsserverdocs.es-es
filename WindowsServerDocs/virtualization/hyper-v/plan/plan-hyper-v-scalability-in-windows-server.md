---
title: Planear la escalabilidad de Hyper-V en Windows Server 2016 y Windows Server 2019
description: Muestra el número máximo admitido de componentes que puede Agregar o quitar de Hyper-V y máquinas virtuales, como la cantidad de memoria y el número de procesadores virtuales.
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.date: 09/28/2016
ms.openlocfilehash: 5a18aad091314543dece0c0270935be020f6810b
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864564"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016-and-windows-server-2019"></a>Planear la escalabilidad de Hyper-V en Windows Server 2016 y Windows Server 2019

> Se aplica a: Windows Server 2016 y Windows Server 2019

En este artículo se proporcionan detalles acerca de la configuración máxima de los componentes que se pueden agregar y quitar en un host de Hyper-V o en sus máquinas virtuales, como los procesadores virtuales o los puntos de control. Cuando planee la implementación, tenga en cuenta los valores máximos que se aplican a cada máquina virtual, así como los que se aplican al host de Hyper-V.

Los valores máximos para la memoria y los procesadores lógicos son los mayores aumentos de Windows Server 2012, en respuesta a las solicitudes para admitir escenarios más recientes, como el aprendizaje automático y el análisis de datos. El blog de Windows Server publicó recientemente los resultados de rendimiento de una máquina virtual con 5,5 terabytes de memoria y 128 procesadores virtuales con una base de datos en memoria de 4 TB. El rendimiento era superior al 95% del rendimiento de un servidor físico. Para obtener más información, consulte [rendimiento de máquinas virtuales de gran escala de Hyper-V de Windows Server 2016 para el procesamiento de transacciones en memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Otros números son similares a los que se aplican a Windows Server 2012. \(Los máximos para Windows Server 2012 R2 eran los mismos que Windows Server 2012.\)

> [!NOTE]
> Para obtener más información acerca de System Center Virtual Machine Manager (VMM), consulte [Virtual Machine Manager](/system-center/vmm/overview). VMM es un producto de Microsoft que permite administrar centros de datos virtualizados que se venden por separado.

## <a name="maximums-for-virtual-machines"></a>Máximo de máquinas virtuales
Estos valores máximos se aplican a cada máquina virtual. No todos los componentes están disponibles en ambas generaciones de máquinas virtuales. Para ver una comparación de las generaciones, consulte ¿ [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)

|Componente|Máxima|Notas|
|-------------|-----------|---------|
|Puntos de control|50|El número real puede ser menor, ya que depende del almacenamiento disponible. Cada punto de control se almacena como un archivo. avhd que usa el almacenamiento físico.|
|Memoria|12 TB para la generación 2; <br>1 TB para la generación 1|Revise los requisitos para el sistema operativo específico a fin de determinar las cantidades mínimas y recomendadas.|
|Puertos (COM) serie|2|Ninguno.|
|Tamaño de los discos físicos conectados directamente a una máquina virtual|Varía|El tamaño máximo viene determinado por el sistema operativo invitado.|
|Adaptadores de canal de fibra virtual|4|El procedimiento recomendado es conectar cada adaptador de canal de fibra virtual a una SAN virtual diferente.|
|Unidades de disquetes virtuales|1 unidad de disquete virtual|Ninguno.|
|Capacidad de disco duro virtual|64 TB para el formato VHDX;<br>2040 GB para el formato VHD|Cada disco duro virtual se almacena en un medio físico como un archivo .vhdx o un archivo .vhd, según el formato utilizado por el disco duro virtual.|
|Discos IDE virtuales|4|El disco de inicio (a veces denominado disco de arranque) debe estar conectado a uno de los dispositivos IDE. El disco de inicio puede ser un disco duro virtual o un disco físico conectado directamente a la máquina virtual.|
|Procesadores virtuales|240 para la generación 2;<br>64 para la generación 1;<br>320 disponible para el sistema operativo del host (partición raíz)|El número de procesadores virtuales compatibles con un sistema operativo invitado podría incluso ser menor. Para obtener más información, consulte la información publicada para el sistema operativo específico.|
|Controladoras SCSI virtuales|4|El uso de dispositivos SCSI virtuales requiere Integration Services, que están disponibles para los sistemas operativos invitados admitidos. Para obtener más información sobre qué sistemas operativos se admiten, consulte [máquinas virtuales Linux y FreeBSD compatibles](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) y [los sistemas operativos invitados de Windows admitidos](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|
|Discos SCSI virtuales|256|Cada controladora SCSI admite hasta 64 discos y eso significa que cada máquina virtual se puede configurar con un máximo de 256 discos SCSI virtuales. (4 controladoras x 64 discos por controladora)|
|Adaptadores de red virtuales|Windows Server 2016 admite 12 en total:<br> -8 adaptadores de red específicos de Hyper-V<br>-4 adaptadores de red heredados <br> Windows Server 2019 admite 68 en total: <br> -64 adaptadores de red específicos de Hyper-V<br>-4 adaptadores de red heredados  |El adaptador de red específico de Hyper-V proporciona un mejor rendimiento y requiere un controlador incluido en Integration Services. Para obtener más información, vea [planear la red de Hyper-V en Windows Server](plan-hyper-v-networking-in-windows-server.md).|

## <a name="maximums-for-hyper-v-hosts"></a>Máximos para hosts de Hyper-V
Estos valores máximos se aplican a cada host de Hyper-V.

|Componente|Máxima|Notas|
|-------------|-----------|---------|
|Procesadores lógicos|512|Ambos deben estar habilitados en el firmware:<p>-Virtualización asistida por hardware<br />-Prevención de ejecución de datos (DEP) forzada por hardware<p>El sistema operativo del host (partición raíz) solo verá los procesadores lógicos 320 máximos|  
|Memoria|24 TB|Ninguno.|
|Equipos de adaptadores de red (formación de equipos NIC)|Hyper-V no impone límites.|Para obtener más información, consulte [formación de equipos NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|
|Adaptadores de red físicos|Hyper-V no impone límites.|Ninguno.|
|Máquinas virtuales en ejecución por servidor|1024|Ninguno.|
|Storage|Limitado por lo que admite el sistema operativo host. Hyper-V no impone límites.|**Nota:** Microsoft admite el almacenamiento conectado a la red (NAS) cuando se usa SMB 3,0. No se admite el almacenamiento basado en NFS.|
|Puertos de conmutador de red virtual por servidor|Varía; Hyper-V no impone límites.|El límite práctico depende de los recursos informáticos disponibles.|
|Procesadores virtuales por procesador lógico|Hyper-V no impone ninguna relación.|Ninguno.|
|Procesadores virtuales por servidor|2048|Ninguno.|
|Redes de área de almacenamiento (SAN) virtuales.|Hyper-V no impone límites.|Ninguno.|
|Conmutadores virtuales|Varía; Hyper-V no impone límites.|El límite práctico depende de los recursos informáticos disponibles.|

## <a name="failover-clusters-and-hyper-v"></a>Clústeres de conmutación por error e Hyper-V
En esta tabla se enumeran los valores máximos que se aplican al usar Hyper-V y los clústeres de conmutación por error. Es importante realizar un planeamiento de la capacidad para asegurarse de que habrá suficientes recursos de hardware para ejecutar todas las máquinas virtuales en un entorno en clúster.

Para obtener información acerca de las actualizaciones de los clústeres de conmutación por error, incluidas las nuevas características de las máquinas virtuales, consulte [novedades de los clústeres de conmutación por error en Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Componente|Máxima|Notas|
|-------------|-----------|---------|
|Nodos por clúster|64|Piense en el número de nodos que desea reservar para la conmutación por error y también para las tareas de mantenimiento, como la aplicación de actualizaciones. Recomendamos que planee los recursos suficientes para permitir que el nodo 1 quede reservado para la conmutación por error, lo que significa que permanece inactivo hasta que otro nodo conmute por error. (Esto se conoce a veces como nodo pasivo). Puede aumentar este número si desea reservar nodos adicionales. No hay ninguna relación recomendada o multiplicador de nodos reservados a nodos activos. el único requisito es que el número total de nodos de un clúster no supere el máximo de 64.|
|Ejecución de máquinas virtuales por clúster y por nodo|8000 por clúster|Varios factores pueden afectar al número real de máquinas virtuales que se pueden ejecutar al mismo tiempo en un nodo, como:<br />-Cantidad de memoria física usada por cada máquina virtual.<br />-Redes y ancho de banda de almacenamiento.<br />-Número de ejes de disco, lo que afecta al rendimiento de e/s de disco.|
