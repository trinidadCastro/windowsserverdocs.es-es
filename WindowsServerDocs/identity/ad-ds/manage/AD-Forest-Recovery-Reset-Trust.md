---
title: "Recuperación de bosque de AD - copia de seguridad completa del servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Restablecer una contraseña de confianza a un lado de la relación de confianza  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Si la recuperación de bosque está relacionado con una infracción de seguridad, usa el siguiente procedimiento para restablecer una contraseña de confianza a un lado de la relación de confianza. Esto incluye implícitas confianzas entre secundario y sus dominios primarios, así como explícitas confianzas entre este dominio (el dominio que confía) y otro dominio (el dominio de confianza).  
  
 Restablecer la contraseña en el que confía lado dominio de la confianza, también conocida como la confianza de entrada (el lado donde pertenece este dominio). A continuación, usa la misma contraseña en el lado del dominio de confianza de la confianza, también conocida como la confianza de salida. Restablecer la contraseña de la confianza de salida, cuando se restaura el primer controlador de dominio en cada uno de los otros dominios (de confianza).  
  
 Restablecer la contraseña de confianza, se garantiza que el controlador de dominio no se replica con controladores de dominio potencialmente malo fuera de su dominio. Al establecer la misma contraseña de confianza al restaurar el primer controlador de dominio en cada uno de los dominios, te asegurarás de que este controlador de dominio se replica con cada uno de los controladores de dominio recuperados. Controladores de dominio posteriores en el dominio que se recuperan mediante la instalación de AD DS automáticamente replicarán estas nuevas contraseñas durante el proceso de instalación.  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Para restablecer una contraseña de confianza a un lado de la relación de confianza  
  
1.  En un símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  Usa la sintaxis que este comando se proporciona para que usar la herramienta NetDom para restablecer la contraseña de confianza.  
  
     Por ejemplo, si hay dos dominios del bosque: principales y secundarias y se ejecuta este comando en el controlador de dominio restaurado en el dominio principal, usa la siguiente sintaxis:  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     Al ejecutar este comando en el dominio secundario, usa la siguiente sintaxis:  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT** debe ser el mismo valor en ambos lados de la relación de confianza. Ejecutar este comando solo una vez (a diferencia de la **netdom resetpwd** comando) porque se restablece automáticamente la contraseña dos veces.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
