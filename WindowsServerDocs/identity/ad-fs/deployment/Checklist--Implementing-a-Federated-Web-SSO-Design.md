---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Lista de comprobación: implementar un diseño de SSO Web federado'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f7a65687ac07c9f7d9a814b50c4090c70817b4f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192373"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de comprobación: Implementar un diseño de SSO web federado

Esta lista de comprobación primaria incluye\-hacen referencia a vínculos a conceptos importantes sobre Federated Web único\-sesión\-en \(SSO\) diseño para los servicios de federación de Active Directory \(AD FS\). También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![sso web federado](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Implementar un diseño de SSO web federado**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![sso web federado](media/icon_checkboxo.gif)|Revisar conceptos importantes sobre el diseño SSO Web federado y determine qué objetivos de implementación de AD FS se puede usar para personalizar este diseño para satisfacer las necesidades de su organización.|![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificar los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planear la implementación](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federado](media/icon_checkboxo.gif)|Revise el hardware, software, certificados, sistema de nombres de dominio \(DNS\), almacén de atributos y los requisitos de cliente para la implementación de AD FS en su organización.|![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: Revisión de los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federado](media/icon_checkboxo.gif)|Revisar conceptos importantes sobre notificaciones, la base de datos de configuración de AD FS, los almacenes de atributos y reglas de notificación antes de implementar AD FS en ambas organizaciones asociadas.|![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Según el plan de diseño, instale a uno o varios servidores de federación en cada organización asociada. **Nota:** El diseño SSO Web federado, necesita al menos un servidor de federación de la organización del asociado de cuenta y al menos un servidor de federación de la organización del asociado de recurso.|![sso web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federado](media/icon_checkboxo.gif)|\(Opcional\) determinar si su organización necesita un servidor proxy de federación. Si el plan de diseño requiere un servidor proxy, puede instalar a uno o más proxies de servidor de federación en cada organización asociada.|![sso web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Según su plan de diseño, comparte los certificados, configura los clientes y configura las relaciones de confianza en ambas organizaciones de asociado para que puedan comunicarse mediante una relación de confianza de federación.|![sso web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: Configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![sso web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: Configuración de la organización del asociado de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Si es administrador en la organización del asociado de recurso, notificaciones\-habilitar su aplicación de explorador Web, aplicación de servicio Web o aplicación de Microsoft® Office SharePoint® Server usando WIF y el SDK de WIF.|![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)|  
