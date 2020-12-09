---
title: Configuración de Hyper-V
description: Consideraciones de configuración de Hyper-V para el ajuste del rendimiento
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 450bd8ea2b28491bdeb7adce271649b19665fa44
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864384"
---
# <a name="hyper-v-configuration"></a>Configuración de Hyper-V

## <a name="hardware-selection"></a>Selección de hardware

Las consideraciones de hardware para los servidores que ejecutan Hyper-V suelen ser similares a las de los servidores no virtualizados, pero los servidores que ejecutan Hyper-V pueden mostrar un aumento del uso de CPU, consumir más memoria y necesitar mayor ancho de banda de e/s debido a la consolidación del servidor.

-   **Procesadores**

    Hyper-V en Windows Server 2016 presenta los procesadores lógicos como uno o más procesadores virtuales para cada máquina virtual activa. Hyper-V requiere ahora procesadores que admiten tecnologías de traducción de direcciones de segundo nivel (SLAT), como tablas de páginas extendidas (EPT) o tablas de páginas anidadas (NPT).

-   **Memoria caché**

    Hyper-V puede beneficiarse de cachés de procesador más grandes, especialmente para cargas que tienen un gran espacio de trabajo en la memoria y en configuraciones de máquinas virtuales en las que la proporción de procesadores virtuales a procesadores lógicos es alta.

-   **Memoria**

    El servidor físico requiere suficiente memoria para las particiones raíz y secundaria. La partición raíz requiere memoria para realizar operaciones de e/s de forma eficaz en nombre de las máquinas virtuales y las operaciones como, por ejemplo, una instantánea de máquina virtual. Hyper-V garantiza que haya suficiente memoria disponible para la partición raíz y permite asignar la memoria restante a las particiones secundarias. Se debe ajustar el tamaño de las particiones secundarias en función de las necesidades de la carga esperada para cada máquina virtual.

-   **Storage**

    El hardware de almacenamiento debe tener suficiente ancho de banda y capacidad de e/s para satisfacer las necesidades actuales y futuras de las máquinas virtuales que hospeda el servidor físico. Tenga en cuenta estos requisitos al seleccionar controladores de almacenamiento y discos y elegir la configuración de RAID. La colocación de las máquinas virtuales con cargas de trabajo con un gran consumo de disco en diferentes discos físicos probablemente mejorará el rendimiento general. Por ejemplo, si cuatro máquinas virtuales comparten un único disco y lo usan activamente, cada máquina virtual puede producir solo el 25 por ciento del ancho de banda de ese disco.

## <a name="power-plan-considerations"></a>Consideraciones sobre el plan de energía

Como tecnología básica, la virtualización es una herramienta muy útil para aumentar la densidad de la carga de trabajo del servidor, lo que reduce el número de servidores físicos necesarios en el centro de trabajo, lo que aumenta la eficacia operativa y reduce los costos de consumo de energía. La administración de energía es fundamental para cost Management.

En un entorno de centro de información ideal, el consumo de energía se administra mediante la consolidación del trabajo en las máquinas hasta que estén más ocupados y, a continuación, desactive los equipos inactivos. Si este enfoque no es práctico, los administradores pueden aprovechar los planes de energía en los hosts físicos para asegurarse de que no consumen más energía de lo necesario.

Las técnicas de administración de energía de servidor incluyen un costo, especialmente porque las cargas de trabajo de inquilinos no son de confianza para dictar la Directiva sobre la infraestructura física del anfitrión. Se deja el software de nivel de host para deducir cómo maximizar el rendimiento a la vez que se minimiza el consumo de energía. En las máquinas que se encuentran en un estado inactivo, esto puede hacer que la infraestructura física concluya que el dibujo de energía moderado es adecuado, lo que da lugar a cargas de trabajo de inquilinos individuales que se ejecutan más lentamente de lo contrario.

Windows Server usa la virtualización en una amplia variedad de escenarios. Desde un servidor IIS con poca carga hasta un SQL Server de disponibilidad moderada, a un host de nube con Hyper-V que ejecuta cientos de máquinas virtuales por servidor. Cada uno de estos escenarios puede tener requisitos de hardware, software y rendimiento únicos. De forma predeterminada, Windows Server usa y recomienda el plan de energía **equilibrado** , que permite la conservación de energía mediante el escalado del rendimiento del procesador en función del uso de la CPU.

Con el plan de energía **equilibrado** , los Estados de energía más altos (y las latencias de respuesta más bajas en cargas de trabajo de inquilino) solo se aplican cuando el host físico está relativamente ocupado. Si tiene valor de respuesta determinista y de baja latencia para todas las cargas de trabajo de inquilinos, considere la posibilidad de cambiar del plan de energía **equilibrado** predeterminado al plan de energía de **alto rendimiento** . El plan de energía de **alto rendimiento** ejecutará los procesadores a toda velocidad, deshabilitando eficazmente Demand-Based cambio junto con otras técnicas de administración de energía y optimizará el rendimiento a través del ahorro de energía.

En el caso de los clientes, que están satisfechos con el ahorro de costos de reducir el número de servidores físicos y desean garantizar el máximo rendimiento de sus cargas de trabajo virtualizadas, considere la posibilidad de usar el plan de energía de **alto rendimiento** .

Para obtener recomendaciones adicionales y obtener información sobre cómo aprovechar los planes de energía para optimizar la infraestructura, consulte [los parámetros del plan de energía equilibrado recomendado para tiempos de respuesta rápidos](../../hardware/power/recommended-balanced-plan-parameters.md) .



## <a name="server-core-installation-option"></a>Opción de instalación Server Core

Windows Server 2016 incluyen la opción de instalación Server Core. Server Core ofrece un entorno mínimo para hospedar un conjunto seleccionado de roles de servidor, incluido Hyper-V. Presenta una superficie de disco más pequeña para el sistema operativo del host y un ataque y una superficie de servicio más pequeños. Por lo tanto, se recomienda encarecidamente que los servidores de virtualización de Hyper-V usen la opción de instalación Server Core.

Una instalación Server Core solo ofrece una ventana de consola cuando el usuario ha iniciado sesión, pero Hyper-V expone características de administración remota, incluido [Windows PowerShell](/powershell/module/hyper-v/) para que los administradores puedan administrarla de forma remota.

## <a name="dedicated-server-role"></a>Rol de servidor dedicado

La partición raíz debe estar dedicada a Hyper-V. La ejecución de roles de servidor adicionales en un servidor que ejecuta Hyper-V puede afectar negativamente al rendimiento del servidor de virtualización, especialmente si consumen una gran cantidad de CPU, memoria o ancho de banda de e/s. Minimizar los roles de servidor en la partición raíz tiene ventajas adicionales, como reducir la superficie expuesta a ataques.

Los administradores del sistema deben considerar detenidamente qué software está instalado en la partición raíz porque algún software puede afectar negativamente al rendimiento general del servidor que ejecuta Hyper-V.

## <a name="guest-operating-systems"></a>Sistemas operativos invitados

Hyper-V admite y se ha optimizado para varios sistemas operativos invitados diferentes. El número de procesadores virtuales que se admiten por invitado depende del sistema operativo invitado. Para obtener una lista de los sistemas operativos invitados admitidos, consulte [información general de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11)).

## <a name="cpu-statistics"></a>Estadísticas de CPU

Hyper-V publica los contadores de rendimiento para ayudar a caracterizar el comportamiento del servidor de virtualización e informar del uso de recursos. El conjunto estándar de herramientas para ver contadores de rendimiento en Windows incluye el monitor de rendimiento y Logman.exe, que puede mostrar y registrar los contadores de rendimiento de Hyper-V. Los nombres de los objetos de contador relevantes tienen el prefijo **Hyper-V**.

Siempre debe medir el uso de CPU del sistema físico mediante los contadores de rendimiento del procesador lógico de hipervisor de Hyper-V. Los contadores de uso de CPU que el administrador de tareas y el monitor de rendimiento notifican en las particiones raíz y secundarias no reflejan el uso de CPU físico real. Use los siguientes contadores de rendimiento para supervisar el rendimiento:

- **Procesador lógico de hipervisor de Hyper-V ( \* ) \\ % total de tiempo de ejecución** el tiempo de inactividad total de los procesadores lógicos

- **Procesador lógico de hipervisor de Hyper-V ( \* ) \\ % tiempo de ejecución del invitado** tiempo dedicado a ejecutar ciclos dentro de un invitado o en el host

- **Procesador lógico de hipervisor de Hyper-V ( \* ) \\ % tiempo de ejecución del hipervisor** el tiempo empleado en ejecutarse dentro del hipervisor

- El **procesador virtual raíz del hipervisor de Hyper-V ( \* ) \\ \\*** mide el uso de CPU de la partición raíz

- **Procesador virtual del hipervisor de Hyper- \* V \\ \\ ()*** mide el uso de CPU de las particiones de invitado


## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
