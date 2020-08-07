---
title: Paso 3 configurar DC1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 254789f18ca1adeebed227081c8177dc3c706e3d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971692"
---
# <a name="step-3-configure-dc1"></a>Paso 3 configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com. Configure DC1 de la siguiente manera:

## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Comprobar que user1 tiene un nombre principal de usuario definido en DC1

1.  En DC1, abra Administrador del servidor y haga clic en **AD DS** en el panel izquierdo. Haga clic con el botón secundario en **DC1** y seleccione **Active Directory usuarios y equipos**. En el panel izquierdo, expanda **Corp. contoso. com\Users**y haga doble clic en user1.

2.  En la pestaña **cuenta** , compruebe que **nombre de inicio de sesión de usuario** está establecido en user1. Si no es así, escriba **user1** en el campo **nombre de inicio de sesión de usuario** .

3.  Haga clic en **Aceptar**. Cierre la consola de **Usuarios y equipos de Active Directory**.



