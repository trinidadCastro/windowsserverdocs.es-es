---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: "Diseño de la topología de sitio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>Diseño de la topología de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una topología de sitio del servicio de directorio es una representación lógica de la red física. Diseñar una topología de sitio para los servicios de dominio de Active Directory (AD DS) implica planear la ubicación del controlador de dominio y diseñar sitios, subredes, vínculos a sitios y puentes para asegurarte de enrutamiento eficaz de tráfico de consulta y de replicación.  
  
Diseño de una topología de sitio te ayuda a enrutar eficazmente las consultas del cliente y el tráfico de replicación de Active Directory. Una topología de sitio bien diseñada ayuda a tu organización a lograr los siguientes beneficios:  
  
-   Minimiza el coste de replicación de datos de Active Directory.  
  
-   Minimizar los esfuerzos administrativos necesarios para mantener la topología de sitio.  
  
-   Programar la replicación que permite ubicaciones con vínculos de red lenta o telefónico para replicar los datos de Active Directory durante las horas.  
  
-   Optimizar la capacidad de los equipos cliente para buscar los recursos más cercano, como controladores de dominio y servidores de sistema de archivos distribuido (DFS). Esto ayuda a reducir el tráfico de red a través de área extensa (WAN) vínculos de red, mejora los procesos de inicio de sesión y cierre de sesión y acelera las operaciones de descarga de archivo.  
  
Antes de empezar a diseñar la topología de sitio, debes comprender la estructura de la red física. Además, primero debes diseñar la estructura lógica de Active Directory, incluida la jerarquía administrativa, el plan de bosque y el plan de dominio para cada bosque. También debes completar el diseño de la infraestructura de sistema de nombres de dominio (DNS) para AD DS. Para obtener más información sobre cómo diseñar la estructura lógica de Active Directory y la infraestructura de DNS, consulta [diseñar la lógica estructura para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Después de completar el diseño de la topología de sitio, debes comprobar que los controladores de dominio cumplen los requisitos de hardware para Windows Server 2008 Standard, Windows Server 2008 Enterprise y Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Descripción de la topología de sitios de Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Recopilación de información de red](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planeación de la ubicación del controlador de dominio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Crear un diseño de sitio](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Creación de un diseño de vínculo del sitio](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Creación de un diseño de puente](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Encontrar recursos adicionales para el diseño de la topología de sitio de Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


