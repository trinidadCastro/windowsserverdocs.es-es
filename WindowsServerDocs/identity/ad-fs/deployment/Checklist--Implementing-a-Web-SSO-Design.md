---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: "Lista de comprobación: implementar un diseño de inicio de sesión único Web"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de comprobación: Implementar un diseño de inicio de sesión único Web

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de comprobación principal incluye vínculos de cross\ referencia a conceptos importantes sobre el diseño de Single\ Sign\ en \(SSO\) para los servicios de federación de Active Directory \(AD FS\) Web. También contiene vínculos para subordinadas listas de comprobación que te ayudarán a realizar las tareas que son necesarios para implementar este diseño.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia para ir a un tema conceptual o a una lista de comprobación subordinada, volver a este tema después de revisar el tema conceptual o completar las tareas en la lista de comprobación subordinada para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![sso Web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: implementar un diseño de inicio de sesión único Web**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![sso Web](media/icon_checkboxo.gif)|Revisa los conceptos importantes sobre el diseño Web SSO y determinar los objetivos de la implementación de AD FS puedes usar para personalizar este diseño para satisfacer las necesidades de la organización. **Nota:**|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Web SSO diseño](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificar los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![sso Web](media/icon_checkboxo.gif)|Revisa el hardware, software, certificado, sistema de nombres de dominio \(DNS\), tienda de atributo y requisitos del cliente para la implementación de AD FS de la organización.|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisar AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso Web](media/icon_checkboxo.gif)|Según tu plan de diseño, instalar a uno o varios servidores de federación de la red corporativa o en la red de perímetro. **Nota:** el diseño de SSO Web requiere solamente un servidor de federación para funcionar correctamente. Un servidor de federación único actúa en el rol de proveedor de notificaciones y el rol de terceros de confianza.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso Web](media/icon_checkboxo.gif)|\(Optional\) determinar si la organización necesita un proxy de federación de servidor en la red perimetral.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de un Proxy de servidor de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso Web](media/icon_checkboxo.gif)|Según tu plan de diseño SSO Web y cómo quieres usarla, agregar el almacén de atributo apropiado, confiar confianzas de terceros, reclamaciones y reglas para el servicio de federación de notificación.|![sso Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de la organización de Partner de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![sso Web](media/icon_checkboxo.gif)|Si eres un administrador en la organización de partner de recurso, claims\ habilitar la aplicación de explorador Web, aplicación de servicio Web o aplicación de Microsoft® Office SharePoint® Server con WIF y el SDK WIF. **Nota:**|![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identidad de Windows Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Foundation de identidad de Windows SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
