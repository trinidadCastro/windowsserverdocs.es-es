---
title: 'Recuperación de bosque de AD: copia de seguridad de un servidor completo'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: e9222685e8f6369e560a841990bc13ab8b0e4d37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390258"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Restablecer una contraseña de confianza en un lado de la confianza  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Si la recuperación del bosque está relacionada con una infracción de seguridad, utilice el siguiente procedimiento para restablecer una contraseña de confianza en un lado de la confianza. Esto incluye las confianzas implícitas entre los dominios primarios y secundarios, así como las confianzas explícitas entre este dominio (el dominio que confía) y otro dominio (el dominio de confianza). 
  
 Restablezca la contraseña solo en el lado del dominio que confía de la confianza, también conocido como confianza entrante (el lado al que pertenece este dominio). A continuación, use la misma contraseña en el lado del dominio de confianza, que también se conoce como confianza saliente. Restablezca la contraseña de la confianza de salida cuando restaure el primer DC en cada uno de los otros dominios (de confianza). 
  
 El restablecimiento de la contraseña de confianza garantiza que el controlador de dominio no se replique con DC potencialmente incorrectos fuera de su dominio. Al establecer la misma contraseña de confianza al restaurar el primer DC en cada uno de los dominios, se asegura de que este controlador de dominio se replique con cada uno de los controladores de dominio recuperados. Los controladores de dominio posteriores del dominio que se recuperan mediante la instalación de AD DS replicarán automáticamente estas nuevas contraseñas durante el proceso de instalación. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Para restablecer una contraseña de confianza en un lado de la confianza  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   ```  
   netdom experthelp trust  
   ```  
  
2. Use la sintaxis que proporciona este comando para usar la herramienta NetDom para restablecer la contraseña de confianza.
   Por ejemplo, si hay dos dominios en el bosque (primario y secundario) y está ejecutando este comando en el controlador de dominio restaurado en el dominio primario, use la siguiente sintaxis de comando:  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Cuando ejecute este comando en el dominio secundario, use la siguiente sintaxis de comando:  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordt** debe ser el mismo valor en ambos lados de la confianza. Ejecute este comando solo una vez (a diferencia del comando **netdom resetpwd** ) porque restablece automáticamente la contraseña dos veces. 
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
