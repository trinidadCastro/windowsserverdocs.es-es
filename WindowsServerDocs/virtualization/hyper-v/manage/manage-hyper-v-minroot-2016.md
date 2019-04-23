---
title: Minroot
description: Configuración de controles de recursos de CPU de Host
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844336"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Administración de recursos de CPU del Host de Hyper-V

Los controles de recursos de CPU de host de Hyper-V introdujeron en Windows Server 2016 o versiones posteriores permiten a los administradores de Hyper-V administrar mejor y asignar los recursos de CPU entre la "raíz", o partición de la administración y las máquinas virtuales invitadas de servidor de host. Con estos controles, los administradores pueden dedicar un subconjunto de los procesadores de un sistema host a la partición raíz. Esto puede separar el trabajo realizado en un host de Hyper-V de las cargas de trabajo que se ejecutan en máquinas virtuales invitadas ejecutándolos independiente subconjuntos de datos de los procesadores del sistema.

Para obtener más información acerca del hardware para hosts de Hyper-V, consulte [requisitos de sistema de Windows 10 Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Background

Antes de que los controles de configuración para Hyper-V alojan los recursos de CPU, es útil revisar los conceptos básicos de la arquitectura de Hyper-V.  
Puede encontrar un resumen general de la [arquitectura Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) sección.
Estos son conceptos importantes de este artículo:

* Hyper-V crea y administra las particiones de máquina virtual, en qué proceso recursos asignados y compartidos, bajo el control del hipervisor.  Las particiones proporcionan límites de aislamiento sólido entre todas las máquinas virtuales de invitado y entre las máquinas virtuales invitadas y la partición raíz.

* La partición raíz es una partición de la máquina virtual, aunque tiene propiedades únicas y mucho más privilegios que las máquinas virtuales invitadas.  La partición raíz proporciona los servicios de administración que controlan todas las máquinas virtuales de invitado, proporciona compatibilidad con dispositivos virtuales para los invitados y administra todas las E/S del dispositivo para las máquinas virtuales invitadas.  Microsoft recomienda encarecidamente no ejecuta las cargas de trabajo de aplicación en una partición del host.

* Cada procesador virtual (VP) de la partición raíz es 1:1 asignada a un procesador lógico subyacente (LP).  Un host VP siempre se ejecutará en el mismo LP subyacente: no hay ninguna migración de VPs la partición raíz.  

* De forma predeterminada, el LPs en el que se ejecutan VPs host también pueden ejecutar VPs de invitado.

* Puede programar un Vicepresidente de invitado el hipervisor para ejecutarse en cualquier procesador lógico disponible.  Mientras que el programador del hipervisor se encarga de considerar la localidad de memoria caché temporal, topología NUMA y muchos otros factores al programar un Vicepresidente de invitado, en última instancia, el Vicepresidente podría programarse en cualquier host LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>La configuración de "Minroot" o raíz mínimo

Las versiones anteriores de Hyper-V tenían un límite máximo de arquitectura de 64 VPs por partición.  Esto se aplica a las particiones de la raíz y el invitado.  Que aparecían en sistemas con más de 64 procesadores lógicos en servidores de gama alta, Hyper-V también ha evolucionado a sus límites de escalado de host para admitir estos sistemas de mayor tamaño, en un punto que admiten un host con hasta 320 LPs.  Sin embargo, interrumpir el 64 VP por límite de partición en ese momento presenta varios desafíos e introdujo las complejidades que realizó la compatibilidad con más de 64 VPs por partición prohibitivo.  Para solucionar este problema, Hyper-V limitado el número de VPs dado a la partición raíz a 64, incluso si la máquina subyacente tenía muchos procesadores lógicos más disponibles.  El hipervisor podría seguir usando todas LPs disponibles para ejecutar invitados VPs, pero limita artificialmente la partición raíz en 64.  Esta configuración se pasó a ser conocida como "mínimo" raíz"o"minroot"configuración.  Las pruebas de rendimiento confirmaron que, incluso en sistemas de gran escala con más de 64 LPs, la raíz no necesitaban más de 64 VPs raíz para proporcionar soporte técnico suficiente para un gran número de máquinas virtuales invitadas y VPs invitado: de hecho, mucho menor que 64 raíz VPs solía ser adecuados , por supuesto en función del número y tamaño de las máquinas virtuales invitadas, las cargas de trabajo específicas que se va a ejecutar, etcetera.

Este concepto de "minroot" continúa que será usado hoy en día.  De hecho, incluso cuando Windows Server 2016 Hyper-V había aumentado su límite máximo de apoyo arquitectónico para host LPs a 512 LPs, la partición raíz todavía se limita a un máximo de 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Uso de Minroot para restringir y aislar los recursos de proceso de Host
Con el umbral alto predeterminado de 320 LPs en Windows Server 2016 Hyper-V, solo se usará la configuración de minroot en los sistemas de servidor muy grandes.  Sin embargo, esta funcionalidad puede configurada para un cantidad de umbral inferior por el administrador del host de Hyper-V y, por tanto, aprovecharse para restringir en gran medida la cantidad de recursos de CPU de host disponibles para la partición raíz.  El número específico de LPs raíz utilizar supuesto se debe elegir cuidadosamente para admitir la demanda máxima de las máquinas virtuales y las cargas de trabajo asignados al host.  Sin embargo, pueden ser valores razonables para el número de LPs host determinado a través de evaluación minuciosa y supervisión de cargas de trabajo de producción y validado en entornos que no sea de producción antes de la implementación amplia.

## <a name="enabling-and-configuring-minroot"></a>Habilitar y configurar Minroot

La configuración de minroot se controla a través de las entradas de BCD del hipervisor. Para habilitar minroot desde un símbolo del sistema con privilegios de administrador:

```
    bcdedit /set hypervisorrootproc n
```
Donde n es el número de raíz VPs. 

Se debe reiniciar el sistema y el nuevo número de procesadores de raíz se conservará durante la vigencia de arranque del sistema operativo.  La configuración de minroot no se puede cambiar dinámicamente en tiempo de ejecución.

Si hay varios nodos NUMA, cada nodo obtendrá `n/NumaNodeCount` procesadores.

Tenga en cuenta que con varios nodos NUMA, debe asegurarse de que topología de la máquina virtual es tal que hay suficiente LPs libres (es decir, LPs sin raíz VPs) en cada nodo NUMA para ejecutar nodos de NUMA de la máquina virtual correspondiente VPs.

## <a name="verifying-the-minroot-configuration"></a>Comprobación de la configuración de Minroot

Puede comprobar la configuración del host minroot mediante el Administrador de tareas, como se muestra a continuación.

![](./media/minroot-taskman.png)

Cuando Minroot está activo, el Administrador de tareas mostrará el número de procesadores lógicos actualmente asignado al host, además del número total de procesadores lógicos en el sistema.
 
