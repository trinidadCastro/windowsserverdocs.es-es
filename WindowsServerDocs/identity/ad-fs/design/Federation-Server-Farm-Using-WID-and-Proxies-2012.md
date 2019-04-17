---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: "Federación granja de servidores con WID y servidores proxy"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0d911b6b12e72a0dc066abc3d6418f5b6e70738
ms.sourcegitcommit: 000db22340f6b076c20d9d040f666fb2732bbc32
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Federación granja de servidores con WID y servidores proxy

>Se aplica a: Windows Server 2012

Esta topología de implementación para los servicios de federación de Active Directory \(AD FS\) es idéntica a la granja de servidores de federación con topología \(WID\) base de datos interna de Windows, pero agrega a proxies de servidor de federación a la red perimetral para admitir los usuarios externos. Los proxy de federación servidor redirigen las solicitudes de autenticación de cliente que provienen de fuera de la red corporativa a los servidores de federación.  
  
## <a name="deployment-considerations"></a>Consideraciones de implementación  
Esta sección describe diversas consideraciones acerca de la audiencia de destino, ventajas y limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 configurado las relaciones de confianza que se deben proporcionar tanto sus usuarios internos y externos a los usuarios \ (que se iniciaron sesión en equipos que se encuentran físicamente fuera el red\ corporativa) con solo sign\ \(SSO\) acceso a los servicios o aplicaciones federadas  
  
-   Organizaciones que necesitan proporcionar tanto sus usuarios internos y externos a los usuarios con acceso de inicio de sesión ÚNICO a Microsoft Office 365  
  
-   Organizaciones pequeñas que tienen los usuarios externos y requieren servicios redundantes, escalables  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas, como se muestra para la [federación servidor granja usando WID](Federation-Server-Farm-Using-WID-2012.md) topología, además de la ventaja de proporcionar acceso adicional para los usuarios externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones según se indiquen para la [federación servidor granja usando WID](Federation-Server-Farm-Using-WID-2012.md) topología  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de red y la ubicación de servidor  
Para implementar esta topología, además de agregar dos proxies de servidor de federación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un servidor de sistema de nombres de dominio \(DNS\) y a un segundo host \(NLB\) equilibrio de carga de red. El segundo host NLB debe estar configurado con un clúster NLB que use una dirección IP de clúster Internet\ accesible y debe usar la misma configuración de nombre de clúster DNS como el clúster NLB anterior que configuraste en el \(fs.fabrikam.com\) red corporativa. Los servidores proxy de servidor de federación también deben estar configurados con direcciones IP Internet\ accesible.  
  
La siguiente ilustración muestra la granja de servidores de federación existente con topología WID que se ha descrito anteriormente, y cómo la empresa ficticia de Fabrikam, Inc., proporciona acceso a un servidor DNS perímetro, agrega un segundo host NLB con la misma clúster DNS nombre \(fs.fabrikam.com\) y agrega dos proxies de servidor de federación \(fsp1 and fsp2\) a la red de perímetro.  
  
![Granja de servidores con WID](media/FarmWIDProxies.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de servidor de federación, consulta uno [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombre de Proxies de servidor de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
