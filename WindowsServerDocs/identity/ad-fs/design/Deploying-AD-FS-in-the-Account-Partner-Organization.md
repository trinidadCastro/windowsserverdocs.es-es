---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Implementación de AD FS en la organización del asociado de cuenta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 63c080904482814f9f62451e8e7cfa4862d19927
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359255"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implementación de AD FS en la organización del asociado de cuenta

Un asociado de cuenta en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 representa la organización en la relación de confianza de Federación que almacena físicamente las cuentas de usuario en un almacén de atributos compatible. Para obtener más información sobre qué almacenes de atributos se admiten, vea [el rol de almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
El servidor de Federación de la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad usados por el asociado de recurso para tomar decisiones de autorización. Los usuarios de confianza como los sitios web y los servicios Web pueden registrarse fácilmente con el servidor de Federación y consumir tokens emitidos para la autenticación y el control de acceso.  
  
En escenarios en los que es necesario proporcionar a los usuarios acceso a varias aplicaciones o servicios federados, cuando cada aplicación o servicio está hospedado en una organización diferente, puede configurar el servidor de Federación del asociado de cuenta para que pueda implementar varios usuarios de confianza.  
  
Para obtener más información sobre cómo instalar y configurar una organización de asociado de cuenta, vea [Checklist: Configuración de la organización del asociado de cuenta @ no__t-0.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisar el rol del servidor de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Revisar el rol del servidor proxy de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar los equipos cliente en el asociado de cuenta](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
