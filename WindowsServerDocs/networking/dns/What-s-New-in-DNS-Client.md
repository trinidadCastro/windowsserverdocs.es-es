---
title: Novedades en el cliente DNS en Windows Server
description: En este tema proporciona información general sobre las nuevas características de cliente DNS en Windows Server y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860566"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novedades del cliente DNS en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe la funcionalidad del cliente del sistema de nombres de dominio (DNS) que son nuevas o modificadas en Windows 10 y Windows Server 2016 y versiones posteriores de estos sistemas operativos.
  
## <a name="updates-to-dns-client"></a>Actualizaciones de cliente DNS

**Enlace de servicio cliente DNS**: En Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red. Para los equipos de hosts múltiples, la resolución de DNS está optimizada de las maneras siguientes:  
  
-   Cuando un servidor DNS que está configurado en una interfaz concreta se usa para resolver una consulta DNS, el servicio cliente DNS se enlazará a esta interfaz antes de enviar la consulta de DNS.  
  
    Mediante el enlace claramente a una interfaz específica, el cliente DNS puede especificar la interfaz donde se produce la resolución de nombres, habilitar aplicaciones para optimizar las comunicaciones con el cliente DNS a través de esta interfaz de red.  
  
-   Si el servidor DNS que se utiliza se designa mediante una configuración de directiva de grupo desde la tabla de directivas de resolución de nombres (NRPT), el servicio cliente DNS no se enlaza a una interfaz específica.  
  
> [!NOTE]  
> Los cambios realizados en el servicio cliente DNS en Windows 10 también están presentes en los equipos que ejecutan Windows Server 2016 y versiones posteriores.  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades en el servidor DNS en Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

