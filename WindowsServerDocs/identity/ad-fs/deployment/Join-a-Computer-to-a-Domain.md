---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Unir un equipo a un dominio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 02df9659ee3a1121c0cee3f7c5fa21b91c36b87c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192053"
---
# <a name="join-a-computer-to-a-domain"></a>Unir un equipo a un dominio

Para los servicios de federación de Active Directory \(AD FS\) funcione, cada equipo que funciona como un servidor de federación debe estar unido a un dominio. servidores proxy de federación pueden unirse a un dominio, pero esto no es un requisito.  
  
No es necesario que unir un servidor Web a un dominio si el servidor Web aloja notificaciones\-sólo aplicaciones compatibles.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para unir un equipo a un dominio  
  
1.  En el **iniciar** , escriba **Panel de Control**, y, a continuación, presione ENTRAR.  
  
2.  Vaya a **sistema y seguridad**y, a continuación, haga clic en **sistema**.  
  
3.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**.  
  
4.  En la ficha **Nombre de equipo** , haga clic en **Cambiar**.  
  
5.  En **miembro de**, haga clic en **dominio**, escriba el nombre del dominio que desea que este equipo para unir y, a continuación, haga clic en **Aceptar**.  
  
6.  Haga clic en **Aceptar** y, a continuación, reinicie el equipo.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

