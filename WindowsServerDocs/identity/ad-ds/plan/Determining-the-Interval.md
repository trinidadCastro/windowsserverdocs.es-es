---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Determinar el intervalo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57b282102f10a595ce3842ac3a4eb1d289b86d22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822166"
---
# <a name="determining-the-interval"></a>Determinar el intervalo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debe establecer la propiedad de intervalo de replicación de vínculo de sitio para indicar con qué frecuencia desea que la replicación que se produzca durante las horas de la programación permite la replicación. Por ejemplo, si la programación permite la replicación entre las horas de 02:00 y 04:00 horas y el intervalo de replicación se establece durante 30 minutos, la replicación puede producirse hasta cuatro veces durante el tiempo programado. El intervalo de replicación predeterminado es 180 minutos o 3 horas. El intervalo mínimo es 15 minutos.  
  
Tenga en cuenta los siguientes criterios para determinar con qué frecuencia se produce la replicación dentro de la ventana de programación:  
  
-   Un intervalo pequeño reduce la latencia, pero aumenta la cantidad de área extensa (WAN) de tráfico de red.  
  
-   Para mantenerse al día las particiones de directorio de dominio, se prefiere una latencia baja.  
  
Con una estrategia de replicación de almacenamiento y reenvío, es difícil determinar cuánto una actualización del directorio puede tardar en replicarse en todos los controladores de dominio. Para proporcionar una estimación conservadora de latencia máxima, realice estas tareas:  
  
-   Crear una tabla de todos los sitios de la red, como se muestra en el ejemplo siguiente:  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Ángeles|||0.25|||  
    |Nueva York||||0.25||  
    |Washington, D.C.|||||0.25|  
  
    Se calcula una peor latencia dentro de un sitio que para ser de 15 minutos.  
  
-   A partir de la programación de replicación, determinar la latencia máxima de replicación que es posible en cualquier vínculo de sitio que conecta dos sitios del concentrador.  
  
    Por ejemplo, si se produce la replicación entre Seattle y Nueva York cada tres horas, el retraso máximo para la replicación entre estos sitios es de tres horas. Si el retraso de replicación entre Seattle y Nueva York es el retraso más largo programado entre todos los sitios del concentrador, la latencia máxima entre todos los concentradores es de tres horas.  
  
-   Para cada sitio del concentrador, cree una tabla de las latencias máximas entre la oficina central y cualquiera de sus sitios de satélite.  
  
    Por ejemplo, si se produce la replicación entre Nueva York y Washington, D.C., cada cuatro horas y este es el retraso de replicación más largo entre Nueva York y cualquiera de sus sitios de satélite, la latencia máxima entre Nueva York y su ensamblados satélite es cuatro horas.  
  
-   Combinar estas latencias máximas para determinar la latencia máxima de toda la red.  
  
    Por ejemplo, si la latencia máxima entre Seattle y su sitio satélite en Los Ángeles es un día, la latencia máxima de replicación para este conjunto de vínculos (Washington, D.C.-Nueva York-Seattle-Los Ángeles) es 31 horas, es decir, 4 (Washington, D.C.-Nueva York) + (nuevo 3 York-Seattle) + 24 (Seattle-Los Ángeles), tal y como se muestra en la tabla siguiente.  
  
    |Sitios|Seattle|Boston|Los Ángeles|Nueva York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4,00|4,00|  
    |Los Ángeles|||0.25|24 + 3|24+3+4|  
    |Nueva York||||0.25|4,00|  
    |Washington, D.C.|||||0.25|  
  


