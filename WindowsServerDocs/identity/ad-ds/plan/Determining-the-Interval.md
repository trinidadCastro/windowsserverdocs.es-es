---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: "Determinar el intervalo de conexión"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d5eb824b3b15c8b0734b2df23b79649778b28e9d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-interval"></a>Determinar el intervalo de conexión

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debes establecer la propiedad de intervalo de replicación de vínculo de sitio para indicar la frecuencia con que desee que se producen en los momentos cuando la programación permite la replicación de replicación. Por ejemplo, si la programación permite la replicación entre 02:00 horas y horas 04:00 y el intervalo de replicación se establece durante 30 minutos, replicación puede producirse hasta cuatro veces durante la hora programada. El intervalo de replicación predeterminado es de 180 minutos o 3 horas. El intervalo mínimo es de 15 minutos.  
  
Ten en cuenta los siguientes criterios para determinar con qué frecuencia se produce la replicación dentro de la ventana de programación:  
  
-   Un intervalo pequeño reduce la latencia pero aumenta la cantidad de área extensa (WAN) del tráfico de red.  
  
-   Para mantener las particiones de directorio de dominio actualizado, se prefiere baja latencia.  
  
Con una estrategia de replicación de almacenamiento y reenvío, es difícil determinar solo el tiempo que puede tardar una actualización del directorio para replicar en todos los controladores de dominio. Para proporcionar una estimación conservadora de latencia máxima, realiza estas tareas:  
  
-   Crear una tabla de todos los sitios de la red, como se muestra en el siguiente ejemplo:  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Ángeles|||0.25|||  
    |Nueva York||||0.25||  
    |Washington, D.C.|||||0.25|  
  
    Una latencia peor dentro de un sitio se calcula que es de 15 minutos.  
  
-   En la programación de replicación, determinar la latencia de replicación máximo que es posible en cualquier vínculo del sitio que conecta dos sitios de concentrador.  
  
    Por ejemplo, si se produce la replicación entre Seattle y Nueva York cada tres horas, el retraso máximo para la replicación entre estos sitios es tres horas como máximo. Si el retraso de replicación entre Nueva York y Seattle es el más largo retraso programado entre todos los sitios de concentrador, la latencia máxima entre todos los concentradores es tres horas como máximo.  
  
-   Para cada sitio hub, crear una tabla de las latencias máximas entre el sitio del concentrador y cualquiera de sus sitios de satélite.  
  
    Por ejemplo, si se produce la replicación entre Nueva York y Washington, D.C., cada cuatro horas y este es el mayor retraso de replicación entre Nueva York y cualquiera de sus sitios de satélite, la latencia máxima entre Nueva York y sus satélites es cuatro horas.  
  
-   Combinar estos latencias máximas para determinar la latencia máxima para toda la red.  
  
    Por ejemplo, si la latencia máxima entre su sitio de satélite en Los Ángeles y Seattle es un día, la latencia de replicación máximo para este conjunto de vínculos (Washington, D.C.-Nueva York-Seattle-Los Ángeles) es 31 horas, es decir, 4 (Washington, D.C.-Nueva York) + 3 (Nueva York y Seattle) + 24 (Seattle-Los Ángeles), como se muestra en la siguiente tabla.  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4.00|4.00|  
    |Los Ángeles|||0.25|24 + 3|24+3+4|  
    |Nueva York||||0.25|4.00|  
    |Washington, D.C.|||||0.25|  
  


