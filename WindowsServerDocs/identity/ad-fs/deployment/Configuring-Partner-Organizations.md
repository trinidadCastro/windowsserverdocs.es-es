---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuración de las organizaciones asociadas
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3d7389ce806a5e3aebf4fe166b10e5262df0be8a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192239"
---
# <a name="configuring-partner-organizations"></a>Configuración de las organizaciones asociadas

Para implementar una nueva organización de asociado en Active Directory Federation Services \(AD FS\), complete las tareas en alguna [lista de comprobación: Configuración de la organización del asociado de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) o [lista de comprobación: Configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md), según su diseño de AD FS.  
  
> [!NOTE]  
> Al usar cualquiera de estas listas de comprobación, recomendamos encarecidamente que lea primero las referencias al asociado de cuenta o instrucciones de planeamiento de asociado de recurso el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de continuar con la procedimientos para la configuración de la organización del asociado de nuevo. La lista de comprobación de esta forma le ayudará a proporcionar una mejor comprensión de la historia completa de diseño e implementación de AD FS para la organización de asociado de cuenta asociado o un recurso.  
  
## <a name="about-account-partner-organizations"></a>Acerca de las organizaciones de asociado de cuenta  
Un asociado de cuenta es la organización en la relación de confianza de federación que almacena físicamente las cuentas de usuario en un almacén de AD FS: admite el atributo. El asociado de cuenta es responsable de recopilar y autenticar las credenciales del usuario, generar notificaciones para que el usuario y empaquetar las notificaciones en tokens de seguridad. Estos tokens se pueden presentar en una confianza de federación para permitir el acceso Web\-basadas en recursos que se encuentran en la organización del asociado de recurso.  
  
En otras palabras, un asociado de cuenta representa la organización para cuyos usuarios a la cuenta\-servidor de federación de lado emite tokens de seguridad. El servidor de federación en la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad que usa el asociado de recurso en decisiones de autorización.  
  
Con respecto a los almacenes de atributos, el asociado de cuenta de AD FS es conceptualmente equivalente a un único bosque de Active Directory cuyas cuentas necesitan tener acceso a los recursos que están ubicados físicamente en otro bosque. Las cuentas en este bosque pueden acceder a recursos en el bosque de recursos solo cuando una confianza externa o no existe relación entre los dos bosques y los recursos a la que están intentando obtener acceso a los usuarios se han establecido con la autorización apropiada de confianza de bosque permisos.  
  
## <a name="about-resource-partner-organizations"></a>Acerca de las organizaciones asociadas  
El asociado de recurso es la organización en una implementación de AD FS donde se encuentran los servidores Web. El asociado de recurso confía el asociado de cuenta para autenticar a los usuarios. Por lo tanto, para tomar decisiones de autorización, el asociado de recurso utiliza las notificaciones empaquetadas en tokens de seguridad que proceden de los usuarios del asociado de cuenta.  
  
En otras palabras, un asociado de recurso representa la organización cuyos servidores Web están protegidos por el recurso\-servidor de federación de lado. El servidor de federación en el asociado de recurso usa los tokens de seguridad generados por el asociado de cuenta para tomar decisiones de autorización para los servidores Web del asociado de recurso.  
  
Para que funcione como un recurso de AD FS, los servidores Web de la organización del asociado de recurso deben tener Windows Identity Foundation \(WIF\) instalado o tiene los servicios de federación de Active Directory \(AD FS\) 1.x Notificaciones\-servicios de rol del agente Web compatible con instalados. Los servidores que funcionan como recurso de AD FS puede hospedar cualquier Web Web\-explorador\-Web o en\-servicio\-aplicaciones basadas en.  
