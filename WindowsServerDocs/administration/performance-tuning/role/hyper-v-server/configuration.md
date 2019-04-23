---
title: Configuración de Hyper-V
description: Consideraciones sobre la configuración de Hyper-V para la optimización de rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e5e55ad492439fcb7150469d9a35b639f5ff9a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830516"
---
# <a name="hyper-v-configuration"></a>Configuración de Hyper-V

## <a name="hardware-selection"></a>Selección de hardware

Las consideraciones de hardware para servidores que ejecutan Hyper-V normalmente se parecen a las de los servidores no virtualizados, pero los servidores que ejecutan Hyper-V pueden presentar un mayor uso de CPU, consume más memoria y necesita mayor ancho de banda de E/S debido a la consolidación de servidores.

-   **Procesadores**

    Hyper-V en Windows Server 2016 presenta los procesadores lógicos como uno o más procesadores virtuales a cada máquina virtual activa. Hyper-V requiere ahora procesadores que admiten las tecnologías de traducción de direcciones de segundo nivel (SLAT) como las tablas de página extendida (EPT) o las tablas de página anidadas (NPT).

-   **Cache**

    Hyper-V puede beneficiarse de mayores memorias caché de procesador, especialmente para las cargas que tienen un gran espacio de trabajo en memoria y en configuraciones de máquina virtual en el que la proporción de procesadores virtuales y los procesadores lógicos es alta.

-   **Memoria**

    El servidor físico requiere suficiente memoria para las particiones de la raíz y el secundario. La partición raíz necesita memoria para realizar las operaciones de E/s de manera eficaz en nombre de las máquinas virtuales y las operaciones como una instantánea de máquina virtual. Hyper-V garantiza que haya memoria suficiente está disponible para la partición raíz y permite restantes de la memoria que se asignará a las particiones secundarias. Las particiones secundarias deben ajustarse según las necesidades de la carga esperada para cada máquina virtual.

-   **Almacenamiento**

    El hardware de almacenamiento debe tener suficiente ancho de banda de E/S y la capacidad para satisfacer las necesidades actuales y futuras de virtual machines que los hosts de servidor físico. Tenga en cuenta estos requisitos cuando seleccione discos y controladores de almacenamiento y elija la configuración de RAID. Colocación de máquinas virtuales con cargas de trabajo intensivas de disco alta en distintos discos físicos probablemente mejorará el rendimiento general. Por ejemplo, si cuatro máquinas virtuales comparten un único disco y usarlo de forma activa, cada máquina virtual pueden producir solo el 25 por ciento del ancho de banda de dicho disco.

## <a name="power-plan-considerations"></a>Consideraciones del plan de energía

Como una tecnología principal, la virtualización es una eficaz herramienta útil para aumentar la densidad de carga de trabajo de servidor, lo que reduce el número de servidores físicos necesarios en su centro de datos, aumentar la eficacia operativa y reduce los costos de consumo de energía. Administración de energía es fundamental para la administración de costos. 

En un entorno ideal del centro de datos, consumo de energía se administra mediante la consolidación de trabajo en equipos hasta que está prácticamente ocupados y, a continuación, al desactivar máquinas inactivas. Si este enfoque no es práctico, los administradores pueden aprovechar los planes de energía en los hosts físicos para asegurarse de que no consumen más energía que es necesario. 

Las técnicas de administración de energía de servidor conlleva un costo, especialmente como inquilino cargas de trabajo no son de confianza para dictar directiva sobre la infraestructura física del proveedor de hospedaje. El software de nivel de host se deja para deducir cómo maximizar el rendimiento minimizando el consumo de energía. En máquinas inactivo principalmente, esto puede causar la infraestructura física concluir es adecuado, lo que las cargas de trabajo de inquilinos individuales ejecutan más despacio de lo contrario, puede que dicho consumo de energía moderados.

Windows Server utiliza la virtualización en una amplia variedad de escenarios. Desde un servidor IIS con poca carga a un servidor SQL moderadamente ocupado, en un host en la nube con Hyper-V que ejecuta cientos de máquinas virtuales por servidor. Cada uno de estos escenarios puede tener requisitos de hardware, software y rendimiento únicos. De forma predeterminada, usa Windows Server y recomienda la **equilibrado** plan de energía, lo que permite la conservación de energía mediante el escalado de rendimiento del procesador en función del uso de CPU.

Con el **equilibrado** plan de energía, los Estados de energía más altos (y latencias de respuesta más bajas en las cargas de trabajo de inquilino) se aplican solo cuando el host físico es relativamente ocupado. Si el valor determinista, baja latencia de respuesta para todas las cargas de trabajo de inquilino, considere la posibilidad de cambiar el valor predeterminado **equilibrado** plan de energía para el **de alto rendimiento** plan de energía. El **de alto rendimiento** plan de energía ejecutará los procesadores a máxima velocidad todo el tiempo, deshabilitando conmutación basada en demanda junto con otras técnicas de administración de energía y optimizar el rendimiento a través de ahorro de energía.

Para los clientes, que están satisfecho con el ahorro de costos de reducir el número de servidores físicos y desean asegurarse de que obtener el máximo rendimiento para sus cargas de trabajo virtualizadas, debe considerar el uso del **de alto rendimiento** plan de energía.

Para obtener más recomendaciones e información sobre el aprovechamiento de los planes de energía para optimizar su infraestructura, leer [recomienda equilibrada Power planear parámetros para los tiempos de respuesta rápida](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Opción de instalación Server Core

La opción de instalación Server Core de características de Windows Server 2016. Server Core ofrece un entorno mínimo para un conjunto seleccionado de roles de servidor, incluido Hyper-V de hospedaje. Ofrece un espacio de disco más pequeño para el sistema operativo host y un ataque más pequeño y mantenimiento de la superficie. Por lo tanto, recomendamos encarecidamente que los servidores de virtualización de Hyper-V usen la opción de instalación Server Core.

Una instalación Server Core ofrece una ventana de consola sólo cuando el usuario haya iniciado sesión, pero Hyper-V expone las características de administración remota como [Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx) por lo que los administradores pueden administrarlo de forma remota.

## <a name="dedicated-server-role"></a>Rol de servidor dedicado

La partición raíz debe estar dedicada a Hyper-V. Ejecuta otros roles de servidor en un servidor que ejecuta Hyper-V puede afectar negativamente al rendimiento del servidor de virtualización, especialmente si se consumen mucho ancho de banda de CPU, memoria o E/S. Minimizar los roles de servidor en la partición raíz tiene ventajas adicionales como la reducción de la superficie de ataque.

Los administradores del sistema deben considerar detenidamente qué software está instalado en la partición raíz porque algunos programas de software pueden afectar negativamente al rendimiento general del servidor que ejecuta Hyper-V.

## <a name="guest-operating-systems"></a>Sistemas operativos invitados

Hyper-V admite y se ha ajustado para un número de sistemas operativos invitados diferentes. El número de procesadores virtuales que se admiten por invitado depende del sistema operativo invitado. Para obtener una lista de los sistemas operativos invitados admitidos, consulte [Introducción a Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Estadísticas de CPU

Hyper-V publica contadores de rendimiento para ayudar a caracterizar el comportamiento del servidor de virtualización y el uso de recursos de informes. El conjunto estándar de herramientas para ver los contadores de rendimiento en Windows incluye Monitor de rendimiento y Logman.exe, que se pueden mostrar y registrar los contadores de rendimiento de Hyper-V. Los nombres de los objetos de contador pertinentes tienen el prefijo **Hyper-V**.

Siempre debe medir el uso de CPU del sistema físico mediante los contadores de rendimiento de procesador lógico del hipervisor de Hyper-V. El uso de CPU de contadores de ese informe del Administrador de tareas y el Monitor de rendimiento en la raíz y las particiones secundarias no reflejan el uso de CPU físico real. Use los siguientes contadores de rendimiento para supervisar el rendimiento:

-   **Procesadores lógicos del hipervisor de Hyper-V (\*)\\% de tiempo de ejecución Total** el tiempo no inactivo total de los procesadores lógicos

-   **Procesadores lógicos del hipervisor de Hyper-V (\*)\\% tiempo de ejecución de invitado** el tiempo invertido en ciclos de ejecución dentro de un invitado o el host

-   **Procesadores lógicos del hipervisor de Hyper-V (\*)\\% tiempo de ejecución del hipervisor** el tiempo dedicado a ejecutar en el hipervisor

-   **Procesador Virtual de raíz de hipervisor de Hyper-V (\*)\\ \***  mide el uso de CPU de la partición raíz

-   **Procesador Virtual del hipervisor de Hyper-V (\*)\\ \***  mide el uso de CPU de las particiones de invitado


## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
