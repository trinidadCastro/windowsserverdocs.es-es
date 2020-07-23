---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuración de organizaciones asociadas
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 71454bdf93ad3f9b873e30e7134f359907c8f9d9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966007"
---
# <a name="configuring-partner-organizations"></a>Configuración de organizaciones asociadas

Para implementar una nueva organización asociada en Servicios de federación de Active Directory (AD FS) \( AD FS \) , complete las tareas de la [lista de comprobación: configuración de la organización del asociado de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) o [lista de comprobación: configuración de la organización del asociado de cuenta](Checklist--Configuring-the-Account-Partner-Organization.md), en función del diseño de la AD FS.  
  
> [!NOTE]  
> Al usar cualquiera de estas listas de comprobación, se recomienda encarecidamente que lea primero las referencias a la guía de planeamiento de asociados de cuenta o de recursos en la [Guía de diseño de AD FS en Windows Server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md) antes de continuar con los procedimientos para configurar la nueva organización asociada. La siguiente lista de comprobación le ayudará a comprender mejor el AD FS de diseño e implementación completos del asociado de cuenta o de la organización del asociado de recurso.  
  
## <a name="about-account-partner-organizations"></a>Acerca de las organizaciones de asociado de cuenta  
Un asociado de cuenta es la organización de la relación de confianza de Federación que almacena físicamente las cuentas de usuario en un almacén de atributos compatible con AD FS. El asociado de cuenta es responsable de recopilar y autenticar las credenciales de un usuario, generar notificaciones para dicho usuario y empaquetar las notificaciones en tokens de seguridad. Estos tokens se pueden presentar a continuación a través de una confianza de Federación para permitir el acceso a \- los recursos basados en Web que se encuentran en la organización del asociado de recurso.  
  
En otras palabras, un asociado de cuenta representa la organización para cuyos usuarios el servidor de Federación de la cuenta \- emite tokens de seguridad. El servidor de Federación de la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad que el asociado de recurso utiliza para tomar decisiones de autorización.  
  
En lo que respecta a los almacenes de atributos, el asociado de cuenta en AD FS es conceptualmente equivalente a un único bosque de Active Directory cuyas cuentas necesitan tener acceso a los recursos ubicados físicamente en otro bosque. Las cuentas de este bosque solo pueden tener acceso a los recursos del bosque de recursos cuando existe una relación de confianza externa o de confianza de bosque entre los dos bosques y los recursos a los que los usuarios intentan obtener acceso se han establecido con los permisos de autorización adecuados.  
  
## <a name="about-resource-partner-organizations"></a>Acerca de las organizaciones de asociado de recurso  
El asociado de recurso es la organización en una implementación de AD FS donde se encuentran los servidores Web. El asociado de recurso confía en el asociado de cuenta para autenticar a los usuarios. Por lo tanto, para tomar decisiones de autorización, el asociado de recurso utiliza las notificaciones que se empaquetan en los tokens de seguridad que proceden de los usuarios del asociado de cuenta.  
  
En otras palabras, un asociado de recurso representa la organización cuyos servidores web están protegidos por el \- servidor de Federación del lado del recurso. El servidor de Federación del asociado de recurso utiliza los tokens de seguridad generados por el asociado de cuenta para tomar decisiones de autorización para los servidores Web en el asociado de recurso.  
  
Para que funcione como un recurso de AD FS, los servidores Web de la organización del asociado de recurso deben tener instalado Windows Identity Foundation \( WIF \) o tener \( instalados los servicios de rol de \) \- agente Web compatibles con notificaciones de servicios de Federación de Active Directory (AD FS) AD FS 1. x. Los servidores web que funcionan como un recurso de AD FS pueden hospedar aplicaciones basadas en el \- explorador Web \- o \- en un servicio Web \- .  
