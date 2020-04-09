---
title: Configuración del orden de las interfaces de red
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: 9e288908df5f5de70f1e369cff08821b8d178de7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862218"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configuración del orden de las interfaces de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016 y Windows 10, puede usar la métrica de interfaz para configurar el orden de las interfaces de red.

Esto es diferente de las versiones anteriores de Windows y Windows Server, que permitía configurar el orden de enlace de los adaptadores de red mediante la interfaz de usuario o los comandos **INetCfgComponentBindings:: MoveBefore** y **INetCfgComponentBindings:: MoveAfter**. Estos dos métodos para el orden de las interfaces de red no están disponibles en Windows Server 2016 y Windows 10.

En su lugar, puede usar el nuevo método para establecer el orden enumerado de los adaptadores de red mediante la configuración de la métrica de la interfaz de cada adaptador. Puede configurar la métrica de la interfaz mediante el comando [set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) de Windows PowerShell.

Cuando se eligen las rutas del tráfico de red y se ha configurado el parámetro **InterfaceMetric** del comando **set-NetIPInterface** , la métrica global que se usa para determinar la preferencia de la interfaz es la suma de la métrica de la ruta y la métrica de la interfaz. Normalmente, la métrica de interfaz da preferencia a una interfaz determinada, como el uso de redes cableadas, si están disponibles tanto con cable como con redes inalámbricas.

En el siguiente ejemplo de comando de Windows PowerShell se muestra el uso de este parámetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

La métrica de la interfaz IPv4 o IPv6 determina el orden en que los adaptadores aparecen en una lista.  Para obtener más información, consulte [función GetAdaptersAddresses](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).
