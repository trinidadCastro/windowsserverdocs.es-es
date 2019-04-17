---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Unir un equipo a un dominio
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>Unir un equipo a un dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para los servicios de federación de Active Directory \(AD FS\) a función, cada equipo que funciona como un servidor de federación debe estar unido a un dominio. Proxies de servidor de federación pueden estar unidos a un dominio, pero esto no es un requisito.  
  
No es necesario para unirte a un servidor Web a un dominio si claims\ aplicaciones solo hospeda el servidor Web.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para unir un equipo a un dominio  
  
1.  En la **inicio**, escriba**Panel de Control**, y, a continuación, presione ENTRAR.  
  
2.  Navegar a **sistema y seguridad**y, a continuación, haz clic en **sistema**.  
  
3.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**.  
  
4.  En la **nombre de equipo**, haga clic **cambio**.  
  
5.  En **miembro de**, haz clic en **dominio**, escribe el nombre del dominio al que unirte a este equipo y, a continuación, haz clic en **Aceptar**.  
  
6.  Haz clic en **Aceptar**y, a continuación, reinicia el equipo.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

