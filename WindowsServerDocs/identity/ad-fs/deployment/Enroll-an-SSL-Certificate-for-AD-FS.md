---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscribir un certificado SSL de AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscribir un certificado SSL de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Active \(AD FS\) servicios de federación Directory requiere un certificado para la autenticación de servidor de capa de sockets seguros \(SSL\) en cada servidor de federación de la granja de servidores de federación. El mismo certificado puede usarse en cada servidor de federación en una granja de servidores. Debe tener el certificado y su clave privada disponible. Por ejemplo, si tienes el certificado y su clave privada en un archivo .pfx, puede importar el archivo directamente en el Asistente para la configuración de los servicios de federación de Active Directory. Este certificado SSL debe incluir lo siguiente:  
  
1.  El nombre de asunto y el nombre alternativo del firmante deben contener el nombre de servicio de federación como fs.contoso.com.  
  
2.  Nombre alternativo del firmante debe contener el valor **enterpriseregistration** seguido el sufijo \(UPN\) nombre Principal de usuario de la organización, por ejemplo, **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Especificar el nombre alternativo del firmante si vas a habilitar el servicio de registro del dispositivo \(DRS\) para unirse a la empresa.  
  
> [!IMPORTANT]  
> Si tu organización usa varios sufijos UPN y tienes previsto habilitar al DRS, el certificado SSL debe contener una entrada de nombre alternativas de asunto para cada sufijo.  
  
## <a name="see-also"></a>Consulta también
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

