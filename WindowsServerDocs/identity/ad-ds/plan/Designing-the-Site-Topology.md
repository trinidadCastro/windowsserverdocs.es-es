---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Diseño de la topología de sitio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888496"
---
# <a name="designing-the-site-topology"></a>Diseño de la topología de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una topología de sitio del servicio de directorio es una representación lógica de la red física. Diseñar una topología de sitio para los servicios de dominio de Active Directory (AD DS) implica el planeamiento de la ubicación del controlador de dominio y diseñar sitios, subredes, vínculos a sitios y puentes de vínculos de sitio para garantizar el enrutamiento eficaz del tráfico de replicación y la consulta.  
  
Diseñar una topología de sitio ayuda a dirigir consultas de cliente y el tráfico de replicación de Active Directory de forma eficiente. Una topología de sitio bien diseñada le ayuda a su organización lograr las siguientes ventajas:  
  
-   Minimizar el costo de replicación de datos de Active Directory.  
  
-   Minimizar los esfuerzos administrativos que están obligados a mantener la topología del sitio.  
  
-   Programar la replicación que permite a las ubicaciones con vínculos de red lenta o telefónico para replicar datos de Active Directory durante horas de poca actividad.  
  
-   Optimizar la capacidad de los equipos cliente para localizar los recursos más cercanos, como controladores de dominio y servidores de sistema de archivos distribuido (DFS). Esto ayuda a reducir el tráfico de red a través de área extensa (WAN) vínculos de red, mejora los procesos de inicio de sesión y cierre de sesión y acelerar las operaciones de descarga de archivos.  
  
Antes de empezar a diseñar la topología de sitio, debe entender la estructura de red físico. Además, en primer lugar debe diseñar la estructura lógica de Active Directory, incluida la jerarquía administrativa, el plan de bosque y el plan de dominio para cada bosque. También debe completar el diseño de la infraestructura del sistema de nombres de dominio (DNS) para AD DS. Para obtener más información sobre cómo diseñar la estructura lógica de Active Directory y la infraestructura de DNS, consulte [diseño lógico estructura para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Después de completar el diseño de topología del sitio, debe comprobar que los controladores de dominio cumplan los requisitos de hardware para Windows Server 2008 Standard, Windows Server 2008 Enterprise y Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Descripción de la topología de sitio de Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planear la ubicación del controlador de dominio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Crear un diseño de sitio](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Crear un diseño de vínculo de sitio](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Crear un diseño de puente de vínculo de sitio](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Búsqueda de recursos adicionales para el diseño de topología de sitio de Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


