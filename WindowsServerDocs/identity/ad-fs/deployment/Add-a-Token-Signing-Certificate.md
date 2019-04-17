---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Agregar un certificado de firma de tokens
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>Agregar un certificado de firma de tokens

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servidores de federación de los servicios de federación de Active Directory \(AD FS\) requieren certificados de firma de token\ para evitar que los atacantes modifiquen o contra la falsificación de tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. Cada certificado de firma de token\ contiene claves privadas cifradas y claves públicas que se usan para firmar digitalmente \ (mediante la key\ privada) un token de seguridad. Más adelante, después de estas claves se reciben un servidor de federación de partner, validar la autenticidad \ (mediante la key\ pública) del token de seguridad cifrado.  
  
> [!CAUTION]  
> Certificados que se usan para firmar token\ son esenciales para la estabilidad del servicio de federación. Porque el servicio puede entorpecer la pérdida o imprevisto eliminación de los certificados configurado para ello, deben copias de seguridad de los certificados configurados para este propósito.  
  
El certificado de firma token\ debe encadenar a raíz de confianza en el servicio de federación. Puedes usar el siguiente procedimiento para agregar el certificado de firma token\ a la administración de AD FS snap\ en desde un archivo que se haya exportado.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Para agregar un certificado de firma token\  
  
1.  En la **inicio**, escriba**AD FS administración**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, double\ clic **servicio**y, a continuación, haz clic en **certificados**.  
  
3.  En la **acciones** panel, haz clic en el **certificado de firma de agregar Token\** vínculo.  
  
4.  En la **buscar el archivo de certificado** diálogo cuadro, vaya al archivo de certificado que quieras agregar, selecciona el archivo de certificado y, a continuación, haz clic en **abrir**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

