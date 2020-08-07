---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Granja de servidores de federación con WID y servidores proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 372d2ff3c372815823261e7e80a05db088c88331
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945439"
---
# <a name="legacy-ad-fs-federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de Federación de AD FS heredada con WID y servidores proxy

Esta topología de implementación de Servicios de federación de Active Directory (AD FS) (AD FS) es idéntica a la granja de servidores de Federación con la topología de Windows Internal Database (WID), pero agrega equipos proxy a la red perimetral para admitir usuarios externos. Estos servidores proxy redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de Federación. En versiones anteriores de AD FS, estos servidores proxy se denominaban servidores proxy de Federación.

> [!IMPORTANT]
> En Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2, el rol de un servidor proxy de Federación se administra mediante un nuevo servicio de rol de acceso remoto denominado proxy de aplicación Web. Para habilitar la AD FS para la accesibilidad desde fuera de la red corporativa, que era el propósito de implementar un servidor proxy de Federación en versiones heredadas de AD FS, como AD FS 2,0 y AD FS en Windows Server 2012, puede implementar uno o varios proxy de aplicación web para AD FS en Windows Server 2012 R2.
>
> En el contexto de AD FS, el proxy de aplicación web funciona como un proxy de servidor de Federación AD FS. Además, el servicio Proxy de aplicación web ofrece funcionalidad de proxy inverso para las aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo acceso a estas desde fuera de la red corporativa. Para obtener información general sobre el servicio de rol Proxy de aplicación web, consulte la introducción a Proxy de aplicación web.
>
> Para planear la implementación de Proxy de aplicación web, puede consultar la información de los temas siguientes:
>
> - [Planear la infraestructura del proxy de aplicación web (WAP)](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))
> - [Planear el servidor de Proxy de aplicación web](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

## <a name="deployment-considerations"></a>Consideraciones de la implementación
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.

### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?

- Organizaciones con 100 o menos relaciones de confianza configuradas que necesitan proporcionar a los usuarios internos y a los usuarios externos (que han iniciado sesión en equipos que están ubicados físicamente fuera de la red corporativa) con acceso de inicio de sesión único (SSO) a servicios o aplicaciones federadas.

- Organizaciones que necesitan proporcionar a los usuarios internos y a los usuarios externos acceso de SSO a Microsoft Office 365

- Organizaciones más pequeñas que tienen usuarios externos y requieren servicios escalables y redundantes

### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?

- Las mismas ventajas que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID.md) la topología WID, además de la ventaja de proporcionar acceso adicional a los usuarios externos.

### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?

- Las mismas limitaciones que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID.md) la topología WID

    | 1-100 relaciones de confianza para usuario autenticado | Más de 100 relaciones de confianza para usuario autenticado |
    |--|--|
    | **1-30 nodos de AD FS:** Compatible con WID | **1-30 nodos de AD FS:** No compatible con WID, se requiere SQL |
    | **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL | **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL |

## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red
Para implementar esta topología, además de agregar dos servidores proxy de aplicación Web, debe asegurarse de que la red perimetral también puede proporcionar acceso a un servidor de sistema de nombres de dominio (DNS) y a un segundo host de equilibrio de carga de red (NLB). El segundo host de NLB debe configurarse con un clúster de NLB que use una dirección IP de clúster accesible desde Internet y debe usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior configurado en la red corporativa (fs.fabrikam.com). Los proxies de aplicación web también deben configurarse con direcciones IP accesibles desde Internet.

En la ilustración siguiente se muestra la granja de servidores de Federación existente con la topología WID que se ha descrito anteriormente y cómo la empresa ficticia Fabrikam, Inc., proporciona acceso a un servidor DNS perimetral, agrega un segundo host de NLB con el mismo nombre DNS de clúster (fs.fabrikam.com) y agrega dos proxies de aplicación web (wap1 y WAP2) a la red perimetral.

![Granja de servidores WID y servidores proxy](media/WIDFarmADFSBlue.gif)

Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxy de aplicación Web, consulte la sección "requisitos de resolución de nombres" en [AD FS requisitos](AD-FS-Requirements.md) y [planear la infraestructura del proxy de aplicación web (WAP)](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)).

## <a name="see-also"></a>Consulte también
[Planeación de la topología](Plan-Your-AD-FS-Deployment-Topology.md) 
 de implementación de AD FS [Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)

