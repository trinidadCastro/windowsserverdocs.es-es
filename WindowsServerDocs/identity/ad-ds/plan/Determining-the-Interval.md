---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Determinar el intervalo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f39ad2ce2ce84e36d2faff2a07b8310d3600b6c9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822578"
---
# <a name="determining-the-interval"></a>Determinar el intervalo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debe establecer la propiedad intervalo de replicación de vínculos a sitios para indicar la frecuencia con la que desea que se produzca la replicación durante las horas en las que la programación permite la replicación. Por ejemplo, si la programación permite la replicación entre 02:00 horas y 04:00 horas, y el intervalo de replicación se establece durante 30 minutos, la replicación puede producirse hasta cuatro veces durante el tiempo programado. El intervalo de replicación predeterminado es de 180 minutos o 3 horas. El intervalo mínimo es de 15 minutos.  
  
Tenga en cuenta los siguientes criterios para determinar la frecuencia con que se produce la replicación dentro de la ventana de programación:  
  
-   Un pequeño intervalo reduce la latencia, pero aumenta la cantidad de tráfico de la red de área extensa (WAN).  
  
-   Para mantener actualizadas las particiones de directorio de dominio, se prefiere una latencia baja.  
  
Con una estrategia de replicación de almacenamiento y reenvío, es difícil determinar el tiempo que una actualización de directorio puede tardar en replicarse en todos los controladores de dominio. Para proporcionar una estimación conservadora de la latencia máxima, realice estas tareas:  
  
-   Cree una tabla de todos los sitios de la red, tal como se muestra en el ejemplo siguiente:  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|||||  
    |Boston||0,25||||  
    |Los Ángeles|||0,25|||  
    |Nueva York||||0,25||  
    |Washington, D.C.|||||0,25|  
  
    Una latencia en el peor de los casos dentro de un sitio se calcula como 15 minutos.  
  
-   En la programación de replicación, determine la latencia de replicación máxima que sea posible en cualquier vínculo a sitios que conecte dos sitios de concentrador.  
  
    Por ejemplo, si la replicación se produce entre Seattle y Nueva York cada tres horas, el retraso máximo para la replicación entre estos sitios es de tres horas. Si el retraso de replicación entre Nueva York y Seattle es el retraso programado más largo entre todos los sitios de concentrador, la latencia máxima entre todos los concentradores es de tres horas.  
  
-   Para cada sitio del concentrador, cree una tabla de las latencias máximas entre el sitio del concentrador y cualquiera de sus sitios satélite.  
  
    Por ejemplo, si la replicación se produce entre Nueva York y Washington, D.C., cada cuatro horas y este es el retraso de replicación más largo entre Nueva York y cualquiera de sus sitios satélite, la latencia máxima entre Nueva York y sus satélites es cuatro horas.  
  
-   Combine estas latencias máximas para determinar la latencia máxima de toda la red.  
  
    Por ejemplo, si la latencia máxima entre Seattle y su sitio satélite en los Ángeles es un día, la latencia de replicación máxima para este conjunto de vínculos (Washington, D.C.-New York-Seattle-los Ángeles) es de 31 horas, es decir, 4 (Washington, D.C.-Nueva York) + 3 (Nueva York-Seattle) + 24 (Seattle-los Ángeles), como se muestra en la tabla siguiente.  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|4 + 3|24,00|3,00|4 + 3|  
    |Boston||0,25|4 + 3 + 24|4,00|4,00|  
    |Los Ángeles|||0,25|24 + 3|24 + 3 + 4|  
    |Nueva York||||0,25|4,00|  
    |Washington, D.C.|||||0,25|  
  


