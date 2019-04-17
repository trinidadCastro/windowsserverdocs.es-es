---
title: Solucionar problemas de su firewall de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas de su firewall de Windows Server Essentials
 
>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Si tienes problemas con el acceso remoto, ejecuta al Asistente de acceso de reparación en cualquier lugar.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para ejecutar al Asistente de acceso de reparación en cualquier lugar  
  
1.  Abra el panel.  
  
2.  Haz clic en **configuración**, haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, haz clic en **reparación**.  
  
3.  Sigue las instrucciones del Asistente de acceso de reparación en cualquier lugar.  
  
 Si estás usando una configuración de red avanzada o uso de un firewall no son de Microsoft, tienes que abrir puertos adicionales en el firewall. Los puertos en la siguiente tabla se registran con Internet asignados números entidad (IANA).  
  
|Número de puerto|Descripción|  
|-----------------|-----------------|  
|65500|Servicio web de certificados|  
|65510 y 65515|Sitio Web de implementación de equipo cliente|  
|65520|Servicio Web para los equipos cliente Mac|  
|65532|Marco de proveedor para las comunicaciones de bucle invertido de servidor|  
|6602|Marco de proveedor para la comunicación entre los equipos servidor y cliente|  
  
## <a name="see-also"></a>Consulta también  
  
-   [Usar el acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Compatibilidad con Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Compatibilidad con Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

