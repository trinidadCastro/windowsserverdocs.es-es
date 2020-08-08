---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar los equipos cliente en el asociado de cuenta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b78c3d3b4c5562acfcc7b2657f6e49119fc491e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967602"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar los equipos cliente en el asociado de cuenta

La manera más sencilla de que un administrador de una organización del asociado de cuenta prepare a los equipos cliente para tener acceso a Servicios de federación de Active Directory (AD FS) \( AD FS \) aplicaciones federadas es usar Directiva de grupo. La directiva de grupo proporciona una manera cómoda para insertar certificados y valores de configuración específicos que son necesarios para la federación a todos los equipos cliente que se usarán para el acceso a las aplicaciones federadas.

Para que los equipos cliente puedan acceder sin problemas a las aplicaciones federadas sin solicitudes de certificado o mensajes de confianza relacionados con el sitio, se recomienda preparar primero cada equipo cliente antes de implementar AD FS en su organización. Considere usar la directiva de grupo para realizar automáticamente las siguientes acciones:

-   Configure Internet Explorer en cada equipo cliente para que confíe en el servidor de Federación de la cuenta.

    Para obtener más información, consulte [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).

-   Instale el servidor de Federación de cuenta adecuado, el servidor de Federación de recursos y el servidor Web Capa de sockets seguros \( \) certificados SSL \( o certificados equivalentes que se encadenan a una raíz \) de confianza en cada equipo cliente.

    Para obtener más información, consulte [distribuir certificados a equipos cliente mediante Directiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).


## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
