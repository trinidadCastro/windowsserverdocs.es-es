---
title: PASO 3 configurar DC1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338e214ac10796d3f9864aef74190f2d27f173b7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281356"
---
# <a name="step-3-configure-dc1"></a>PASO 3 configurar DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

DC1 actúa como un controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com. Configure DC1 como sigue:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Compruebe el que Usuario1 tiene un nombre de entidad de seguridad de usuario definido en DC1  
  
1.  En DC1, abra Administrador del servidor y haga clic en **AD DS** en el panel izquierdo. Haga clic en **DC1** y seleccione **equipos y usuarios de Active Directory**. En el panel izquierdo, expanda **corp.contoso.com\Users**y haga doble clic en User1.  
  
2.  En el **cuenta** ficha, compruebe que **nombre de inicio de sesión de usuario** debe establecerse como User1. Si no, a continuación, escriba **User1** en el **nombre de inicio de sesión de usuario** campo.  
  
3.  Haga clic en **Aceptar**. Cierre la consola de **Usuarios y equipos de Active Directory**.  
  


