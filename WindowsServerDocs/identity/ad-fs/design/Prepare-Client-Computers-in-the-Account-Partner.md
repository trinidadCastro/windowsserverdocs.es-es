---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar los equipos cliente en la cuenta de Partner
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar los equipos cliente en la cuenta de Partner

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La forma más sencilla para que un administrador de una organización de partner de la cuenta para preparar los equipos cliente para obtener acceso a los servicios de federación de Active Directory \(AD FS\) federados aplicaciones es usar la directiva de grupo. Directiva de grupo ofrece una manera cómoda para insertar los certificados específicos y opciones de configuración necesarias para federación a todos los equipos cliente que se usará para acceder a aplicaciones federadas.  
  
Para que los equipos cliente puedan acceder fácilmente aplicaciones federadas sin peticiones de certificado o mensajes de relacionados con el sitio de confianza, recomendamos que primero preparar cada equipo cliente antes de implementar AD FS ampliamente en la organización. Considera la posibilidad de usar la directiva de grupo para automáticamente:  
  
-   Configura Internet Explorer en cada equipo cliente para confiar en el servidor de federación de cuenta.  
  
    Para obtener más información, consulta [configurar equipos cliente para confiar en el servidor de federación de cuenta](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instalar el servidor de federación de cuenta adecuado, servidor de federación de recursos y certificados de capa de Sockets seguros \(SSL\) del servidor Web \ (o equivalente certificados que se encadenan a una confianza root\) en cada equipo cliente.  
  
    Para obtener más información, consulta [distribuir certificados en los equipos cliente mediante la directiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
