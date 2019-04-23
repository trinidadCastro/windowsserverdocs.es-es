---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscribir un certificado SSL de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825246"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscribir un certificado SSL de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Servicios de federación de Active Directory \(AD FS\) requiere un certificado de capa de sockets seguros \(SSL\) autenticación de servidor en cada servidor de federación en la granja de servidores de federación. Puede utilizarse el mismo certificado en cada servidor de federación en una granja de servidores. Debe tener disponibles tanto el certificado como su clave privada. Por ejemplo, si tiene el certificado y su clave privada en un archivo .pfx, puede importar el archivo directamente al Asistente para la configuración de los Servicios de federación de Active Directory. Este certificado SSL debe contener lo siguiente:  
  
1.  El nombre de sujeto y nombre alternativo del firmante deben contener el nombre de servicio de federación, por ejemplo, fs.contoso.com.  
  
2.  Nombre alternativo del sujeto debe contener el valor **enterpriseregistration** seguido por el nombre Principal de usuario \(UPN\) sufijo de su organización, por ejemplo,  **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Especifique el nombre alternativo del firmante si va a habilitar el servicio de registro de dispositivos \(DRS\) para la unión.  
  
> [!IMPORTANT]  
> Si su organización usa varios sufijos UPN, y planea habilitar al DRS, el certificado SSL debe contener un nombre alternativo de firmante para cada sufijo.  
  
## <a name="see-also"></a>Vea también
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  
