---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Agregar un certificado de firma de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826156"
---
# <a name="add-a-token-signing-certificate"></a>Agregar un certificado de firma de tokens

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federación de Active Directory Federation Services \(AD FS\) requieren token\-certificados de firma para evitar que los atacantes modifiquen o falsifiquen los tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. Cada token\-certificado de firma contiene claves privadas cifradas y claves públicas que se utilizan para firmar digitalmente \(mediante la clave privada\) un token de seguridad. Más adelante, después de un servidor de federación asociado recibe estas claves, validar la autenticidad \(por medio de la clave pública\) del token de seguridad cifrada.  
  
> [!CAUTION]  
> Los certificados usados para el token\-firma son fundamentales para la estabilidad del servicio de federación. Porque la pérdida o una eliminación accidental de los certificados configurados con este fin podría interrumpir el servicio, debe copia de seguridad los certificados configurados con este fin.  
  
El token\-certificado de firma debe encadenarse a una raíz de confianza en el servicio de federación. Puede usar el procedimiento siguiente para agregar el token\-certificado de firma para el complemento de administración de AD FS\-en desde un archivo que ha exportado.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Para agregar un token\-certificado de firma  
  
1.  En el **iniciar** , escriba**administración de AD FS**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  
  
3.  En el **acciones** panel, haga clic en el **agregar Token\-certificado de firma** vínculo.  
  
4.  En el **Buscar archivo de certificado** diálogo cuadro, vaya al archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abierto**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Cómo configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificados para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

