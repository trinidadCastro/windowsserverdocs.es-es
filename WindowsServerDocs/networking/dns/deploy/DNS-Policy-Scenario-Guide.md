---
title: Guía de escenario de directiva DNS
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7eab7aa403271992e9bc38f20ca0ddfa52bdd9cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policy-scenario-guide"></a>Guía de escenario de directiva DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Esta guía está prevista para su uso por los administradores de sistemas, red y DNS.  
  
Directiva de DNS es una nueva característica de DNS en Windows Server&reg; 2016. Puedes usar a esta guía para aprender a usar la directiva DNS para controlar la forma en que un servidor DNS procesa consultas de resolución de nombres en función de parámetros diferentes que defines en las directivas.   
  
Esta guía contiene información general de directiva DNS, así como los escenarios específicos de directiva DNS que se proporcionan instrucciones sobre cómo configurar el comportamiento del servidor DNS para alcanzar sus objetivos, incluida la administración de tráfico en función de ubicación geográfica servidores DNS principales y secundarios, alta disponibilidad de la aplicación, produzca DNS y mucho más.  
  
Esta guía contiene las siguientes secciones.  
  
- [Introducción a las directivas DNS](DNS-Policies-Overview.md)  
- [Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con servidores principales](primary-geo-location.md)  
- [Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con las implementaciones de principal secundario](primary-secondary-geo-location.md)  
- [Usar la directiva de DNS para las respuestas DNS inteligente en función de la hora del día](dns-tod-intelligent.md)
- [Las respuestas DNS en función de la hora del día con una Azure servidor de aplicaciones en la nube](dns-tod-azure-cloud-app-server.md)
- [Usar la directiva de DNS para la implementación de DNS produzca](split-brain-DNS-deployment.md)
- [Usar la directiva de DNS para produzca DNS en Active Directory](dns-sb-with-ad.md)
- [Usar la directiva de DNS para la aplicación de filtros en las consultas DNS](apply-filters-on-dns-queries.md)
- [Usar la directiva de DNS para equilibrio de carga de aplicación](app-lb.md)
- [Usar la directiva de DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica](app-lb-geo.md)

