---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Crear un diseño de puente de vínculos a sitios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813996"
---
# <a name="creating-a-site-link-bridge-design"></a>Crear un diseño de puente de vínculos a sitios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un puente de vínculos de sitio conecta dos o más sitio vincula y habilita la transitividad entre los vínculos a sitios. Cada vínculo de sitio de un puente debe tener un sitio en común con otro vínculo de sitio en el puente. El Comprobador de coherencia de la información (KCC) utiliza la información de cada vínculo de sitio para calcular el costo de la replicación entre sitios en un vínculo de sitio y en los vínculos a otros sitios del puente. Sin la presencia de un sitio común entre los vínculos a sitios, el KCC también no puede establecer conexiones directas entre los controladores de dominio en los sitios que están conectados mediante el mismo puente de vínculo de sitio.  
  
De forma predeterminada, todos los vínculos a sitios son transitivos. Se recomienda que mantenga habilitada, no cambie el valor predeterminado de la transitividad **enlazar todos los vínculos a sitios** (habilitado de forma predeterminada). Sin embargo, deberá deshabilitar **enlazar todos los vínculos a sitios** y completar un diseño de puente de vínculo de sitio si:  

- La red IP no se enruta por completo. Cuando deshabilite **enlazar todos los vínculos a sitios**, todos los vínculos a sitios se consideran no transitivas y puede crear y configurar objetos de puente de vínculo de sitio para modelar el comportamiento de enrutamiento real de la red.  
- Necesario controlar el flujo de replicación de los cambios realizados en los servicios de dominio de Active Directory (AD DS). Deshabilitando **enlazar todos los vínculos a sitios** para el transporte IP de vínculo de sitio y configurar un puente de vínculos de sitio, el puente de vínculo de sitio, se convierte en el equivalente de una red separado. Todos los vínculos a sitios dentro del puente de vínculo de sitio pueden enrutar de manera transitiva, pero no se enrutan fuera el puente de vínculos de sitio.  

Para obtener más información sobre cómo usar el complemento Servicios y sitios de Active Directory para deshabilitar la **enlazar todos los vínculos a sitios** establecer, consulte el artículo [habilitar o deshabilitar los puentes de vínculos](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controlar el flujo de replicación de AD DS

Dos escenarios en que necesite un diseño de puente de vínculo de sitio para controlar el flujo de replicación incluyen controlar la conmutación por error de replicación y controlar la replicación a través de un firewall.  
  
### <a name="controlling-replication-failover"></a>Controlar la conmutación por error de replicación

Si su organización tiene una topología de red de concentrador y radio, generalmente no desea que los sitios de satélite para crear conexiones de replicación a otros sitios de satélite si se producen errores en todos los controladores de dominio en el sitio concentrador. En estos escenarios, se debe deshabilitar **enlazar todos los vínculos a sitios** y crear puentes de vínculos de sitio para que se crean las conexiones de replicación entre el sitio satélite y otro sitio del concentrador que es sólo una o dos saltos fuera del sitio satélite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controlar la replicación a través de un firewall

Si dos controladores de dominio que representa el mismo dominio en dos sitios distintos se permitan específicamente para comunicarse entre sí únicamente a través de un firewall, puede deshabilitar **enlazar todos los vínculos a sitios** y crear puentes de vínculos de sitio para sitios en el mismo lado del servidor de seguridad. Por lo tanto, si la red está separada por los servidores de seguridad, se recomienda que deshabilite la transitividad de vínculos a sitios y crear puentes de vínculos de sitio para la red en un lado del servidor de seguridad. Para obtener información acerca de cómo administrar la replicación a través de firewalls, consulte el artículo [Active Directory en redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=107074).
