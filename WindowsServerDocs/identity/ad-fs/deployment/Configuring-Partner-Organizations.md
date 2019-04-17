---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: "Configuración de las organizaciones asociadas"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>Configuración de las organizaciones asociadas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implementar una nueva organización de partner en los servicios de federación de Active Directory \(AD FS\), completar las tareas en cualquiera [lista de comprobación: configuración de la organización de Partner de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) o [lista de comprobación: configuración de la organización de Partner cuenta](Checklist--Configuring-the-Account-Partner-Organization.md), según el diseño de AD FS.  
  
> [!NOTE]  
> Al usar cualquiera de estas listas de comprobación, te recomendamos leer primero las referencias a partner de la cuenta o el partner de recurso planeación instrucciones en el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de seguir los procedimientos para configurar la organización de partner de nuevo. La lista de comprobación de esta forma te ayudará a comprender mejor la historia de diseño e implementación de AD FS completa para la organización de partner de partner o un recurso de cuenta.  
  
## <a name="about-account-partner-organizations"></a>Acerca de las organizaciones asociadas de cuenta  
Un asociado de cuenta es la organización en la relación de confianza de federación que almacena físicamente cuentas de usuario en un almacén de AD FS: admite el atributo. El asociado de cuenta es responsable de recopilar y autenticar las credenciales del usuario, creación de notificaciones para ese usuario y empaquetar las notificaciones en los tokens de seguridad. Estos tokens se pueden representar, a continuación, en una confianza de federación para habilitar el acceso a recursos Web\ que se encuentran en la organización de partner de recurso.  
  
En otras palabras, un asociado de cuenta representa la organización para cuyos usuarios el servidor de federación de lado account\ emite tokens de seguridad. El servidor de federación de la organización de partner de cuenta autentica a los usuarios locales y crea tokens de seguridad que usa el asociado de recurso de tomar decisiones de autorización.  
  
Con respecto a los almacenes de atributo, el asociado de la cuenta en AD FS es conceptualmente equivalente a un único bosque de Active Directory cuyas cuentas necesitan tener acceso a recursos que se encuentren físicamente de otro bosque. Cuentas en este bosque pueden acceder a recursos en el bosque de recursos solo cuando una confianza externa o un bosque de confianza existe relación entre los dos bosques y se han establecido los recursos a la que están intentando acceder a los usuarios con los permisos de la autorización adecuada.  
  
## <a name="about-resource-partner-organizations"></a>Acerca de las organizaciones asociadas de recursos  
El asociado de recurso es la organización en una implementación de AD FS donde se encuentran los servidores Web. El asociado de recurso confía en el partner de cuenta para autenticar a los usuarios. Por lo tanto, para tomar decisiones de autorización, el asociado de recurso utiliza las notificaciones que se empaquetan en tokens de seguridad que provienen de usuarios en el asociado de cuenta.  
  
En otras palabras, un partner de recurso representa la organización cuyos servidores Web están protegidos por el servidor de federación de resource\ lado. El servidor de federación en el partner de recursos usa los tokens de seguridad que se presentan con el asociado de cuenta para tomar decisiones de autorización para servidores Web en el partner de recursos.  
  
Para que funcione como un recurso de AD FS, servidores Web en la organización de partner de recursos debe tener Windows Identity Foundation \(WIF\) instalados o han instalado los servicios de rol de servicios de federación de Active Directory \(AD FS\) 1.x Claims\ cuenta agente de Web. Los servidores Web que funcionen como un recurso de AD FS pueden hospedar aplicaciones Web\ basados en browser\ o bien Web\ service\-basado.  
