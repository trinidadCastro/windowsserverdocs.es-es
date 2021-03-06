---
title: Configuración del orden de las interfaces de red
description: Obtenga información sobre cómo usar la métrica de interfaz para configurar el orden de las interfaces de red.
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: 9f75e68b04ba32421148f4f21e8a6ca5aed8eaad
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040385"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configuración del orden de las interfaces de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016 y Windows 10, puede usar la métrica de interfaz para configurar el orden de las interfaces de red.

Esto es diferente de las versiones anteriores de Windows y Windows Server, que permitía configurar el orden de enlace de los adaptadores de red mediante la interfaz de usuario o los comandos **INetCfgComponentBindings:: MoveBefore** y **INetCfgComponentBindings:: MoveAfter**. Estos dos métodos para el orden de las interfaces de red no están disponibles en Windows Server 2016 y Windows 10.

En su lugar, puede usar el nuevo método para establecer el orden enumerado de los adaptadores de red mediante la configuración de la métrica de la interfaz de cada adaptador. Puede configurar la métrica de la interfaz mediante el comando [set-NetIPInterface](/powershell/module/nettcpip/set-netipinterface) de Windows PowerShell.

Cuando se eligen las rutas del tráfico de red y se ha configurado el parámetro **InterfaceMetric** del comando **set-NetIPInterface** , la métrica global que se usa para determinar la preferencia de la interfaz es la suma de la métrica de la ruta y la métrica de la interfaz. Normalmente, la métrica de interfaz da preferencia a una interfaz determinada, como el uso de redes cableadas, si están disponibles tanto con cable como con redes inalámbricas.

En el siguiente ejemplo de comando de Windows PowerShell se muestra el uso de este parámetro.

```powershell
Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15
```

La métrica de la interfaz IPv4 o IPv6 determina el orden en que los adaptadores aparecen en una lista.  Para obtener más información, consulte [función GetAdaptersAddresses](/windows/win32/api/iphlpapi/nf-iphlpapi-getadaptersaddresses?f=255&MSPPError=-2147217396).

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).