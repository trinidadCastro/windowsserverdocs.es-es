---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: "Planear la implementación de AD FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>Planear la implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Después de recopilar información acerca de su entorno y decidir en un diseño de los servicios de federación de Active Directory \(AD FS\) siguiendo las instrucciones en el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), puedes empezar a planear la implementación del diseño de AD FS de la organización. Con el diseño completado y la información de este tema, puedes determinar qué tareas para llevar a cabo para implementar AD FS de la organización.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisa el diseño de AD FS  
Si el equipo de diseño que se construye la versión original de AD FS de diseño para la organización es diferente del equipo de implementación que realmente realizar la implementación, asegúrate de que el equipo de implementación revisa el diseño final con el equipo de diseño. Revisa los siguientes puntos en relación con el diseño:  
  
-   Estrategia del equipo de diseño para determinar la mejor topología física para la colocación de los servidores de federación de su red corporativa o perímetro. El equipo de distribución puede consultar la documentación sobre este tema revisando los siguientes temas de la Guía de diseño de AD FS:  
  
    -   [La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Colocación del servidor de federación de planeación](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Colocación del servidor Proxy de federación de planeación](https://technet.microsoft.com/library/dd807130.aspx)  
  
    Es posible que el equipo de diseño puede dejar al asunto del servidor de federación o la ubicación del servidor proxy de federación para el equipo de distribución. El equipo de distribución, a continuación, es responsable de documentar e implementar la topología física de los servidores.  
  
-   Las razones empresariales para designación de la organización como un proveedor de notificaciones, usuario de confianza o ambos, el ámbito del diseño de AD FS documentado. Asegúrate de que los miembros del grupo de implementación comprenden los motivos por qué se está implementando AD FS y cuáles son otras empresas u organizaciones implicados en la asociación de federación. Asegúrese de que los miembros del grupo de implementación también comprenden las restricciones que existen para las otras empresas u organizaciones \ (limitadas de hardware, ningún entorno extranet y así forth\) que puede limitar el ámbito del diseño de alguna manera. Para obtener más información acerca de las organizaciones asociadas, consulta [planeamiento de la implementación](https://technet.microsoft.com/library/dd807083.aspx).  
  
Después del diseño y equipos de distribución y equipos acordar estos problemas, puede continuar con la implementación del diseño de AD FS. Para obtener más información, consulta [implementar su Plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
