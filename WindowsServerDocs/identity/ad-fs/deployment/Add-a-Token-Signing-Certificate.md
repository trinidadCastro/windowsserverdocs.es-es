---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Agregar un certificado de firma de tokens
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9b737cf8c9efb89ef9b3befaa1875b273bfcadf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814938"
---
# <a name="add-a-token-signing-certificate"></a>Agregar un certificado de firma de tokens


Los servidores de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS\) requieren certificados de firma de\-de tokens para evitar que los atacantes modifiquen o falsifican los tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. Cada certificado de firma\-token contiene claves privadas criptográficas y claves públicas que se usan para firmar digitalmente \(por medio de la clave privada\) un token de seguridad. Más adelante, después de que un servidor de Federación asociado reciba estas claves, validará la autenticidad \(por medio de la clave pública\) del token de seguridad cifrado.  
  
> [!CAUTION]  
> Los certificados usados para la firma de\-de token son fundamentales para la estabilidad de la Servicio de federación. Dado que la pérdida o la eliminación no planeada de los certificados configurados con este fin pueden interrumpir el servicio, debe realizar una copia de seguridad de todos los certificados configurados con este fin.  
  
El token\-certificado de firma debe encadenarse a una raíz de confianza en el Servicio de federación. Puede usar el siguiente procedimiento para agregar el token\-certificado de firma al\-de administración de AD FS en desde un archivo que haya exportado.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Para agregar un token\-certificado de firma  
  
1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.  
  
2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  
  
3.  En el panel **acciones** , haga clic en el vínculo **Agregar token\-certificado de firma** .  
  
4.  En el cuadro de diálogo **Buscar archivo de certificado** , navegue hasta el archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

