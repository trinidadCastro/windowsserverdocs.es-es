---
title: Paso 3 configurar DC1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 07167f2bc68ab8c465a96ce00552339d04dbb198
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856078"
---
# <a name="step-3-configure-dc1"></a>Paso 3 configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com. Configure DC1 de la siguiente manera:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Comprobar que user1 tiene un nombre principal de usuario definido en DC1  
  
1.  En DC1, abra Administrador del servidor y haga clic en **AD DS** en el panel izquierdo. Haga clic con el botón secundario en **DC1** y seleccione **Active Directory usuarios y equipos**. En el panel izquierdo, expanda **Corp. contoso. com\Users**y haga doble clic en user1.  
  
2.  En la pestaña **cuenta** , compruebe que **nombre de inicio de sesión de usuario** está establecido en user1. Si no es así, escriba **user1** en el campo **nombre de inicio de sesión de usuario** .  
  
3.  Haga clic en **Aceptar**. Cierre la consola de **Usuarios y equipos de Active Directory**.  
  


