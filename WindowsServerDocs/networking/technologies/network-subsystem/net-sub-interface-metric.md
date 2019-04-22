---
title: Configuración del orden de las interfaces de red
description: En este tema forma parte de la Guía de ajuste de rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821776"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configuración del orden de las interfaces de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016 y Windows 10, puede usar la métrica de interfaz para configurar el orden de las interfaces de red.

Esto es diferente en versiones anteriores de Windows y Windows Server, lo que permite configurar el orden de enlace de adaptadores de red mediante la interfaz de usuario o los comandos **INetCfgComponentBindings::MoveBefore**y **INetCfgComponentBindings::MoveAfter**. Estos dos métodos para la ordenación de las interfaces de red no están disponibles en Windows Server 2016 y Windows 10.

En su lugar, puede usar el nuevo método para establecer el orden enumerado de adaptadores de red mediante la configuración de la métrica de interfaz de cada adaptador. Puede configurar la métrica de interfaz mediante el [Set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) comando de Windows PowerShell.

Cuando se eligen las rutas de tráfico de red y ha configurado el **InterfaceMetric** parámetro de la **Set-NetIPInterface** comando, la métrica general que se usa para determinar la interfaz preferencia es la suma de la métrica de ruta y la métrica de interfaz. Normalmente, la métrica de interfaz da preferencia a una interfaz concreta, como el uso de cableado si ambos cableadas e inalámbricas están disponibles.

El siguiente ejemplo de comando de Windows PowerShell muestra el uso de este parámetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

El orden en que los adaptadores aparecen en una lista viene determinada por la métrica de interfaz IPv4 o IPv6.  Para obtener más información, consulte [GetAdaptersAddresses función](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste de rendimiento del subsistema de red](net-sub-performance-top.md).
