---
title: Guía de escenarios de la directiva DNS
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865646"
---
# <a name="dns-policy-scenario-guide"></a>Guía de escenarios de la directiva DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía sirve para su uso por los administradores de sistemas, red y DNS.  
  
Directiva de DNS es una característica nueva para DNS en Windows Server&reg; 2016. Puede usar a esta guía para aprender a usar la directiva DNS para controlar cómo un servidor DNS procesa las consultas de resolución de nombre en función de los distintos parámetros que definen en las directivas.   
  
Esta guía contiene información general de directiva DNS, así como escenarios específicos de directiva DNS que se proporcionan instrucciones sobre cómo configurar el comportamiento del servidor DNS para lograr sus objetivos, incluida la administración de tráfico en función de ubicación geográfica para el elemento primario y los servidores DNS secundarios, alta disponibilidad de aplicaciones, DNS de cerebro dividido y mucho más.  
  
En esta guía se incluyen las siguientes secciones.  
  
- [Introducción a las directivas DNS](DNS-Policies-Overview.md)  
- [Uso de directiva DNS para la ubicación geográfica en función de administración del tráfico con servidores principales](primary-geo-location.md)  
- [Uso de directiva DNS para la ubicación geográfica en función de administración del tráfico con implementaciones primarias-secundarias](primary-secondary-geo-location.md)  
- [Uso de directiva DNS para las respuestas DNS inteligentes basadas en la hora del día](dns-tod-intelligent.md)
- [Las respuestas DNS según la hora del día con una instancia de Azure en la nube del servidor de aplicaciones](dns-tod-azure-cloud-app-server.md)
- [Uso de directiva DNS para la implementación de DNS de cerebro dividido](split-brain-DNS-deployment.md)
- [Uso de directiva DNS para DNS de cerebro dividido en Active Directory](dns-sb-with-ad.md)
- [Usar la directiva de DNS para aplicar filtros en las consultas de DNS](apply-filters-on-dns-queries.md)
- [Uso de directiva DNS para equilibrio de carga de aplicación](app-lb.md)
- [Uso de directiva DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica](app-lb-geo.md)

