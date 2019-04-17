---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: "Lista de comprobación: implementar un diseño SSO Web federado"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de comprobación: Implementar un diseño SSO Web federado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de comprobación principal incluye vínculos de cross\ referencia a conceptos importantes sobre el diseño federados Web Single\-Sign\-On \(SSO\) para \(AD FS\) los servicios de federación de Active Directory. También contiene vínculos para subordinadas listas de comprobación que te ayudarán a realizar las tareas que son necesarios para implementar este diseño.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia tiene un tema conceptual o una lista de comprobación subordinada, volver a este tema después de revisar el tema conceptual o completar las tareas en la lista de comprobación subordinada para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![federados sso web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: implementar un diseño de inicio de sesión único federados Web**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![sso web federado](media/icon_checkboxo.gif)|Revisa los conceptos importantes sobre el diseño SSO Web federado y determinar los objetivos de la implementación de AD FS puedes usar para personalizar este diseño para satisfacer las necesidades de la organización.|![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[federados Web SSO diseño](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificar los objetivos de implementación de AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de la implementación](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federado](media/icon_checkboxo.gif)|Revisa el hardware, software, certificado, sistema de nombres de dominio \(DNS\), tienda de atributo y requisitos del cliente para la implementación de AD FS de la organización.|![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Apéndice A: revisar AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federado](media/icon_checkboxo.gif)|Revisa los conceptos importantes acerca de las notificaciones, reclamar reglas, tiendas de atributo y la base de datos de configuración de AD FS antes de implementar AD FS en ambas organizaciones asociadas.|![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprensión clave AD FS conceptos](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Según tu plan de diseño, instale a uno o varios servidores de federación de cada organización del asociado. **Nota:** para el diseño SSO Web federado, necesitas servidor de federación de al menos una de la organización de partner de la cuenta y el servidor de federación de al menos una en la organización de partner de recurso.|![federados sso web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federado](media/icon_checkboxo.gif)|\(Optional\) determinar si la organización necesita un proxy de servidor de federación. Si se llama a tu plan de diseño de un servidor proxy, puedes instalar a uno o varios servidores proxy de servidor de federación de cada organización del asociado.|![federados sso web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de un Proxy de servidor de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Según tu plan de diseño, compartir certificados, configurar a los clientes y configurar las relaciones de confianza en ambas organizaciones asociadas para que puedan comunicarse a través de una confianza de federación.|![federados sso web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de la organización de Partner de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![federados sso web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de la organización de Partner de recursos](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso web federado](media/icon_checkboxo.gif)|Si eres un administrador en la organización de partner de recurso, claims\ habilitar la aplicación de explorador Web, aplicación de servicio Web o aplicación de Microsoft® Office SharePoint® Server con WIF y el SDK WIF.|![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identidad de Windows Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![federados sso web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Foundation de identidad de Windows SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
