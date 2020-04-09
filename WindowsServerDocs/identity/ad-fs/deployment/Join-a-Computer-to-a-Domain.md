---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Unir un equipo a un dominio
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 174f585f3e156fc8e068b9300fc90a20a67869cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855358"
---
# <a name="join-a-computer-to-a-domain"></a>Unir un equipo a un dominio

Por Servicios de federación de Active Directory (AD FS) \(AD FS\) funcione, cada equipo que funcione como servidor de Federación debe estar unido a un dominio. los servidores proxy de Federación pueden estar Unidos a un dominio, pero esto no es un requisito.  
  
No es necesario unir un servidor Web a un dominio si el servidor Web hospeda notificaciones\-solo aplicaciones compatibles.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para unir un equipo a un dominio  
  
1.  En la pantalla **Inicio** , escriba **Panel de control**y, a continuación, presione Entrar.  
  
2.  Vaya a **sistema y seguridad**y, a continuación, haga clic en **sistema**.  
  
3.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**.  
  
4.  En la pestaña **Nombre de equipo**, haz clic en **Cambiar**.  
  
5.  En **miembro de**, haga clic en **dominio**, escriba el nombre del dominio al que desea unir este equipo y, a continuación, haga clic en **Aceptar**.  
  
6.  Haz clic en **Aceptar** y reinicia el equipo.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

