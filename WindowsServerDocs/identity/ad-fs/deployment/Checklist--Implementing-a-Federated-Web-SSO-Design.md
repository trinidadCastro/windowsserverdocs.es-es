---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Lista de comprobación: implementar un diseño de SSO Web federado'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 165471fd06031a68343a54d019357afee782d082
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359950"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de comprobación: implementación de un diseño de SSO web federado

Esta lista de comprobación primaria incluye vínculos de referencia de\-cruzados a conceptos importantes sobre el signo de\-único Web federado\-en \(diseño de\) de SSO para Servicios de federación de Active Directory (AD FS) \(AD FS\). También contiene vínculos a listas de comprobación subordinadas que te ayudarán a completar las tareas necesarias para implementar este diseño.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual o a una lista de comprobación subordinada, vuelve a este tema después de revisar el tema conceptual o de completar las tareas de la lista de comprobación subordinada para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación de SSO Web federado ![: implementar un diseño de SSO Web federado**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![SSO Web federado](media/icon_checkboxo.gif)|Revise los conceptos importantes sobre el diseño de SSO Web federado y determine qué objetivos de implementación de AD FS puede usar para personalizar este diseño para satisfacer las necesidades de su organización.|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[diseño de SSO Web federado](https://technet.microsoft.com/library/dd807050.aspx) de SSO Web federado<br /><br />![SSO Web federado que](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![SSO Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación de la implementación](https://technet.microsoft.com/library/dd807083.aspx)|  
|![SSO Web federado](media/icon_checkboxo.gif)|Revise el hardware, el software, el certificado, el sistema de nombres de dominio \(DNS\), el almacén de atributos y los requisitos del cliente para implementar AD FS en su organización.|![SSO Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisión de los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web federado](media/icon_checkboxo.gif)|Revise los conceptos importantes sobre las notificaciones, las reglas de notificaciones, los almacenes de atributos y la base de datos de configuración de AD FS antes de implementar AD FS en ambas organizaciones asociadas.|![los conceptos de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS clave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md) de SSO Web federado|  
|![SSO Web federado](media/icon_checkboxo.gif)|Según el plan de diseño, instale uno o varios servidores de Federación en cada organización asociada. **Nota:** Para el diseño de SSO Web federado, necesita al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso.|![lista de comprobación de SSO Web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web federado](media/icon_checkboxo.gif)|\(\) opcional determine si su organización necesita o no un servidor proxy de Federación. Si el plan de diseño llama a un proxy, puede instalar uno o varios servidores proxy de Federación en cada organización asociada.|![lista de comprobación de SSO Web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web federado](media/icon_checkboxo.gif)|Según su plan de diseño, comparte los certificados, configura los clientes y configura las relaciones de confianza en ambas organizaciones de asociado para que puedan comunicarse mediante una relación de confianza de federación.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO Web federado: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación de SSO Web federado: configuración de la organización del asociado de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![SSO Web federado](media/icon_checkboxo.gif)|Si es administrador en la organización del asociado de recurso, las notificaciones\-habilitar la aplicación del explorador Web, la aplicación de servicio web o la aplicación Microsoft® Office SharePoint® Server con WIF y el SDK de WIF.|![SSO Web federado de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para sso web ![federado|  
