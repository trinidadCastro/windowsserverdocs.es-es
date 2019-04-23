---
title: 'Recuperación de bosques de AD: restablecer la cuenta de equipo en el controlador de dominio'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852766"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperación de bosques de AD: restablecer la cuenta de equipo en el controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Use el procedimiento siguiente para restablecer la contraseña de cuenta de equipo del controlador de dominio. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para restablecer la contraseña de cuenta de equipo del controlador de dominio  

1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   ```
   netdom help resetpwd  
   ```
  
2. Use la sintaxis que proporciona este comando para que usar la herramienta de línea de comandos Netdom para restablecer la contraseña de la cuenta de equipo, por ejemplo:  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    Donde *el nombre del controlador de dominio* es el controlador de dominio local que se va a recuperar. 
  
   > [!NOTE]
   > Debe ejecutar este comando dos veces.
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
