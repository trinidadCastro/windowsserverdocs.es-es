---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Crear un diseño de puente vínculo de sitio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: b77899c0603e89f972f2fbcb705a95b202c5062e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967742"
---
# <a name="creating-a-site-link-bridge-design"></a>Crear un diseño de puente vínculo de sitio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un puente de vínculo a sitios conecta dos o más vínculos a sitios y permite la transitividad entre los vínculos a sitios. Cada vínculo de sitio en un puente debe tener un sitio en común con otro vínculo a sitios en el puente. El comprobador de coherencia de la información (KCC) utiliza la información de cada vínculo de sitio para calcular el costo de replicación entre sitios en un vínculo a sitios y sitios en los otros vínculos de sitio del puente. Sin la presencia de un sitio común entre los vínculos a sitios, el KCC tampoco puede establecer conexiones directas entre los controladores de dominio de los sitios conectados mediante el mismo puente de vínculo a sitio.

De forma predeterminada, todos los vínculos a sitios son transitivos. Se recomienda mantener la transitividad habilitada si no se cambia el valor predeterminado de **enlazar todos los vínculos a sitios** (habilitado de forma predeterminada). Sin embargo, tendrá que deshabilitar **Bridge todos los vínculos a sitios** y completar un diseño de puente de vínculo a sitios si:

- La red IP no está enrutada por completo. Cuando se deshabilita el **puente de todos los vínculos a sitios**, todos los vínculos a sitios se consideran no transitivos y se pueden crear y configurar objetos de puente de vínculos a sitios para modelar el comportamiento de enrutamiento real de la red.
- Debe controlar el flujo de replicación de los cambios realizados en Active Directory Domain Services (AD DS). Al deshabilitar **Bridge todos los vínculos a sitios** para el transporte IP de vínculo a sitios y configurar un puente de vínculo a sitios, el puente de vínculo a sitios se convierte en el equivalente de una red separada. Todos los vínculos a sitios dentro del puente de vínculos a sitios pueden enrutarse de manera transitiva, pero no se enrutan fuera del puente de vínculos a sitios.

Para obtener más información sobre cómo usar el complemento sitios y servicios de Active Directory para deshabilitar la configuración **enlazar todos los vínculos a sitios** , consulte el artículo [habilitar o deshabilitar puentes de vínculos a sitios](/previous-versions/windows/it-pro/windows-server-2003/cc738789(v=ws.10)).

## <a name="controlling-ad-ds-replication-flow"></a>Controlar el flujo de replicación de AD DS

Dos escenarios en los que se necesita un diseño de puente de vínculo de sitio para controlar el flujo de replicación son controlar la conmutación por error de replicación y controlar la replicación a través de un firewall.

### <a name="controlling-replication-failover"></a>Controlar la conmutación por error de replicación

Si su organización tiene una topología de red de concentrador y radio, normalmente no desea que los sitios satélite creen conexiones de replicación a otros sitios satélite si se produce un error en todos los controladores de dominio del sitio del concentrador. En estos escenarios, debe deshabilitar **Bridge todos los vínculos a sitios** y crear puentes de vínculos a sitios para que las conexiones de replicación se creen entre el sitio satélite y otro sitio del concentrador que sea solo uno o dos saltos fuera del sitio satélite.

### <a name="controlling-replication-through-a-firewall"></a>Control de la replicación a través de un firewall

Si dos controladores de dominio que representan el mismo dominio en dos sitios diferentes están específicamente permitidos para comunicarse únicamente entre sí a través de un firewall, puede deshabilitar el **puente de todos los vínculos de sitio** y crear puentes de vínculos de sitio para sitios en el mismo lado del firewall. Por lo tanto, si la red está separada por firewalls, se recomienda que deshabilite la transitividad de los vínculos a sitios y cree puentes de vínculos a sitios para la red en un lado del firewall. Para obtener información sobre la administración de la replicación a través de firewalls, consulte el artículo [Active Directory en redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=107074).
