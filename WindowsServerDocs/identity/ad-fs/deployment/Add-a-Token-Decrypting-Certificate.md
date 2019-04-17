---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Agregar un certificado de descifrado de Token
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-decrypting-certificate"></a>Agregar un certificado de descifrado de Token

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servidores de federación utilizan un certificado de descifrado de token\ cuando un usuario de confianza servidor de federación de terceros debe descifrar tokens emitidos con un certificado anterior después de establece un nuevo certificado como el certificado de descifrado principal. Active \(AD FS\) servicios de federación de directorio usa el certificado de capa de Sockets seguros \(SSL\) para Internet Information Services \(IIS\) como el certificado de descifrado de forma predeterminada.  
  
> [!CAUTION]  
> Certificados que se usa para descifrar token\ son esenciales para la estabilidad del servicio de federación. Porque el servicio puede entorpecer la pérdida o imprevisto eliminación de los certificados configurado para ello, deben copias de seguridad de los certificados configurados para este propósito.  
  
Puedes usar el siguiente procedimiento para agregar el certificado de descifrado de token\ a la administración de AD FS snap\ en desde un archivo que se haya exportado.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para agregar un certificado de descifrado de token\  
  
1.  En la **inicio**, escriba**AD FS administración**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, double\ clic **servicio**y, a continuación, haz clic en **certificados**.  
  
3.  En la **acciones** panel, haz clic en el **descifrar Token\ agregar certificado** vínculo.  
  
4.  En la **buscar el archivo de certificado** diálogo cuadro, vaya al archivo de certificado que quieras agregar, selecciona el archivo de certificado y, a continuación, haz clic en **abrir**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

