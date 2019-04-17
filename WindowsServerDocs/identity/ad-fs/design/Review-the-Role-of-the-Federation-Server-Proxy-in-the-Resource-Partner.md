---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: "Revisa el rol del servidor Proxy de servidor de federación en el Partner de recursos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Revisa el rol del servidor Proxy de servidor de federación en el Partner de recursos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un proxy de federación de servidor en servicios de federación de Active Directory \(AD FS\) puede funcionar en una o varias de las siguientes funciones, dependiendo de cómo configurar el servidor para satisfacer las necesidades de la organización de partner de recursos:  
  
-   **Detección de asociado de cuenta**: equipo del cliente Internet debe identificar qué cuenta de partner, autenticará de él. El cliente encuentra al asociado de cuenta mediante el uso de una cuenta partner detección Web formulario \(discoverclientrealm.aspx\), que se almacena en el servidor proxy de servidor de federación del asociado de recurso. Si hay más de un partner de cuenta se configura en la administración de AD FS snap\ en, que aparece un menú desplegable drop\ al cliente con todos los asociados de cuenta disponible que son visibles para los equipos de cliente de Internet que tienen acceso a la detección de asociado de cuenta formulario Web. Puedes cambiar cómo se presenta la detección de asociado de cuenta formulario Web a los equipos cliente personalizando el archivo discoverclientrealm.aspx.  
  
-   **Redireccionamiento de token de seguridad**: el proxy de servidor de federación de asociado de cuenta envía los tokens de seguridad para el asociado de recurso. El proxy de servidor de federación de recursos acepta estos tokens y pasa el servidor de federación de asociado de recurso. El servidor de federación de recursos, a continuación, emite un token de seguridad que está enlazado a un servidor Web de recurso específico. El proxy de servidor de federación de recursos, a continuación, redirige el token al cliente.  
  
Para resumir, un proxy de servidor de federación de recursos facilita el proceso de inicio de sesión federado redirigiendo los equipos cliente a un servidor de federación que puede autenticar a los clientes. Un proxy de servidor de federación de recursos también actúa como proxy para tokens de seguridad de cliente en servidores de federación de recursos.  
  
> [!NOTE]  
> Cuando sea necesario ayudar a reducir la cantidad de hardware y el número de certificados obligatorios, el proxy de servidor de federación puede encontrarse en el mismo equipo que el servidor Web.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

