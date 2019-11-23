---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscribir un certificado SSL de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: efa7c7aee848a5bbb68d3ce7140e135d37c2161d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408369"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscribir un certificado SSL de AD FS

Servicios de federación de Active Directory (AD FS) \(AD FS\) requiere un certificado para la capa de sockets seguros \(la autenticación de servidor SSL\) en cada servidor de Federación de la granja de servidores de Federación. Se puede usar el mismo certificado en cada servidor de Federación de una granja. Debe tener disponibles tanto el certificado como su clave privada. Por ejemplo, si tiene el certificado y su clave privada en un archivo .pfx, puede importar el archivo directamente al Asistente para la configuración de los Servicios de federación de Active Directory. Este certificado SSL debe contener lo siguiente:  
  
1.  El nombre del firmante y el nombre alternativo del firmante deben contener el nombre del servicio de Federación, como fs.contoso.com.  
  
2.  El nombre alternativo del sujeto debe contener el valor **enterpriseregistration** seguido del nombre principal de usuario \(UPN\) sufijo de la organización, por ejemplo, **enterpriseregistration.Corp.contoso.com**.  
  
    > [!WARNING]  
    > Especifique el nombre alternativo del firmante si tiene previsto habilitar el servicio de registro de dispositivos \(DRS\) para Workplace Join.  
  
> [!IMPORTANT]  
> Si su organización usa varios sufijos UPN y tiene previsto habilitar DRS, el certificado SSL debe contener una entrada de nombre alternativo del firmante para cada sufijo.  
  
## <a name="see-also"></a>Consulta también
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

