---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Agregar un certificado de descifrado de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 388414fff97705901bf52ee844b90508d62f8c83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408452"
---
# <a name="add-a-token-decrypting-certificate"></a>Agregar un certificado de descifrado de tokens

Los servidores de Federación usan un certificado de descifrado de\-de token cuando un servidor de Federación de usuario de confianza debe descifrar los tokens que se emiten con un certificado anterior después de establecer un nuevo certificado como el certificado de descifrado principal. Servicios de federación de Active Directory (AD FS) \(AD FS\) usa el certificado capa de sockets seguros \(SSL\) para Internet Information Services \(IIS\) como el certificado de descifrado predeterminado.  
  
> [!CAUTION]  
> Los certificados usados para el descifrado de\-de token son fundamentales para la estabilidad de la Servicio de federación. Dado que la pérdida o la eliminación no planeada de los certificados configurados con este fin pueden interrumpir el servicio, debe realizar una copia de seguridad de todos los certificados configurados con este fin.  
  
Puede usar el siguiente procedimiento para agregar el token\-el descifrado del certificado al ajuste de administración de AD FS\-en desde un archivo que haya exportado.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para agregar un token\-descifrar el certificado  
  
1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.  
  
2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  
  
3.  En el panel **acciones** , haga clic en el vínculo **Agregar token\-descifrar certificado** .  
  
4.  En el cuadro de diálogo **Buscar archivo de certificado** , navegue hasta el archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

