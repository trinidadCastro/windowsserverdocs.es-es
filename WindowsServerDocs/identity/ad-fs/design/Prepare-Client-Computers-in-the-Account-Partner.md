---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar los equipos cliente en el asociado de cuenta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e3a746ec003cf312ffe0b9804f84a55c98aa8089
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190992"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar los equipos cliente en el asociado de cuenta

La manera más fácil para que un administrador en una cuenta de organización preparar los equipos cliente para tener acceso a los servicios de federación de Active Directory asociado \(AD FS\) aplicaciones federadas es usar la directiva de grupo. La directiva de grupo proporciona una manera cómoda para insertar certificados y valores de configuración específicos que son necesarios para la federación a todos los equipos cliente que se usarán para el acceso a las aplicaciones federadas.  
  
Para que los equipos cliente puedan acceder sin problemas las aplicaciones federadas sin solicitudes de certificado o preguntas relacionados con el sitio de confianza, se recomienda preparar cada equipo cliente antes de implementar AD FS ampliamente en su organización. Considere usar la directiva de grupo para realizar automáticamente las siguientes acciones:  
  
-   Configurar Internet Explorer en cada equipo cliente debe confiar en el servidor de federación de cuenta.  
  
    Para obtener más información, consulte [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instalar el servidor de federación de cuenta correspondiente, el servidor de federación de recursos y el servidor Web Secure Sockets Layer \(SSL\) certificados \(o equivalente en los certificados vinculados a una raíz de confianza\) en cada uno equipo cliente.  
  
    Para obtener más información, consulte [distribuir certificados a los equipos cliente mediante la directiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)