---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Lista de comprobación: implementar un diseño SSO Web'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864046"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: Implementar un diseño de SSO web

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de comprobación primaria incluye\-hacen referencia a vínculos a conceptos importantes sobre el Web único\-sesión\-en \(SSO\) diseño para servicios de federación de Active Directory \(AD FS \). También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![sso Web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Implementar un diseño SSO Web**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![sso Web](media/icon_checkboxo.gif)|Revisar conceptos importantes sobre el diseño SSO Web y determine qué objetivos de implementación de AD FS se puede usar para personalizar este diseño para satisfacer las necesidades de su organización. **Nota:**|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[diseño de SSO Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificar los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![sso Web](media/icon_checkboxo.gif)|Revise el hardware, software, certificados, sistema de nombres de dominio \(DNS\), almacén de atributos y los requisitos de cliente para la implementación de AD FS en su organización.|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: Revisar los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso Web](media/icon_checkboxo.gif)|Según el plan de diseño, instale a uno o varios servidores de federación en la red corporativa o en la red perimetral. **Nota:** El diseño SSO Web requiere un único servidor de federación funcione correctamente. Un servidor de federación único actúa en el rol de proveedor de notificaciones y de confianza.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: Cómo configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso Web](media/icon_checkboxo.gif)|\(Opcional\) determinar si su organización necesita un servidor proxy de federación en la red perimetral.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: Configurar un servidor Proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso Web](media/icon_checkboxo.gif)|Según tu diseño de SSO web y cómo pretendas usarlo, agrega el almacén de atributos, las relaciones de confianza para usuario autenticado, las notificaciones y las reglas de notificaciones correspondientes al servicio de federación.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: Configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![sso Web](media/icon_checkboxo.gif)|Si es administrador en la organización del asociado de recurso, notificaciones\-habilitar su aplicación de explorador Web, aplicación de servicio Web o aplicación de Microsoft® Office SharePoint® Server usando WIF y el SDK de WIF. **Nota:**|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)| 
