---
title: "Recuperación de bosque de AD - restablecer la cuenta de equipo en el controlador de dominio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperación de bosque de AD - restablecer la cuenta de equipo en el controlador de dominio 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para restablecer la contraseña de cuenta de equipo del controlador de dominio.  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para restablecer la contraseña de cuenta de equipo del controlador de dominio  
  
1.  En un símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  Usa la sintaxis que este comando se proporciona para que usar la herramienta de línea de comandos de Netdom para restablecer la contraseña de cuenta de equipo, por ejemplo:  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     Donde *nombre del controlador de dominio* es el controlador de dominio local que se realiza la recuperación.  
  
    > [!NOTE]
    >  Debes ejecutar este comando dos veces.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
