---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Implementación de AD FS en la organización del asociado de cuenta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bf21860603b3055c2ef2c9e7b77bb106eb06e238
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191624"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implementación de AD FS en la organización del asociado de cuenta

Un asociado de cuenta en Active Directory Federation Services \(AD FS\) representa la organización en la relación de confianza de federación que almacena físicamente las cuentas de usuario en un almacén de atributos admitidos. Para obtener más información acerca de qué atributo se admiten los almacenes, vea [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
El servidor de federación en la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad que se usan por el asociado de recurso de tomar decisiones de autorización. Usuarios de confianza, como sitios Web y servicios Web pueden, a continuación, se registran en el servidor de federación con facilidad y consumen tokens emitidos para autenticación y control de acceso.  
  
En escenarios en los que es necesario proporcionar a los usuarios con acceso a varias aplicaciones federadas o servicios, cuando cada aplicación o servicio se hospeda por otra organización, puede configurar el servidor de federación del asociado de cuenta para que pueda implementar varios usuarios de confianza.  
  
Para obtener más información acerca de cómo instalar y configurar una organización del asociado de cuenta, consulte [lista de comprobación: Configuración de la organización del asociado de cuenta](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisar el rol del servidor de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Revisar el rol del servidor proxy de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar los equipos cliente en el asociado de cuenta](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)