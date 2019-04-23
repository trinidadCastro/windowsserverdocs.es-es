---
title: Recuperación de bosques de AD - copia de seguridad completa del servidor
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865566"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Restablecer una contraseña de confianza en un lado de la relación de confianza  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Si la recuperación de bosque está relacionado con una infracción de seguridad, use el procedimiento siguiente para restablecer una contraseña de confianza en un lado de la relación de confianza. Esto incluye relaciones de confianza implícitas entre secundarios y dominios primarios, así como relaciones de confianza explícitas entre este dominio (el dominio que confía) y otro dominio (el dominio de confianza). 
  
 Restablecer la contraseña en el que confía lado dominio de la confianza, también conocida como la confianza entrante (el lado que pertenece este dominio). A continuación, utilice la misma contraseña en el lado del dominio de confianza de la confianza, también conocida como la confianza de salida. Restablecer la contraseña de la confianza de salida cuando se restaura el primer controlador de dominio en cada uno de los demás dominios (de confianza). 
  
 Restablecer la contraseña de confianza, se garantiza que el controlador de dominio no se replica con controladores de dominio potencialmente erróneos fuera de su dominio. Al establecer la misma contraseña de confianza mientras restauraba el primer controlador de dominio en cada uno de los dominios, se asegura de que este controlador de dominio se replica con cada uno de los controladores de dominio recuperadas. Controladores de dominio posteriores en el dominio que se recuperan mediante la instalación de AD DS que replicarán automáticamente estas contraseñas nuevas durante el proceso de instalación. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Para restablecer una contraseña de confianza en un lado de la relación de confianza  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   ```  
   netdom experthelp trust  
   ```  
  
2. Use la sintaxis que proporciona este comando para que usar la herramienta NetDom para restablecer la contraseña de confianza.
   Por ejemplo, si hay dos dominios del bosque, primarios y secundarios y se ejecuta este comando en el controlador de dominio restaurado en el dominio primario, use la sintaxis del comando siguiente:  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Al ejecutar este comando en el dominio secundario, use la sintaxis del comando siguiente:  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT** debe ser el mismo valor en ambos lados de la relación de confianza. Ejecute este comando sólo una vez (a diferencia de la **netdom resetpwd** comando) porque se restablece automáticamente la contraseña dos veces. 
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
