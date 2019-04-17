---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: "Federación granja de servidores con WID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bb4e5f88f3d62511b185a2b4317416169717c860
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>Federación granja de servidores con WID

>Se aplica a: Windows Server 2012

La topología de forma predeterminada para los servicios de federación de Active Directory \(AD FS\) es una granja de servidores de federación, usando la \(WID\) base de datos interna de Windows, que consta de hasta cinco servidores de federación de servicios de federación de la organización. En esta topología, AD FS utiliza WID como la tienda para la base de datos de configuración de AD FS para todos los servidores de federación que están unidos a ese conjunto. El conjunto de replica y mantiene los datos de servicios de federación de la base de datos de configuración en cada servidor de la batería.  
  
La acción de crear el primer servidor de federación de un conjunto de también crea un nuevo servicio de federación. Cuando usas WID para la base de datos de configuración de AD FS, el primer servidor de federación que crees en el conjunto se denomina la *servidor de federación principal*. Esto significa que este equipo está configurado con una copia de la base de datos de configuración de AD FS read\ y escritura.  
  
Todos los otros servidores de federación que configurar para este conjunto se conocen como *servidores de federación secundario* porque deben replicar los cambios realizados en el servidor de federación principal para las copias solo read\ de la base de datos de configuración de AD FS que se almacenan localmente.  
  
> [!NOTE]  
> Se recomienda el uso de al menos dos servidores de federación en una configuración de equilibrio de load\.  
  
## <a name="deployment-considerations"></a>Consideraciones de implementación  
Esta sección describe diversas consideraciones acerca de la audiencia de destino, ventajas y limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 configurado las relaciones de confianza que debas proporcionan a los usuarios internos \ (sesión iniciada en equipos que estén conectados físicamente a la red\ corporativa) con solo sign\ \(SSO\) acceso a los servicios o aplicaciones federadas  
  
-   Organizaciones que desean proporcionar sus usuarios internos con acceso de inicio de sesión único a Microsoft Online Services o Microsoft Office 365  
  
-   Organizaciones pequeñas que requieren servicios redundantes, escalables  
  
> [!NOTE]  
> Las organizaciones con bases de datos más grandes consideren el uso del [granja de servidor de federación usar SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología de implementación, que se describe más adelante en esta sección. Deben tener en cuenta las organizaciones que tienen los usuarios que inicien sesión desde fuera de la red ya sea con la [federación servidor granja usando WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md) topología o la [granja de servidor de federación usar SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Proporciona acceso de inicio de sesión único a los usuarios internos  
  
-   Datos y servicios de federación de redundancia \ (cada servidor de federación replica los cambios a otros servidores de federación de la misma farm\)  
  
-   Puede ser el conjunto de escalado agregando hasta cinco de los servidores de federación  
  
-   WID se incluye con Windows; por lo tanto, no es necesario comprar SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Una granja de servidores WID tiene un límite de cinco de los servidores de federación. Para obtener más información, consulta [consideraciones de la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Una granja de servidores WID no es compatible con una resolución de detección o artefacto reproducción token \ (que forma parte de la \(SAML\) protocol\ lenguaje de marcado de aserción de seguridad).  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de red y la ubicación de servidor  
Cuando estés listo para empezar a implementar esta topología en la red, debes planear en todos los servidores de federación de colocar en la red corporativa detrás de un host \(NLB\) equilibrio de carga de red que puede configurarse para un clúster NLB con un nombre de clúster dedicado \(DNS\) sistema de nombres de dominio y la dirección IP.  
  
> [!NOTE]  
> Este nombre DNS de clúster debe coincidir con el nombre de servicio de federación, por ejemplo, fs.fabrikam.com.  
  
El host NLB puede usar la configuración que se define en este clúster NLB asignar solicitudes de cliente a los servidores de federación individuales. La siguiente ilustración muestra cómo la empresa ficticia de Fabrikam, Inc., configura la primera fase de su implementación mediante una granja de servidores de federación de two\ equipo \(fs1 and fs2\) con WID y la colocación de un servidor DNS y un único host NLB que se conecta a la red corporativa.  
  
![Granja de servidores con WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Si hay un error en este host NLB, los usuarios no podrán obtener acceso a los servicios o aplicaciones federadas. Agregar hosts NLB adicionales si los requisitos empresariales no permiten tener un único punto de error.  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación, consulta [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md) en la Guía de diseño de AD FS.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
