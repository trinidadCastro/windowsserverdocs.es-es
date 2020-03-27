---
title: Novedades del cliente DNS en Windows Server
description: En este tema se proporciona información general sobre las nuevas características del cliente DNS en Windows Server y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a02aea149a658be2696195a2a17429b506dec1fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317981"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novedades del cliente DNS en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones de cliente del sistema de nombres de dominio (DNS) nuevas o modificadas en Windows 10 y Windows Server 2016 y versiones posteriores de estos sistemas operativos.
  
## <a name="updates-to-dns-client"></a>Actualizaciones del cliente DNS

**Enlace de servicio de cliente DNS**: en Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red. En el caso de los equipos de host múltiple, la resolución de DNS se optimiza de las siguientes maneras:  
  
-   Cuando se usa un servidor DNS que está configurado en una interfaz específica para resolver una consulta DNS, el servicio cliente DNS se enlazará a esta interfaz antes de enviar la consulta DNS.  
  
    Al enlazar a una interfaz específica, el cliente DNS puede especificar claramente la interfaz en la que se produce la resolución de nombres, lo que permite a las aplicaciones optimizar las comunicaciones con el cliente DNS a través de esta interfaz de red.  
  
-   Si el servidor DNS que se usa se designa con un valor de directiva de grupo de la tabla de directivas de resolución de nombres (NRPT), el servicio cliente DNS no se enlaza a una interfaz específica.  
  
> [!NOTE]  
> Los cambios en el servicio cliente DNS en Windows 10 también están presentes en los equipos que ejecutan Windows Server 2016 y versiones posteriores.  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades del servidor DNS en Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

