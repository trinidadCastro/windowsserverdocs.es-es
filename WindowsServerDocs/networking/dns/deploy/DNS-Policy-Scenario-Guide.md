---
title: Guía de escenarios de la directiva DNS
description: Este tema forma parte de la guía del escenario de la Directiva DNS para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2168bc6d2f2b3a5f365bb2738a15ce7f96408de2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317922"
---
# <a name="dns-policy-scenario-guide"></a>Guía de escenarios de la directiva DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía está pensada para su uso por parte de administradores de sistemas, redes y DNS.  
  
La Directiva de DNS es una característica nueva para DNS en Windows Server&reg; 2016. Puede usar esta guía para obtener información sobre cómo usar la Directiva de DNS para controlar cómo un servidor DNS procesa las consultas de resolución de nombres en función de los distintos parámetros que se definen en las directivas de.   
  
Esta guía contiene información general sobre la Directiva de DNS, así como escenarios específicos de la Directiva de DNS que proporcionan instrucciones sobre cómo configurar el comportamiento del servidor DNS para lograr sus objetivos, incluida la administración de tráfico basada en la ubicación geográfica para los principales y servidores DNS secundarios, alta disponibilidad de aplicaciones, DNS de cerebro dividido y mucho más.  
  
En esta guía se incluyen las siguientes secciones.  
  
- [Información general sobre directivas DNS](DNS-Policies-Overview.md)  
- [Usar la Directiva DNS para la administración del tráfico basada en la ubicación geográfica con los servidores principales](primary-geo-location.md)  
- [Uso de la Directiva de DNS para la administración del tráfico basada en la ubicación geográfica con las implementaciones principales y secundarias](primary-secondary-geo-location.md)  
- [Usar la Directiva DNS para respuestas DNS inteligentes basadas en la hora del día](dns-tod-intelligent.md)
- [Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure](dns-tod-azure-cloud-app-server.md)
- [Uso de la Directiva de DNS para la implementación de DNS de cerebro dividido](split-brain-DNS-deployment.md)
- [Usar la Directiva de DNS para DNS de cerebro dividido en Active Directory](dns-sb-with-ad.md)
- [Usar la Directiva de DNS para aplicar filtros en consultas DNS](apply-filters-on-dns-queries.md)
- [Usar la Directiva de DNS para el equilibrio de carga de la aplicación](app-lb.md)
- [Usar la Directiva de DNS para el equilibrio de carga de la aplicación con reconocimiento de ubicación geográfica](app-lb-geo.md)

