---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Lista de comprobación: implementar un diseño de SSO Web'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408468"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: Implementar un diseño de SSO web

Esta lista de comprobación primaria incluye vínculos cruzados de @ no__t-0reference a conceptos importantes sobre el diseño web único @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 para Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-6. También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![Web SSO @ no__t-1Checklist: Implementar un diseño de SSO web**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![SSO Web](media/icon_checkboxo.gif)|Revise los conceptos importantes sobre el diseño de SSO Web y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización. **Nota:**|@no__t: diseño de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SSO Web](https://technet.microsoft.com/library/dd807033.aspx) de SSO de 0Web<br /><br />@no__t 0Web SSO que](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Revise el hardware, el software, el certificado, el sistema de nombres de dominio \(DNS @ no__t-1, el almacén de atributos y los requisitos de cliente para implementar AD FS en su organización.|![Web SSO @ no__t-1Appendix A: Revisión de los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Según el plan de diseño, instale uno o varios servidores de Federación en la red corporativa o en la red perimetral. **Nota:** El diseño de SSO Web solo requiere un servidor de Federación para funcionar correctamente. Un único servidor de Federación actúa tanto en el rol de proveedor de notificaciones como en el rol de usuario de confianza.|![Web SSO @ no__t-1Checklist: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web](media/icon_checkboxo.gif)|\(Optional @ no__t-1 Determine si su organización necesita o no un servidor proxy de Federación en la red perimetral.|![Web SSO @ no__t-1Checklist: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web](media/icon_checkboxo.gif)|Según tu diseño de SSO web y cómo pretendas usarlo, agrega el almacén de atributos, las relaciones de confianza para usuario autenticado, las notificaciones y las reglas de notificaciones correspondientes al servicio de federación.|![Web SSO @ no__t-1Checklist: Configuración de la organización del asociado de cuenta @ no__t-0|  
|![SSO Web](media/icon_checkboxo.gif)|Si es administrador en la organización del asociado de recurso, Claims @ no__t-0enable la aplicación de explorador Web, la aplicación de servicio web o la aplicación de Microsoft® Office SharePoint® Server con WIF y el SDK de WIF. **Nota:**|@no__t 0Web SSO de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO @no__t 0Web| 
