---
title: Las puertas de enlace de escritorio remotos de optimización del rendimiento
description: Recomendaciones para las puertas de enlace de escritorio remoto de optimización del rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 70b27d45acbfb046d52271a50ca7deffb226b8d0
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266729"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Las puertas de enlace de escritorio remotos de optimización del rendimiento

> [!Note]
> En Windows 8 y versiones posteriores y Windows Server 2012 R2 y versiones posteriores, puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) es compatible con TCP, UDP y los transportes RPC heredados. La mayoría de los siguientes datos está relacionado con el transporte RPC heredado. Si no se utiliza el transporte RPC heredado, esta sección no es aplicable.

En este tema se describe los parámetros relacionados con el rendimiento que ayudan a mejorar el rendimiento de una implementación de cliente y los ajustes que se basan en patrones de uso de red del cliente.

En esencia, la puerta de enlace de escritorio remoto lleva a cabo muchas operaciones entre las instancias de la conexión a Escritorio remoto y las instancias del servidor Host de sesión de escritorio remoto en la red del cliente de reenvío.

> [!Note]
> Los parámetros siguientes se aplican a sólo el transporte RPC.

Internet Information Services (IIS) y la puerta de enlace de escritorio remoto de exportación de los parámetros del registro siguientes para ayudar a mejorar el rendimiento del sistema en la puerta de enlace de escritorio remoto.

**Ajustes del subproceso**

-   **MaxIoThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Este grupo de subprocesos específicos de la aplicación especifica el número de subprocesos que crea la puerta de enlace de escritorio remoto para controlar las solicitudes entrantes. Si este valor del registro está presente, entrará en vigor. El número de subprocesos es igual al número de procesos lógicos. Si el número de procesadores lógicos es menor que 5, el valor predeterminado es 5 subprocesos.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Este parámetro especifica el número de subprocesos del grupo IIS pueden crear por cada procesador lógico. El grupo de subprocesos IIS inspeccionar la red para las solicitudes y procesa todas las solicitudes entrantes. El **MaxPoolThreads** recuento no incluye los subprocesos que consume la puerta de enlace de escritorio remoto. El valor predeterminado es 4.

**Ajustes de llamada a procedimiento remoto para la puerta de enlace de escritorio remoto**

Los parámetros siguientes pueden ayudarle a ajustar las llamadas a procedimiento remoto (RPC) que se reciben los equipos de puerta de enlace de escritorio remoto y conexión a Escritorio remoto. Cambiar el windows ayuda a limitar la cantidad de datos se envíen a través de cada conexión y puede mejorar el rendimiento de RPC a través de escenarios HTTP v2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    El valor predeterminado es 64 KB. Este valor especifica el período que el servidor utiliza para los datos que se reciben desde el proxy RPC. El valor mínimo se establece en 8 KB y el valor máximo se establece en 1 GB. Si un valor no está presente, se usa el valor predeterminado. Cuando se realizan cambios en este valor, IIS debe reiniciarse para que el cambio surta efecto.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    El valor predeterminado es 64 KB. Este valor especifica el período que el cliente utiliza para los datos que se reciben desde el proxy RPC. El valor mínimo es de 8 KB y el valor máximo es 1 GB. Si un valor no está presente, se usa el valor predeterminado.

## <a name="monitoring-and-data-collection"></a>Supervisión y recopilación de datos


La siguiente lista de contadores de rendimiento se considera un conjunto básico de contadores al supervisar el uso de recursos en la puerta de enlace de escritorio remoto:

-   \\Terminal Service Gateway\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP por servidor\\\*

-   \\Servicio Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\Memoria\\\*

-   \\Interfaz de red (\*)\\\*

-   \\Proceso (\*)\\\*

-   \\Información del procesador (\*)\\\*

-   \\Sincronización (\*)\\\*

-   \\Sistema\\\*

-   \\TCPv4\\\*

Los siguientes contadores de rendimiento son aplicables solo para el transporte RPC antiguo:

-   \\Proxy RPC/HTTP\\ \* RPC

-   \\Proxy RPC/HTTP por servidor\\ \* RPC

-   \\Servicio Web\\ \* RPC

-   \\W3SVC\_W3WP\\\* RPC

**Tenga en cuenta**    si procede, agregue el \\IPv6\\ \* y \\TCPv6\\ \* objetos. ReplaceThisText

 
