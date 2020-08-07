---
title: Terminología de Hyper-V
description: Terminología de Hyper-v útil en el ajuste del rendimiento de Hyper-V
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 78552615dd67d79b8c0f4f700068fd302256a567
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896069"
---
# <a name="hyper-v-terminology"></a>Terminología de Hyper-V
En esta sección se resume la terminología clave específica de la tecnología de máquina virtual que se usa a lo largo de este tema de optimización del rendimiento:

| Término        | Definición           |
| ------------- |:------------|
|*partición secundaria* | Cualquier máquina virtual creada por la partición raíz.|
|*Virtualización de dispositivos* | Mecanismo que permite abstraer y compartir un recurso de hardware entre varios consumidores.|
|*dispositivo emulado*|Dispositivo virtualizado que imita un dispositivo de hardware físico real para que los invitados puedan usar los controladores típicos para dicho dispositivo de hardware.|
|*Habilitación*|Optimización de un sistema operativo invitado para que conozca los entornos de máquinas virtuales y ajuste su comportamiento para las máquinas virtuales.|
|*invita*|Software que se ejecuta en una partición. Puede tratarse de un sistema operativo con todas las características o de un pequeño núcleo especial. El hipervisor es independiente del invitado.|
|*hipervisor*|Una capa de software que se encuentra encima del hardware y que está por debajo de uno o más sistemas operativos. Su función principal es proporcionar entornos de ejecución aislados, llamados particiones. Cada partición tiene su propio conjunto de recursos de hardware virtualizado (unidad central de procesamiento, CPU, memoria y dispositivos). El hipervisor controla y arbitra el acceso al hardware subyacente.|
|*procesador lógico*| Una unidad de procesamiento que controla un subproceso de ejecución (secuencia de instrucciones). Puede haber uno o más procesadores lógicos por núcleo de procesador y uno o varios núcleos por socket de procesador.|
| *acceso al disco de acceso directo*|Representación de un disco físico completo como un disco virtual dentro del invitado. Los datos y los comandos se pasan al disco físico (a través de la pila de almacenamiento nativo de la partición raíz) sin ningún procesamiento intermedio de la pila virtual.|
|*partición raíz*|La partición raíz que se crea en primer lugar y posee todos los recursos que no tiene el hipervisor, incluida la mayoría de los dispositivos y la memoria del sistema. La partición raíz hospeda la pila de virtualización y crea y administra las particiones secundarias.|
|*Dispositivo específico de Hyper-V*|Un dispositivo virtualizado sin hardware físico analógico, por lo que los invitados pueden necesitar un controlador (cliente de servicio de virtualización) para ese dispositivo específico de Hyper-V. El controlador puede usar el bus de máquina virtual (VMBus) para comunicarse con el software de dispositivo virtualizado en la partición raíz.|
|*máquina virtual*|Un equipo virtual que se creó mediante la emulación de software y que tiene las mismas características que un equipo real.|
| *conmutador de red virtual*|(también denominado conmutador virtual) Una versión virtual de un conmutador de red físico. Una red virtual se puede configurar para proporcionar acceso a recursos de red locales o externos para una o más máquinas virtuales.|
|*procesador virtual*|Una abstracción virtual de un procesador que está programada para ejecutarse en un procesador lógico. Una máquina virtual puede tener uno o más procesadores virtuales.|
|*cliente del servicio de virtualización (VSC)*|Módulo de software que un invitado carga para consumir un recurso o un servicio. En el caso de los dispositivos de e/s, el cliente del servicio de virtualización puede ser un controlador de dispositivo que carga el kernel del sistema operativo.|
| *proveedor de servicios de virtualización (VSP)*|  Proveedor expuesto por la pila de virtualización en la partición raíz que proporciona recursos o servicios como e/s a una partición secundaria.|
| *pila de virtualización*|Colección de componentes de software en la partición raíz que funcionan conjuntamente para admitir máquinas virtuales. La pila de virtualización funciona con y se encuentra sobre el hipervisor. También proporciona capacidades de administración.|
|*VMBus*|Mecanismo de comunicación basado en canales que se usa para la comunicación entre particiones y la enumeración de dispositivos en sistemas con varias particiones virtualizadas activas. El VMBus se instala con los servicios de integración de Hyper-V.|

## <a name="additional-references"></a>Referencias adicionales

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
