---
title: Configurar el orden de las Interfaces de red
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurar el orden de las Interfaces de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En Windows Server 2016 y Windows 10, puedes usar la métrica de interfaz para configurar el orden de interfaces de red.

Esto es diferente en versiones anteriores de Windows y Windows Server, que permite configurar el orden de enlace de los adaptadores de red mediante el uso de la interfaz de usuario o los comandos **INetCfgComponentBindings::MoveBefore** y **INetCfgComponentBindings::MoveAfter**. Estos dos métodos para ordenar las interfaces de red no están disponibles en Windows Server 2016 y Windows 10.

En su lugar, puedes usar el nuevo método para establecer el orden enumerado de adaptadores de red mediante la configuración de la métrica de cada adaptador. La métrica se puede configurar mediante la [conjunto NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) comando de Windows PowerShell.

Cuando se eligen las rutas de tráfico de red y se configuró la **InterfaceMetric** parámetro de la **conjunto NetIPInterface** comando, la métrica de carácter general que se usa para determinar la preferencia de la interfaz es la suma de la métrica de ruta y la métrica de interfaz. Por lo general, esta métrica da preferencia a una interfaz en particular, como el uso por cable si ambos cableadas e inalámbricas están disponibles.

El siguiente ejemplo de comandos de Windows PowerShell muestra el uso de este parámetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

El orden en que los adaptadores aparecen en una lista viene determinado por la métrica IPv4 o IPv6.  Para obtener más información, consulta [GetAdaptersAddresses función](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obtener vínculos a todos los temas de esta guía, consulte [optimización del rendimiento de red subsistema](net-sub-performance-top.md).
