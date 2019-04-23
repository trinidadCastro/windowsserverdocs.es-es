---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: Granja de servidores de federación con WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bb4e5f88f3d62511b185a2b4317416169717c860
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851406"
---
# <a name="federation-server-farm-using-wid"></a>Granja de servidores de federación con WID

>Se aplica a: Windows Server 2012

La topología predeterminada para los servicios de federación de Active Directory \(AD FS\) es una granja de servidores de federación, mediante la Windows Internal Database \(WID\), que consta de hasta cinco servidores de federación que hospedan su Servicio de federación de su organización. En esta topología, AD FS utiliza WID como almacén de la base de datos de configuración de AD FS para todos los servidores de federación que se unen a esa granja. La granja replica y mantiene los datos del Servicio de federación de la base de datos de configuración de todos los servidores de la granja.  
  
Al crear el primer servidor de federación en una granja, se crea también un nuevo Servicio de federación. Cuando se usa WID para la base de datos de configuración de AD FS, el primer servidor de federación que se crea en la granja de servidores se conoce como el *servidor de federación principal*. Esto significa que este equipo está configurado con una lectura\/escribir la copia de la base de datos de configuración de AD FS.  
  
Todos los demás servidores de federación que configure para esta granja de servidores se conocen como *servidores de federación secundarios* porque deben replicar los cambios realizados en el servidor de federación principal para la lectura\-solo copias de la base de datos de configuración de AD FS que almacenen localmente.  
  
> [!NOTE]  
> Se recomienda el uso de al menos dos servidores de federación en una carga\-configuración equilibrada.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
Esta sección describen diversas consideraciones acerca de los destinatarios, ventajas y limitaciones asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con relaciones de confianza configurado 100 o menos que deba proporcionar a sus usuarios internos \(ha iniciado sesión en equipos que estén conectados físicamente a la red corporativa\) con inicio de sesión único\-en \(SSO\) acceso a aplicaciones federadas o servicios  
  
-   Organizaciones que desean proporcionar a sus usuarios internos con acceso de inicio de sesión único para Microsoft Office 365 o Microsoft Online Services  
  
-   Organizaciones más pequeñas que requieren servicios redundantes, escalables  
  
> [!NOTE]  
> Las organizaciones con bases de datos mayores deben considerar el uso del [granja de servidores de federación Server utilizando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología de implementación, como se describe más adelante en esta sección. Las organizaciones con usuarios que inician sesión desde fuera de la red deben considerar el uso de cualquiera el [granja de servidores de federación con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md) topología o [granja de servidores de federación Server utilizando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Proporciona acceso de inicio de sesión único a los usuarios internos  
  
-   Redundancia de datos y servicios de federación \(cada servidor de federación replica los cambios en otros servidores de federación en la misma granja\)  
  
-   La granja de servidores se puede escalar horizontalmente mediante la adición de hasta cinco servidores de federación  
  
-   Se incluye con Windows; WID por lo tanto, no hay necesidad de adquirir SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Una granja WID tiene un límite de cinco servidores de federación. Para obtener más información, consulta [Consideraciones sobre la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Una granja WID no admite la resolución de artefacto o de detección de reproducción de tokens \(parte del lenguaje de marcado de aserción de seguridad \(SAML\) protocolo\).  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de selección de ubicación y la red de servidor  
Cuando esté listo para empezar a implementar esta topología de la red, debe planear la ubicación de todos los servidores de federación en su red corporativa detrás de un equilibrio de carga de red \(NLB\) host que se puede configurar para un clúster de NLB con un sistema de nombres de dominio del clúster dedicado \(DNS\) dirección IP de nombre y el clúster.  
  
> [!NOTE]  
> Este nombre DNS del clúster debe coincidir con el nombre del servicio de federación, por ejemplo, fs.fabrikam.com.  
  
El host de NLB puede usar la configuración que se define en este clúster NLB para asignar las solicitudes de cliente a los servidores de federación individuales. La ilustración siguiente muestra cómo la empresa ficticia de Fabrikam, Inc., configura la primera fase de su implementación mediante dos\-granja de servidores de federación de equipo \(fs1 y fs2\) con WID y el posicionamiento de un servidor DNS y un único host de NLB está conectado a la red corporativa.  
  
![granja de servidores con WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Si se produce un error en este host NLB único, los usuarios no podrán tener acceso a aplicaciones federadas o servicios. Agregue más hosts de NLB si sus necesidades empresariales no le permiten tener un único punto de error.  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) en la Guía de diseño de AD FS.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
