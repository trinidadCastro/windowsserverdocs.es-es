---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planeación para la implementación de AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831696"
---
# <a name="planning-to-deploy-ad-fs"></a>Planeación para la implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Después de recopilar información sobre el entorno y decidir un Active Directory Federation Services \(AD FS\) diseño siguiendo las instrucciones de la [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), puede empezar a planear la implementación del diseño de AD FS de su organización. Con el diseño completado y la información de este tema, puede determinar qué tareas debe realizar para implementar AD FS en su organización.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisar el diseño de AD FS  
Si el equipo de diseño que construye la instancia original de AD FS de diseño para su organización es diferente del equipo de implementación que realmente va a implementar la implementación, asegúrese de que el equipo de implementación revisa el diseño final con el equipo de diseño. Revisa los siguientes aspectos relativos al diseño:  
  
-   La estrategia del equipo de diseño para determinar la mejor topología física para colocar los servidores de federación en la red corporativa o en la red perimetral. El equipo de implementación puede hacer referencia a la documentación sobre este tema, revise los temas siguientes en la Guía de diseño de AD FS:  
  
    -   [El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Planear la ubicación del servidor de federación](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Planear la ubicación de Proxy de servidor de federación](https://technet.microsoft.com/library/dd807130.aspx)  
  
    Es posible que el equipo de diseño pueda dejar la ubicación del servidor de federación y del servidor proxy de federación para el equipo de implementación. En ese caso, el equipo de implementación será responsable de documentar e implementar la topología física de los servidores.  
  
-   Los motivos empresariales para designar a su organización como proveedor de notificaciones, usuario de confianza o ambos, dentro del ámbito del diseño de AD FS documentado. Asegúrese de que los miembros del grupo de implementación comprenden los motivos por qué se va a implementar AD FS y qué otras empresas u organizaciones están implicadas en la asociación de federación. Asegúrese de que los miembros del grupo de implementación comprenden también las restricciones que existen para las demás empresas u organizaciones \(limitadas de hardware, ningún entorno de extranet y así sucesivamente\) que puede limitar el ámbito del diseño de alguna manera. Para obtener más información sobre las organizaciones asociadas, consulta [Planear la implementación](https://technet.microsoft.com/library/dd807083.aspx).  
  
Después del diseño y los equipos y equipos de implementación acuerden estos aspectos, pueden continuar con la implementación del diseño de AD FS. Para obtener más información, consulta [Implementar el plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
