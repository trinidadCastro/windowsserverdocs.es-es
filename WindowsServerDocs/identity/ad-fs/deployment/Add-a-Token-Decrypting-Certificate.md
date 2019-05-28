---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Agregar un certificado de descifrado de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cf89972120f3f0effa3eb1cf0fee6d29dbc8ed4e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192483"
---
# <a name="add-a-token-decrypting-certificate"></a>Agregar un certificado de descifrado de tokens

Los servidores de federación usan un token\-certificado de descifrado cuando un servidor de federación de terceros para usuario autenticado debe descifrar los tokens emitidos por un certificado antiguo después de establece un nuevo certificado como certificado de descifrado principal. Servicios de federación de Active Directory \(AD FS\) usa la capa de Sockets seguros \(SSL\) certificado para Internet Information Services \(IIS\) como el descifrado de forma predeterminada certificado.  
  
> [!CAUTION]  
> Los certificados usados para el token\-descifrar son fundamentales para la estabilidad del servicio de federación. Porque la pérdida o una eliminación accidental de los certificados configurados con este fin podría interrumpir el servicio, debe copia de seguridad los certificados configurados con este fin.  
  
Puede usar el procedimiento siguiente para agregar el token\-descifrar el certificado para el complemento de administración de AD FS\-en desde un archivo que ha exportado.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para agregar un token\-certificado de descifrado  
  
1.  En el **iniciar** , escriba**administración de AD FS**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  
  
3.  En el **acciones** panel, haga clic en el **agregar Token\-certificado de descifrado** vínculo.  
  
4.  En el **Buscar archivo de certificado** diálogo cuadro, vaya al archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abierto**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

