---
description: 'Más información acerca de: lista de comprobación: implementar un diseño de SSO Web'
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Lista de comprobación: implementar un diseño de SSO Web'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: de48195031fd1d413a2751c554fd1020ff59093b
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814994"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: Implementar un diseño de SSO web

Esta lista de comprobación primaria incluye \- vínculos de referencia cruzada a conceptos importantes sobre \- el \- diseño de SSO Web de inicio de sesión único \( \) para servicios de Federación de Active Directory (AD FS) \( AD FS \) . También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.

> [!NOTE]
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.

![Icono de la lista de comprobación de implementación de un diseño de SSO Web. ](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**Lista de comprobación: implementar un diseño de SSO Web**

|Tarea|Referencia|
|--------|-------------|
|Revise los conceptos importantes sobre el diseño de SSO Web y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización. **Nota:**|![Icono para el vínculo identificar los objetivos de implementación de AD FS que puede usar en referencia para implementar un diseño de SSO Web. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Diseño de SSO Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))<p>![Icono del vínculo de diseño SSO Web que puede usar en referencia para implementar un diseño de SSO Web. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Identificación de los objetivos de implementación de AD FS](../design/identifying-your-ad-fs-deployment-goals.md)|
|Revise el hardware, el software, el certificado, el sistema de nombres \( de dominio DNS \) , el almacén de atributos y los requisitos del cliente para implementar AD FS en su organización.|![Icono del Apéndice A: revisar AD FS vínculo de requisitos que puede usar en referencia a la implementación de un diseño de SSO Web. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisión de los requisitos de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|Según el plan de diseño, instale uno o varios servidores de Federación en la red corporativa o en la red perimetral. **Nota:** El diseño de SSO Web solo requiere un servidor de Federación para funcionar correctamente. Un único servidor de Federación actúa tanto en el rol de proveedor de notificaciones como en el rol de usuario de confianza.|![Icono de la lista de comprobación: configuración de un vínculo de servidor de Federación que puede usar en referencia para implementar un diseño de SSO Web. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)|
|\(Opcional \) determine si su organización necesita o no un servidor proxy de Federación en la red perimetral.|![Icono de la lista de comprobación: configuración de un vínculo de servidor proxy de Federación que puede usar en referencia para implementar un diseño de SSO Web. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|Según tu diseño de SSO web y cómo pretendas usarlo, agrega el almacén de atributos, las relaciones de confianza para usuario autenticado, las notificaciones y las reglas de notificaciones correspondientes al servicio de federación.|![Icono de la lista de comprobación: configurar el vínculo de la organización del asociado de cuenta puede usar en referencia para implementar un diseño de SSO Web. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)|
|Si es administrador en la organización del asociado de recurso, las notificaciones \- habilitan la aplicación del explorador Web, la aplicación de servicio web o &reg; la aplicación de Microsoft Office SharePoint &reg; Server mediante WIF y el SDK de WIF. **Nota:**|![Icono del vínculo de Windows Identity Foundation que puede usar en referencia para implementar un diseño de SSO Web. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![SDK de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO Web|
