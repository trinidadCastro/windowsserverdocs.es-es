---
description: 'Más información sobre: Planeación de la implementación de AD FS'
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planear la implementación de AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: efac16b60fa59fdbd30f1b9636629c97c2072677
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043244"
---
# <a name="planning-to-deploy-ad-fs"></a>Planear la implementación de AD FS


Después de recopilar información sobre su entorno y decidir sobre un Servicios de federación de Active Directory (AD FS) \( AD FS \) diseño siguiendo las instrucciones de la [Guía de diseño de AD FS en Windows Server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md), puede empezar a planear la implementación del diseño de AD FS de su organización. Con el diseño completado y la información de este tema, puede determinar qué tareas realizar para implementar AD FS en su organización.

## <a name="reviewing-your-ad-fs-design"></a>Revisar el diseño de AD FS
Si el equipo de diseño que construyó el diseño de AD FS original para su organización es diferente del equipo de implementación que implementará realmente la implementación, asegúrese de que el equipo de implementación revisa el diseño final con el equipo de diseño. Revisa los siguientes aspectos relativos al diseño:

-   La estrategia del equipo de diseño para determinar la mejor topología física para colocar los servidores de federación en la red corporativa o en la red perimetral. El equipo de implementación puede hacer referencia a la documentación sobre este tema revisando los temas siguientes en la guía de diseño de AD FS:

    -   [El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)

    -   [Planear la ubicación del servidor de federación](../design/planning-federation-server-placement.md)

    -   [Planear la ubicación del servidor proxy de federación](../design/planning-federation-server-proxy-placement.md)

    Es posible que el equipo de diseño pueda dejar la ubicación del servidor de federación y del servidor proxy de federación para el equipo de implementación. En ese caso, el equipo de implementación será responsable de documentar e implementar la topología física de los servidores.

-   Los motivos empresariales para designar a su organización como proveedor de notificaciones, usuario de confianza o ambos, dentro del ámbito del diseño de AD FS documentado. Asegúrese de que los miembros del equipo de implementación comprenden los motivos por los que se está implementando AD FS y qué otras empresas u organizaciones participan en la Asociación de Federación. Asegúrese de que los miembros del equipo de implementación comprenden también las restricciones que existen para las otras compañías u organizaciones \( hardware limitado, sin entorno de extranet, etc \) ., que pueden limitar el ámbito del diseño de alguna manera. Para obtener más información sobre las organizaciones asociadas, consulta [Planear la implementación](../design/planning-your-deployment.md).

Después de que los equipos de diseño y los equipos de implementación acuerden estos problemas, pueden continuar con la implementación del diseño de AD FS. Para obtener más información, consulta [Implementar el plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).
