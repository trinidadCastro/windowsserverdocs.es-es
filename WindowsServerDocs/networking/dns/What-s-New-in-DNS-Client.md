---
title: Novedades en el cliente DNS en Windows Server
description: En este tema proporciona una descripción general de las nuevas características en el cliente DNS en Windows Server y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novedades en el cliente DNS en Windows Server 2016

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema describe la funcionalidad del cliente de sistema de nombres de dominio (DNS) que es nuevo o cambiado en Windows 10 y Windows Server 2016 y versiones posteriores de estos sistemas operativos.
  
## <a name="updates-to-dns-client"></a>Actualizaciones de cliente DNS

**Enlace de servicio del cliente DNS**: en Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red. Para equipos con varios adaptadores, se ha optimizado la resolución de DNS de las siguientes maneras:  
  
-   Cuando se usa un servidor DNS que está configurado en una interfaz específica para resolver una consulta DNS, el servicio cliente DNS enlazará con esta interfaz antes de enviar la consulta DNS.  
  
    Al enlazar a una interfaz específica, el puede del cliente DNS claramente la interfaz donde se produce la resolución de nombres, para especificar habilitar aplicaciones para optimizar comunicaciones con el cliente DNS a través de esta interfaz de red.  
  
-   Si el servidor DNS que se usa designado por una configuración de directiva de grupo de la tabla de directivas de resolución de nombres (NRPT), el servicio cliente DNS no enlazar a una interfaz específica.  
  
> [!NOTE]  
> Cambios en el servicio cliente DNS en Windows 10 también están presentes en equipos que ejecutan Windows Server 2016 y versiones posteriores.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Novedades en el servidor DNS en Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

