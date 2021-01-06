---
title: Guía de escenarios de la directiva DNS
description: Obtenga información acerca de cómo usar la Directiva de DNS para controlar cómo un servidor DNS procesa las consultas de resolución de nombres en función de los distintos parámetros que se definen en las directivas de.
manager: brianlic
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: ac146ca0381185e091d96577fe61c0f6cbb226d4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949471"
---
# <a name="dns-policy-scenario-guide"></a>Guía de escenarios de la directiva DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía está pensada para su uso por parte de administradores de sistemas, redes y DNS.

La Directiva de DNS es una característica nueva para DNS en Windows Server &reg; 2016. Puede usar esta guía para obtener información sobre cómo usar la Directiva de DNS para controlar cómo un servidor DNS procesa las consultas de resolución de nombres en función de los distintos parámetros que se definen en las directivas de.

Esta guía contiene información general sobre la Directiva de DNS, así como escenarios específicos de la Directiva DNS que proporcionan instrucciones sobre cómo configurar el comportamiento del servidor DNS para lograr sus objetivos, como la administración del tráfico basado en la ubicación geográfica para los servidores DNS principal y secundario, la alta disponibilidad de la aplicación, el DNS de cerebro dividido y mucho más.

En esta guía se incluyen las siguientes secciones.

- [Información general sobre directivas DNS](DNS-Policies-Overview.md)
- [Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales](primary-geo-location.md)
- [Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias](primary-secondary-geo-location.md)
- [Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día](dns-tod-intelligent.md)
- [Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure](dns-tod-azure-cloud-app-server.md)
- [Usar la Directiva DNS para la implementación de DNS de Split-Brain](split-brain-DNS-deployment.md)
- [Usar la Directiva de DNS para Split-Brain DNS en Active Directory](dns-sb-with-ad.md)
- [Usar la Directiva de DNS para aplicar filtros en consultas DNS](apply-filters-on-dns-queries.md)
- [Uso de la directiva de DNS para el equilibrio de carga de aplicación](app-lb.md)
- [Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica](app-lb-geo.md)

