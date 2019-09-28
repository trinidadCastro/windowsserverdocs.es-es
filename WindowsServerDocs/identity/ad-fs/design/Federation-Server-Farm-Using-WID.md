---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Granja de servidores de federación con WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0a84940018a0e71aaa1b47c7af3aba5966fe0ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408054"
---
# <a name="federation-server-farm-using-wid"></a>Granja de servidores de federación con WID

La topología predeterminada para Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 es una granja de servidores de Federación con Windows Internal Database \(WID @ no__t-3. En esta topología, AD FS usa WID como almacén para la base de datos de configuración AD FS para todos los servidores de Federación que se unen a esa granja. La granja replica y mantiene los datos del Servicio de federación de la base de datos de configuración de todos los servidores de la granja. AD FS en Windows Server 2012 R2 permite a las organizaciones con 100 o menos relaciones de confianza para usuario autenticado configurar granjas de servidores de Federación con WID con hasta 30 servidores.  
  
Al crear el primer servidor de federación en una granja, se crea también un nuevo Servicio de federación. Si usa WID para la base de datos de configuración de AD FS, el primer servidor de Federación que cree en la granja se denominará *servidor de Federación principal*. Esto significa que este equipo está configurado con una copia\/de lectura y escritura de la base de datos de configuración de AD FS.  
  
Todos los demás servidores de Federación configurados para esta granja se denominan *servidores de Federación secundarios* , ya que deben replicar los cambios realizados en el servidor de Federación principal\-en las copias de solo lectura del AD FS base de datos de configuración que almacenan de forma local.  
  
> [!IMPORTANT]  
> Se recomienda el uso de al menos dos servidores de Federación en una\-configuración con equilibrio de carga.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Organizaciones con 100 o menos relaciones de confianza configuradas que necesitan proporcionar a \(los usuarios internos que han iniciado sesión en equipos conectados físicamente a\) la red corporativa\-con inicio de \(sesiónúnicoAcceso\) SSO a aplicaciones o servicios federados  
  
-   Organizaciones que desean proporcionar a sus usuarios internos acceso SSO a Microsoft Online Services o Microsoft Office 365  
  
-   Organizaciones más pequeñas que requieran servicios redundantes y escalables  
  
> [!NOTE]  
> Las organizaciones con bases de datos de mayor tamaño deben considerar el uso de la [granja de servidores de Federación mediante SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología de implementación. Las organizaciones con usuarios que inician sesión desde fuera de la red deben considerar la posibilidad de usar la granja de servidores de Federación con la topología de [WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md) , o la [granja de servidores de Federación mediante SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topología.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Proporciona acceso SSO a los usuarios internos  
  
-   Datos y servicio de Federación redundancia \(cada servidor de Federación replica los cambios en otros servidores de Federación de la misma granja\)  
  
-   WID se incluye con Windows; por lo tanto, no es necesario adquirir SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Una granja WID tiene un límite de 30 servidores de Federación si tiene 100 o menos relaciones de confianza para usuario autenticado.  
  
-   Una granja WID no admite la detección de reproducción de tokens ni \(la resolución de artefactos\) del\)protocolo SAML lenguaje de marcado de aserción de seguridad \(.  
  
En la tabla siguiente se proporciona un resumen del uso de una granja de servidores WID.  Úselo para planear la implementación.  
  
|| 1 \- 100 RP confianzas | Más de 100 confianzas RP |
| --- | --- | --- |
|1 \- 30 nodos AD FS|WID compatible|No se admite con WID: SQL requerido 
|Más de 30 nodos AD FS|No se admite con WID: SQL requerido|No se admite con WID: SQL requerido  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red  
Cuando esté listo para empezar a implementar esta topología en la red, debe planear la colocación de todos los servidores de Federación en la red corporativa detrás de un host NLB \(\) de equilibrio de carga de red que se puede configurar para un clúster NLB. con un sistema \(de nombres de dominio de\) clúster dedicado nombre DNS y dirección IP del clúster.  
  
> [!NOTE]  
> Este nombre DNS de clúster debe coincidir con el nombre de Servicio de federación, por ejemplo, fs.fabrikam.com.  
  
El host de NLB puede usar la configuración definida en este clúster de NLB para asignar solicitudes de cliente a los servidores de Federación individuales. En la ilustración siguiente se muestra cómo la empresa ficticia Fabrikam, Inc., configura la primera fase de su implementación mediante una granja\- \(de servidores de Federación de dos equipos\) FS1 y FS2 con WID y el posicionamiento de un servidor DNS. y un único host de NLB que está conectado a la red corporativa.  
  
![granja de servidores con WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Si se produce un error en este host de NLB único, los usuarios no podrán tener acceso a los servicios o aplicaciones federados. Agregue más hosts de NLB si sus necesidades empresariales no le permiten tener un único punto de error.  
  
Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación, consulte la sección requisitos de resolución de nombres en [AD FS requisitos](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Vea también  
[Planear la topología de la implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

