---
description: 'Más información acerca de: lista de comprobación: implementar un diseño de SSO Web'
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Lista de comprobación: implementar un diseño de SSO Web'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: eb8627434fb29b167896a4704617231adb4b1794
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050333"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: Implementar un diseño de SSO web

Esta lista de comprobación primaria incluye \- vínculos de referencia cruzada a conceptos importantes sobre \- el \- diseño de SSO Web de inicio de sesión único \( \) para servicios de Federación de Active Directory (AD FS) \( AD FS \) . También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.

> [!NOTE]
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.

![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación de SSO Web: implementar un diseño de SSO Web**

|Tarea|Referencia|
|--------|-------------|
|Revise los conceptos importantes sobre el diseño de SSO Web y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización. **Nota:**|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[diseño SSO Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11)) SSO Web<p>![SSO Web que](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica sus objetivos de implementación de AD FS](../design/identifying-your-ad-fs-deployment-goals.md)|
|Revise el hardware, el software, el certificado, el sistema de nombres \( de dominio DNS \) , el almacén de atributos y los requisitos del cliente para implementar AD FS en su organización.|![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisión de los requisitos de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|Según el plan de diseño, instale uno o varios servidores de Federación en la red corporativa o en la red perimetral. **Nota:** El diseño de SSO Web solo requiere un servidor de Federación para funcionar correctamente. Un único servidor de Federación actúa tanto en el rol de proveedor de notificaciones como en el rol de usuario de confianza.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO Web: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)|
|\(Opcional \) determine si su organización necesita o no un servidor proxy de Federación en la red perimetral.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO Web: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|Según tu diseño de SSO web y cómo pretendas usarlo, agrega el almacén de atributos, las relaciones de confianza para usuario autenticado, las notificaciones y las reglas de notificaciones correspondientes al servicio de federación.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO Web: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)|
|Si es administrador en la organización del asociado de recurso, las notificaciones \- habilitan la aplicación del explorador Web, la aplicación de servicio web o &reg; la aplicación de Microsoft Office SharePoint &reg; Server mediante WIF y el SDK de WIF. **Nota:**|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) de SSO Web<p>![SDK de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO Web|
