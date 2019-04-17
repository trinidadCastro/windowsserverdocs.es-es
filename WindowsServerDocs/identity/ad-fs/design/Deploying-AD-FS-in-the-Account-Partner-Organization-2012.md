---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: "Implementación de AD FS en la organización de Partner de cuenta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a668659f375f7fe96d676e7018e9e9315e35be5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implementación de AD FS en la organización de Partner de cuenta

>Se aplica a: Windows Server 2012

Un asociado de la cuenta en los servicios de federación de Active Directory \(AD FS\) representa la organización en la relación de confianza de federación que físicamente almacena las cuentas de usuario en un almacén de atributo admitido. Para obtener más información acerca de qué atributos son compatibles tiendas, consulta [el rol de atributo almacena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
El servidor de federación de la organización de partner de cuenta autentica a los usuarios locales y crea tokens de seguridad que se usan por el asociado de recurso de tomar decisiones de autorización. Usuarios de confianza, como sitios Web y servicios Web, a continuación, se pueden registrarse con el servidor de federación y consumir fácilmente emiten tokens de autenticación y control de acceso.  
  
En escenarios en que debe proporcionar a los usuarios con acceso a varios de los servicios o aplicaciones federadas: cuando cada aplicación o servicio está hospedado por una organización diferente, puedes configurar el servidor de federación de asociado de cuenta, lo que puede implementar varios usuarios de confianza.  
  
Para obtener más información acerca de cómo instalar y configurar una organización de partner de cuenta, consulta [lista de comprobación: configuración de la organización de Partner cuenta](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisa el rol de servidor de federación de asociado de cuenta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Revisa el rol del servidor Proxy Server federación del asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar los equipos cliente en la cuenta de Partner](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
