---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: "Creación de un diseño de puente"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>Creación de un diseño de puente

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un puente conecta a dos o vínculos a sitios más y permite transitividad entre los vínculos. Cada vínculo del sitio de un puente debe tener un sitio en común con otro vínculo del sitio en el puente. El Comprobador de coherencia de la información (KCC) usa la información sobre cada vínculo al sitio para calcular el coste de replicación entre sitios en un vínculo del sitio y en los vínculos a otros sitios del puente. Sin la presencia de un sitio común entre vínculos a sitios, el KCC también no puede establecer conexiones directas entre controladores de dominio en los sitios que están conectados por el mismo puente.  
  
De manera predeterminada, todos los vínculos son transitivos. Te recomendamos que mantengas transitividad habilitada si no cambia el valor predeterminado de **enlazar todos los vínculos a sitios** (habilitado de manera predeterminada). Sin embargo, debes deshabilitar **enlazar todos los vínculos a sitios** y completar un diseño de puente de vínculo de sitio si:  
  
-   La red IP no se enruta completamente. Al deshabilitar **enlazar todos los vínculos a sitios**, todos los vínculos se consideran intransitivas y puede crear y configurar objetos puente vínculos a sitios para modelar el comportamiento de enrutamiento real de la red.  
  
-   Debes controlar el flujo de replicación de los cambios realizados en los servicios de dominio de Active Directory (AD DS). Al deshabilitar **enlazar todos los vínculos a sitios** para el transporte IP del vínculo de sitio y configurar un puente, el puente es el equivalente de una red inconexa. Todos los vínculos en el puente pueden enrutar transitivamente, pero no se enrutan fuera el puente.  
  
Para obtener más información acerca de cómo usar el complemento Servicios y sitios de Active Directory para deshabilitar la **enlazar todos los vínculos a sitios** configuración, vea Habilitar o deshabilitar puentes ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073)).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controlar el flujo de replicación de AD DS  
Dos escenarios en los que necesita un diseño de sitio vínculo puente para controlar el flujo de replicación incluyen permite controlar la conmutación por error de replicación y replicación a través de un firewall.  
  
### <a name="controlling-replication-failover"></a>Controlar la conmutación por error de replicación  
Si tu organización tiene una topología de red de concentrador y radio, por lo general, no desea que los sitios de TV por satélite para crear conexiones de replicación a otros sitios de satélite si no todos los controladores de dominio en el sitio de concentrador. En estos casos, debes deshabilitar **enlazar todos los vínculos a sitios** y crear puentes para que se crean las conexiones de replicación entre el sitio de satélite y otro sitio de concentrador es solo uno o dos saltos fuera del sitio de satélite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controlar la replicación a través de un firewall  
Si dos controladores de dominio que representa el mismo dominio en dos lugares diferentes se hayan permitido específicamente para comunicarse entre ellos solo a través de un firewall, puedes deshabilitar **enlazar todos los vínculos a sitios** y crear puentes de sitios en el mismo lado del servidor de seguridad. Por lo tanto, si la red es separada por firewalls, te recomendamos que deshabilitar transitividad de vínculos a sitios y crear puentes de la red en un lado del servidor de seguridad. Para obtener información sobre la administración de replicación a través del firewall, vea en redes segmentados los servidores de seguridad de Active Directory ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074)).  
  


