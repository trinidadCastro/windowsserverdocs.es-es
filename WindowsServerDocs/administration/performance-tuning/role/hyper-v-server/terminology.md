---
title: Terminología de Hyper-v.
description: Terminología de Hyper-v útil para la optimización del rendimiento de Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841146"
---
# <a name="hyper-v-terminology"></a>Terminología de Hyper-v.
En esta sección se resume la terminología clave específico de tecnología de máquina virtual que se utiliza a lo largo de este tema de optimización del rendimiento:

| Término        | Definición           |
| ------------- |:------------|
|*partición secundaria* | Cualquier máquina virtual que se crea mediante la partición raíz.|
|*virtualización de Device* | Un mecanismo que permite un hardware recursos se abstraen y compartir entre varios consumidores.|
|*dispositivo emulado*|Un dispositivo virtualizado que se asemeja a un dispositivo de hardware físico real para que los invitados pueden usar los controladores típicos para ese dispositivo de hardware.|
|*enlightenment*|Optimización de un sistema operativo para que sea consciente de los entornos de máquina virtual y ajustar su comportamiento para las máquinas virtuales.|
|*guest*|Software que se ejecuta en una partición. Puede ser un sistema operativo completo o un kernel pequeño, en especial. El hipervisor es independiente del invitado.|
|*hypervisor*|Una capa de software que se encuentra sobre el hardware y por debajo de uno o más sistemas operativos. Su función principal es proporcionar entornos de ejecución aislados, llamados particiones. Cada partición tiene su propio conjunto de recursos de hardware virtualizado (unidad central de procesamiento o CPU, memoria y dispositivos). El hipervisor controla y arbitra el acceso al hardware subyacente.|
|*procesador lógico*| Una unidad de procesamiento que controla un subproceso de ejecución (secuencia de instrucciones). Puede haber uno o varios procesadores lógicos por núcleo de procesador y uno o varios núcleos por socket de procesador.|
| *acceso al disco de acceso directo*|Una representación de todo un disco físico como un disco virtual en el invitado. Los datos y los comandos se pasan a través de en el disco físico (a través de la pila de almacenamiento nativo de la partición raíz) con ningún procesamiento que intervienen por la pila virtual.|
|*partición raíz*|La partición raíz que se crea en primer lugar y posee todos los recursos que el hipervisor no lo haga, incluidos la mayoría de los dispositivos y memoria del sistema. La partición raíz hospeda en la pila de virtualización y se crea y administra las particiones secundarias.|
|*Dispositivo específicos de Hyper-V*|Un dispositivo virtualizado con ningún analógico de hardware físico, los invitados por lo que quizá necesite un controlador (cliente de servicio de virtualización) en el dispositivo específico de Hyper-V. El controlador puede usar el bus de máquina virtual (VMBus) para comunicarse con el software del dispositivo virtualizado en la partición raíz.|
|*Máquina virtual*|Un equipo virtual que se creó mediante la emulación de software y tiene las mismas características que un equipo real.|
| *conmutador de red virtual*|(también denominados un conmutador virtual) Una versión virtual de un conmutador de red físico. Una red virtual se puede configurar para proporcionar acceso a recursos de red locales o externos para una o más máquinas virtuales.|
|*procesador virtual*|Una abstracción de virtual de un procesador que está programada para ejecutarse en un procesador lógico. Una máquina virtual puede tener uno o más procesadores virtuales.|
|*cliente de servicio de virtualización (VSC)*|Un módulo de software que se carga un invitado para consumir un recurso o servicio. Para dispositivos de E/S, el cliente del servicio de virtualización puede ser un controlador de dispositivo que se carga el kernel del sistema operativo.|
| *proveedor de servicios de virtualización (VSP)*|  Un proveedor expuesto por la pila de virtualización en la partición raíz proporciona recursos o servicios como la E/S a una partición secundaria.|
| *pila de virtualización*|Una colección de componentes de software en la partición raíz que funcionan conjuntamente para admitir las máquinas virtuales. La pila de virtualización funciona con y se encuentra por encima del hipervisor. También proporciona capacidades de administración.|
|*VMBus*|Mecanismo de comunicación de basado en canal utilizado para la enumeración de comunicación y el dispositivo entre particiones en los sistemas con varias particiones virtualizadas activas. El VMBus se instala con los servicios de integración de Hyper-V.|

## <a name="see-also"></a>Vea también

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
