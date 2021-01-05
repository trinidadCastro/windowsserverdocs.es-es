---
description: 'Más información acerca de: lista de comprobación: implementar un diseño de SSO Web federado'
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Lista de comprobación: implementar un diseño de SSO Web federado'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b6bec4d2923e69c80e32585a14d8781a6f705ae5
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814963"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de comprobación: implementación de un diseño de SSO web federado

Esta lista de comprobación primaria incluye \- vínculos de referencia cruzada a conceptos importantes sobre el \- diseño de inicio de sesión único Web federado \- \( \) para servicios de Federación de Active Directory (AD FS) \( AD FS \) . También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.

> [!NOTE]
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.

![Icono de la lista de comprobación de implementación de un diseño de SSO Web federado. ](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**Lista de comprobación: implementar un diseño de SSO Web federado**

|Tarea|Referencia|
|--------|-------------|
|Revise los conceptos importantes sobre el diseño de SSO Web federado y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización.|![Icono del vínculo de diseño de SSO Web federado que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Diseño de SSO Web federado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))<p>![Icono para el vínculo identificar los objetivos de implementación de AD FS que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Identificación de los objetivos de implementación de AD FS](../design/identifying-your-ad-fs-deployment-goals.md)<p>![Icono de planeación del vínculo de implementación que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Planeación de la implementación](../design/planning-your-deployment.md)|
|Revise el hardware, el software, el certificado, el sistema de nombres \( de dominio DNS \) , el almacén de atributos y los requisitos del cliente para implementar AD FS en su organización.|![Icono del Apéndice A: revisar AD FS vínculo de requisitos puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisión de los requisitos de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|Revise los conceptos importantes sobre las notificaciones, las reglas de notificaciones, los almacenes de atributos y la base de datos de configuración de AD FS antes de implementar AD FS en ambas organizaciones asociadas.|![Icono del vínculo Understanding Key AD FS Concepts, que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Descripción de los conceptos de AD FS clave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|
|Según el plan de diseño, instale uno o varios servidores de Federación en cada organización asociada. **Nota:** Para el diseño de SSO Web federado, necesita al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso.|![Icono de la lista de comprobación: configuración de un vínculo de servidor de Federación que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)|
|\(Opcional \) determine si su organización necesita o no un servidor proxy de Federación. Si el plan de diseño llama a un proxy, puede instalar uno o varios servidores proxy de Federación en cada organización asociada.|![Icono de la lista de comprobación: configuración de un vínculo de servidor proxy de Federación que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|Según su plan de diseño, comparte los certificados, configura los clientes y configura las relaciones de confianza en ambas organizaciones de asociado para que puedan comunicarse mediante una relación de confianza de federación.|![Icono de la lista de comprobación: configurar el vínculo de la organización del asociado de cuenta puede usar en referencia para implementar un diseño de SSO Web federado. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![Icono de la lista de comprobación: configurar el vínculo de la organización del asociado de recurso puede usar en referencia para implementar un diseño de SSO Web federado. ](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Lista de comprobación: configuración de la organización del asociado de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|
|Si es administrador en la organización del asociado de recurso, las notificaciones \- habilitan la aplicación del explorador Web, la aplicación de servicio web o &reg; la aplicación de Microsoft Office SharePoint &reg; Server mediante WIF y el SDK de WIF.|![Icono del vínculo de Windows Identity Foundation que puede usar en referencia para implementar un diseño de SSO Web federado. ](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO Web federado|
