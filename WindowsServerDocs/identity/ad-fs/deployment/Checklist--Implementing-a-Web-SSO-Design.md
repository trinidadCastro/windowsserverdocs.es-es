---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Lista de comprobación: implementar un diseño de SSO Web'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 298233eb20bd54e3e07eb87e029211502142b215
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855218"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: implementación de un diseño de SSO web

Esta lista de comprobación primaria incluye vínculos de referencia de\-cruzados a conceptos importantes sobre el inicio de sesión de\-único Web\-en \(diseño de\) de SSO para Servicios de federación de Active Directory (AD FS) \(AD FS\). También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación de SSO web ![: implementar un diseño de SSO Web**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![SSO Web](media/icon_checkboxo.gif)|Revise los conceptos importantes sobre el diseño de SSO Web y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización. **Nota:**|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[diseño SSO Web](https://technet.microsoft.com/library/dd807033.aspx) SSO Web<p>![SSO Web que](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Revise el hardware, el software, el certificado, el sistema de nombres de dominio \(DNS\), el almacén de atributos y los requisitos del cliente para implementar AD FS en su organización.|![SSO Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisión de los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Según el plan de diseño, instale uno o varios servidores de Federación en la red corporativa o en la red perimetral. **Nota:** El diseño de SSO Web solo requiere un servidor de Federación para funcionar correctamente. Un único servidor de Federación actúa tanto en el rol de proveedor de notificaciones como en el rol de usuario de confianza.|Lista de comprobación de SSO Web ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web](media/icon_checkboxo.gif)|\(\) opcional determine si su organización necesita o no un servidor proxy de Federación en la red perimetral.|Lista de comprobación de SSO Web ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web](media/icon_checkboxo.gif)|Según tu diseño de SSO web y cómo pretendas usarlo, agrega el almacén de atributos, las relaciones de confianza para usuario autenticado, las notificaciones y las reglas de notificaciones correspondientes al servicio de federación.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO web ![: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![SSO Web](media/icon_checkboxo.gif)|Si es administrador en la organización del asociado de recurso, las notificaciones\-habilitar la aplicación del explorador Web, la aplicación de servicio web o la aplicación Microsoft&reg; Office SharePoint&reg; Server con WIF y el SDK de WIF. **Nota:**|![SSO Web de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para sso web ![| 
