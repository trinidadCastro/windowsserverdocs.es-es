---
title: Minroot
description: Configuración de controles de recursos de CPU del host
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: de621b3bfdc9792e61e6d21d9f3774da76c55df6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860788"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Administración de recursos de CPU del host de Hyper-V

Los controles de recursos de CPU del host de Hyper-V introducidos en Windows Server 2016 o posterior permiten a los administradores de Hyper-V administrar y asignar mejor los recursos de CPU del servidor host entre la "raíz", la partición de administración y las máquinas virtuales invitadas. Con estos controles, los administradores pueden dedicar un subconjunto de los procesadores de un sistema host a la partición raíz. Esto puede separar el trabajo realizado en un host de Hyper-V de las cargas de trabajo que se ejecutan en máquinas virtuales invitadas mediante su ejecución en subconjuntos independientes de los procesadores del sistema.

Para obtener más información sobre el hardware de los hosts de Hyper-V, consulte [requisitos del sistema de Hyper-v de Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Fondo

Antes de establecer los controles para los recursos de CPU del host de Hyper-V, resulta útil revisar los conceptos básicos de la arquitectura de Hyper-V.  
Puede encontrar un resumen general en la sección [arquitectura de Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) .
Estos son los conceptos importantes de este artículo:

* Hyper-V crea y administra particiones de máquinas virtuales, en las que los recursos de proceso están asignados y compartidos, bajo el control del hipervisor.  Las particiones proporcionan límites de aislamiento fuertes entre todas las máquinas virtuales invitadas y entre las máquinas virtuales invitadas y la partición raíz.

* La partición raíz es en sí misma una partición de máquina virtual, aunque tiene propiedades únicas y privilegios mucho mayores que las máquinas virtuales invitadas.  La partición raíz proporciona los servicios de administración que controlan todas las máquinas virtuales invitadas, proporcionan compatibilidad con dispositivos virtuales para los invitados y administran todas las e/s de dispositivos para las máquinas virtuales invitadas.  Microsoft recomienda no ejecutar ninguna carga de trabajo de la aplicación en una partición del host.

* Cada procesador virtual (VP) de la partición raíz se asigna 1:1 a un procesador lógico subyacente (LP).  Un Vicepresidente de host siempre se ejecutará en el mismo LP subyacente, ya que no se realiza ninguna migración del VPs de la partición raíz.  

* De forma predeterminada, el LPs en el que se ejecuta VPs de host también puede ejecutar el VPs invitado.

* El hipervisor puede programar un Vicepresidente de invitado para que se ejecute en cualquier procesador lógico disponible.  Aunque el programador del hipervisor se encarga de considerar la ubicación de la caché temporal, la topología NUMA y muchos otros factores al programar un Vicepresidente invitado, en última instancia, el VP podría programarse en cualquier host LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>La configuración raíz mínima o "Minroot"

Las versiones anteriores de Hyper-V tenían un límite máximo de 64 VPs por partición.  Esto se aplica a las particiones raíz e invitadas.  Como los sistemas con más de 64 procesadores lógicos aparecían en servidores de tecnología avanzada, Hyper-V también ha evolucionado sus límites de escala de host para admitir estos sistemas más grandes, en un punto que admite un host con hasta 320 LPs.  Sin embargo, si se divide el límite del VP 64 por partición en ese momento, se presentaron varios desafíos y se presentaron las complejidades que eran compatibles con más de 64 VPs por partición prohibida.  Para solucionar este paso, Hyper-V limitó el número de VPs dado a la partición raíz a 64, incluso si el equipo subyacente tuviera muchos más procesadores lógicos disponibles.  El hipervisor seguiría usando todos los LPs disponibles para ejecutar VPs de invitado, pero limitaría artificialmente la partición raíz en 64.  Esta configuración se conocía como la configuración de "raíz mínima" o "minroot".  Las pruebas de rendimiento confirmaron que, incluso en sistemas de gran escala con más de 64 LPs, la raíz no necesitaba más de 64 VPs raíz para proporcionar suficiente compatibilidad con un gran número de máquinas virtuales invitadas y VPs de invitados, de hecho, mucho menor que 64 raíz VPs era a menudo adecuada, en función del número y el tamaño de las máquinas virtuales invitadas. , las cargas de trabajo específicas que se ejecutan, etc.

Este concepto "minroot" continúa utilizándose hoy en día.  De hecho, aunque Windows Server 2016 Hyper-V haya aumentado su límite máximo de compatibilidad de arquitectura para el host de LPs a 512 LPs, la partición raíz seguirá estando limitada a un máximo de 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Uso de Minroot para restringir y aislar los recursos de proceso del host
Con el umbral máximo predeterminado de 320 LPs en Windows Server 2016 Hyper-V, la configuración de minroot solo se utilizará en los sistemas de servidor más grandes.  Sin embargo, esta funcionalidad puede configurarse para un umbral mucho menor por parte del administrador del host de Hyper-V y, por tanto, aprovechar para restringir en gran medida la cantidad de recursos de CPU del host disponibles para la partición raíz.  El número específico de LPs raíz que se va a usar debe elegirse cuidadosamente para admitir las demandas máximas de las máquinas virtuales y las cargas de trabajo asignadas al host.  Sin embargo, los valores razonables para el número de hosts de host se pueden determinar mediante una minuciosa evaluación y supervisión de cargas de trabajo de producción y se validan en entornos que no son de producción antes de una implementación amplia.

## <a name="enabling-and-configuring-minroot"></a>Habilitación y configuración de Minroot

La configuración de minroot se controla a través de entradas BCD de hipervisor. Para habilitar minroot, desde un símbolo del sistema con privilegios de administrador:

```
     bcdedit /set hypervisorrootproc n
```
Donde n es el número de VPs raíz. 

El sistema debe reiniciarse y el nuevo número de procesadores raíz se conservará durante la vigencia del arranque del sistema operativo.  La configuración de minroot no se puede cambiar dinámicamente en tiempo de ejecución.

Si hay varios nodos NUMA, cada nodo obtendrá `n/NumaNodeCount` procesadores.

Tenga en cuenta que con varios nodos NUMA, debe asegurarse de que la topología de la máquina virtual sea tal que haya suficientes LPs LP (es decir, LPs sin VPs raíz) en cada nodo NUMA para ejecutar el nodo NUMA de la máquina virtual correspondiente VPs.

## <a name="verifying-the-minroot-configuration"></a>Comprobación de la configuración de Minroot

Puede comprobar la configuración de minroot del host mediante el administrador de tareas, como se muestra a continuación.

![](./media/minroot-taskman.png)

Cuando Minroot está activo, el administrador de tareas mostrará el número de procesadores lógicos asignados actualmente al host, además del número total de procesadores lógicos del sistema.
 
