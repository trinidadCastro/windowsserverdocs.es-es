---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Unir un equipo a un dominio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9f6d657397cb07d081a229135e3e6c97c7191164
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408346"
---
# <a name="join-a-computer-to-a-domain"></a>Unir un equipo a un dominio

Por Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 para que funcione, cada equipo que funcione como servidor de Federación debe estar unido a un dominio. los servidores proxy de Federación pueden estar Unidos a un dominio, pero esto no es un requisito.  
  
No es necesario unir un servidor Web a un dominio si el servidor Web hospeda únicamente aplicaciones de notificaciones @ no__t-0aware.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para unir un equipo a un dominio  
  
1.  En la pantalla **Inicio** , escriba **Panel de control**y, a continuación, presione Entrar.  
  
2.  Vaya a **sistema y seguridad**y, a continuación, haga clic en **sistema**.  
  
3.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**.  
  
4.  En la ficha **Nombre de equipo** , haga clic en **Cambiar**.  
  
5.  En **miembro de**, haga clic en **dominio**, escriba el nombre del dominio al que desea unir este equipo y, a continuación, haga clic en **Aceptar**.  
  
6.  Haga clic en **Aceptar** y, a continuación, reinicie el equipo.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

