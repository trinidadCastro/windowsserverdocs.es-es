---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuración de organizaciones asociadas
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 575d7e3fc97496c3f7c147220fe342add66517c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408396"
---
# <a name="configuring-partner-organizations"></a>Configuración de organizaciones asociadas

Para implementar una nueva organización asociada en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1, complete las tareas en [Checklist: Configuración de la organización del asociado de recurso @ no__t-0 o [Checklist: Configuración de la organización del asociado de cuenta @ no__t-0, en función del diseño del AD FS.  
  
> [!NOTE]  
> Al usar cualquiera de estas listas de comprobación, se recomienda encarecidamente que lea primero las referencias a la guía de planeamiento de asociados de cuenta o de recursos en la [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de continuar con los procedimientos para configurar el nueva organización asociada. La siguiente lista de comprobación le ayudará a comprender mejor el AD FS de diseño e implementación completos del asociado de cuenta o de la organización del asociado de recurso.  
  
## <a name="about-account-partner-organizations"></a>Acerca de las organizaciones de asociado de cuenta  
Un asociado de cuenta es la organización de la relación de confianza de Federación que almacena físicamente las cuentas de usuario en un almacén de atributos compatible con AD FS. El asociado de cuenta es responsable de la recopilación y autenticación de las credenciales de un usuario, la creación de notificaciones para ese usuario y el empaquetado de las notificaciones en los tokens de seguridad. Después, estos tokens se pueden presentar a través de una confianza de Federación para permitir el acceso a los recursos Web @ no__t-0based que se encuentran en la organización del asociado de recurso.  
  
En otras palabras, un asociado de cuenta representa la organización para cuyos usuarios el servidor de Federación Account @ no__t-0side emite tokens de seguridad. El servidor de Federación de la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad que el asociado de recurso utiliza para tomar decisiones de autorización.  
  
En lo que respecta a los almacenes de atributos, el asociado de cuenta en AD FS es conceptualmente equivalente a un único bosque de Active Directory cuyas cuentas necesitan tener acceso a los recursos ubicados físicamente en otro bosque. Las cuentas de este bosque pueden acceder a los recursos del bosque de recursos solo cuando existe una relación de confianza externa o de confianza de bosque entre los dos bosques y los recursos a los que los usuarios intentan obtener acceso se han establecido con la autorización adecuada. los.  
  
## <a name="about-resource-partner-organizations"></a>Acerca de las organizaciones de asociado de recurso  
El asociado de recurso es la organización en una implementación de AD FS donde se encuentran los servidores Web. El asociado de recurso confía en el asociado de cuenta para autenticar a los usuarios. Por lo tanto, para tomar decisiones de autorización, el asociado de recurso utiliza las notificaciones que se empaquetan en los tokens de seguridad que proceden de los usuarios del asociado de cuenta.  
  
En otras palabras, un asociado de recurso representa la organización cuyos servidores web están protegidos por el servidor de Federación de Resource @ no__t-0side. El servidor de Federación del asociado de recurso utiliza los tokens de seguridad generados por el asociado de cuenta para tomar decisiones de autorización para los servidores Web en el asociado de recurso.  
  
Para que funcione como un recurso de AD FS, los servidores Web de la organización del asociado de recurso deben tener Windows Identity Foundation \(WIF @ no__t-1 instalado o tener el Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-3 1. x Claims @ no__t-4Aware Web Servicios de rol del agente instalado. Los servidores web que funcionan como un recurso de AD FS pueden hospedar las aplicaciones web @ no__t-0browser @ no__t-1based o Web @ no__t-2Service @ no__t-3based.  
