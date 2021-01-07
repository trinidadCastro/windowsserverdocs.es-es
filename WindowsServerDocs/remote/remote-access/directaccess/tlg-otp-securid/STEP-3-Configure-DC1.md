---
title: Paso 3 configurar DC1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 642449e9c0c6524aa053cfb73c57b69507e5f4eb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950461"
---
# <a name="step-3-configure-dc1"></a>Paso 3 configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com. Configure DC1 de la siguiente manera:

## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Comprobar que user1 tiene un nombre principal de usuario definido en DC1

1.  En DC1, abra Administrador del servidor y haga clic en **AD DS** en el panel izquierdo. Haga clic con el botón secundario en **DC1** y seleccione **Active Directory usuarios y equipos**. En el panel izquierdo, expanda **Corp. contoso. com\Users** y haga doble clic en user1.

2.  En la pestaña **cuenta** , compruebe que **nombre de inicio de sesión de usuario** está establecido en user1. Si no es así, escriba **user1** en el campo **nombre de inicio de sesión de usuario** .

3.  Haga clic en **Aceptar**. Cierre la consola de **Usuarios y equipos de Active Directory**.



