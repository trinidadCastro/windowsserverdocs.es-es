---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Revisar el rol del servidor proxy de federación en el asociado de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 377baa8f282f3886284a53b686944fe145b1b15e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190888"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Revisar el rol del servidor proxy de federación en el asociado de recurso

Un servidor proxy de federación de Active Directory Federation Services \(AD FS\) puede funcionar en uno o varios de los roles siguientes, dependiendo de cómo configure el servidor para satisfacer las necesidades de la organización del asociado de recurso:  
  
-   **Detección de asociados de cuenta**: Un equipo cliente de Internet tiene que identificar qué asociado de cuenta lo autenticará. El cliente busca el asociado de cuenta mediante el uso de un asociado de cuenta formulario Web de detección \(discoverclientrealm.aspx\), que está almacenado en el servidor proxy de federación del asociado de recurso. Si se configura más de un asociado de cuenta en el complemento Administración de AD FS\-en un descenso\-menú desplegable que aparece al cliente con todos los asociados de cuenta disponibles que son visibles para los equipos de cliente de Internet que el asociado de cuenta de acceso formulario Web de detección. Puedes cambiar cómo se muestra el formulario web de detección de asociados de cuenta a los equipos cliente personalizando el archivo discoverclientrealm.aspx.  
  
-   **Redireccionamiento del token de seguridad**: El servidor proxy de federación del asociado de cuenta envía los tokens de seguridad al asociado de recurso. El servidor proxy de federación de recursos acepta estos tokens y los pasa a la del servidor de federación del asociado de recurso. El servidor de federación de recursos, a continuación, emite un token de seguridad que está limitado por un servidor Web de recursos específico. El servidor proxy de federación de recursos, a continuación, redirige el token al cliente.  
  
En resumen, un servidor proxy de federación de recursos facilita el proceso de inicio de sesión federado redirigiendo los equipos cliente a un servidor de federación que puede autenticar a los clientes. Un servidor proxy de federación de recursos también actúa como un proxy para los tokens de seguridad de cliente en servidores de federación de recursos.  
  
> [!NOTE]  
> Cuando es necesario ayudar a reducir la cantidad de hardware y el número de certificados necesarios, el servidor proxy de federación puede encontrarse en el mismo equipo que el servidor Web.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

