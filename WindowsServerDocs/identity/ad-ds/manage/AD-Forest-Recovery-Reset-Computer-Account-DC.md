---
description: 'Más información acerca de la recuperación del bosque de AD: restablecimiento de la cuenta de equipo en el controlador de dominio'
title: 'Recuperación de bosque de AD: restablecimiento de la cuenta de equipo en el controlador de dominio'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.openlocfilehash: 21d36989ff6174e5b59eed0b6b63e8aea0a9d921
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041573"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperación de bosque de AD: restablecimiento de la cuenta de equipo en el controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Utilice el siguiente procedimiento para restablecer la contraseña de la cuenta de equipo del controlador de dominio.

## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para restablecer la contraseña de la cuenta de equipo del controlador de dominio

1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

   ```
   netdom help resetpwd
   ```

2. Use la sintaxis que proporciona este comando para usar la herramienta de línea de comandos Netdom para restablecer la contraseña de la cuenta de equipo, por ejemplo:

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*
   ```

    Donde *nombre del controlador de dominio* es el controlador de dominio local que se va a recuperar.

   > [!NOTE]
   > Debe ejecutar este comando dos veces.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
