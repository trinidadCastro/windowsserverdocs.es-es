---
title: Puertas de enlace de Escritorio remoto de optimización del rendimiento
description: Recomendaciones para la optimización del rendimiento para puertas de enlace de Escritorio remoto
ms.topic: article
ms.author: hammadbu
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 34305b94ff431f90d152bfd987c827b990fb0e74
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078172"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Puertas de enlace de Escritorio remoto de optimización del rendimiento

> [!NOTE]
> En Windows 8 + y Windows Server 2012 R2 +, la puerta de enlace de Escritorio remoto (puerta de enlace de escritorio remoto) admite TCP, UDP y los transportes de RPC heredados. La mayoría de los datos siguientes se refieren al transporte RPC heredado. Si no se utiliza el transporte RPC heredado, esta sección no es aplicable.

En este tema se describen los parámetros relacionados con el rendimiento que ayudan a mejorar el rendimiento de la implementación de un cliente y las optimizaciones que se basan en los patrones de uso de red del cliente.

En su núcleo, la puerta de enlace de escritorio remoto realiza muchas operaciones de reenvío de paquetes entre Conexión a Escritorio remoto instancias y las instancias de servidor host de sesión de escritorio remoto dentro de la red del cliente.

> [!NOTE]
> Los parámetros siguientes se aplican solo al transporte RPC.

Internet Information Services (IIS) y puerta de enlace de escritorio remoto exportan los siguientes parámetros del registro para ayudar a mejorar el rendimiento del sistema en la puerta de enlace de escritorio remoto.

**Optimizaciones de subprocesos**

-   **MaxIOThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Este grupo de subprocesos específicos de la aplicación especifica el número de subprocesos que la puerta de enlace de escritorio remoto crea para controlar las solicitudes entrantes. Si esta configuración del registro está presente, surte efecto. El número de subprocesos es igual al número de procesos lógicos. Si el número de procesadores lógicos es menor que 5, el valor predeterminado es 5 subprocesos.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Este parámetro especifica el número de subprocesos del grupo de IIS que se va a crear por procesador lógico. Los subprocesos del grupo de IIS ven la red en busca de solicitudes y procesan todas las solicitudes entrantes. El número de **MaxPoolThreads** no incluye los subprocesos que usa la puerta de enlace de escritorio remoto. El valor predeterminado es 4.

**Optimizaciones de llamadas a procedimientos remotos para puerta de enlace de escritorio remoto**

Los parámetros siguientes pueden ayudar a ajustar las llamadas a procedimiento remoto (RPC) recibidas por Conexión a Escritorio remoto y equipos de puerta de enlace de escritorio remoto. Cambiar las ventanas ayuda a limitar la cantidad de datos que fluyen a través de cada conexión y puede mejorar el rendimiento de los escenarios de RPC a través de HTTP V2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    El valor predeterminado es 64 KB. Este valor especifica la ventana que el servidor utiliza para los datos que se reciben del proxy RPC. El valor mínimo se establece en 8 KB y el valor máximo se establece en 1 GB. Si un valor no está presente, se utiliza el valor predeterminado. Cuando se realizan cambios en este valor, se debe reiniciar IIS para que el cambio surta efecto.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    El valor predeterminado es 64 KB. Este valor especifica la ventana que el cliente usa para los datos que se reciben del proxy RPC. El valor mínimo es de 8 KB y el valor máximo es 1 GB. Si un valor no está presente, se utiliza el valor predeterminado.

## <a name="monitoring-and-data-collection"></a>Supervisión y recopilación de datos

La siguiente lista de contadores de rendimiento se considera un conjunto básico de contadores al supervisar el uso de recursos en la puerta de enlace de escritorio remoto:

-   \\Puerta de enlace de Terminal Services\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP por servidor\\\*

-   \\Servicio Web\\\*

-   \\W3SVC \_ w3wp\\\*

-   \\IPv6\\\*

-   \\Memoria\\\*

-   \\Interfaz de red ( \* )\\\*

-   \\Proceso ( \* )\\\*

-   \\Información del procesador ( \* )\\\*

-   \\Sincronización ( \* )\\\*

-   \\Integrado\\\*

-   \\TCPv4\\\*

Los siguientes contadores de rendimiento solo son aplicables para el transporte RPC heredado:

-   \\RPC de RPC/proxy HTTP \\ \*

-   \\Proxy RPC/HTTP por RPC de servidor \\ \*

-   \\RPC del servicio Web \\ \*

-   \\W3SVC \_ \\ \* RPC w3wp

> [!NOTE]
> Si es aplicable, agregue \\ los \\ \* objetos IPv6 y \\ TCPv6 \\ \* . ReplaceThisText

