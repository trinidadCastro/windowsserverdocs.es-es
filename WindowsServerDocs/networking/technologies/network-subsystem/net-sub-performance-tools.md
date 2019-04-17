---
title: Herramientas de rendimiento de las cargas de trabajo de la red
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>Herramientas de rendimiento de las cargas de trabajo de la red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre herramientas de rendimiento.

Este tema contiene secciones acerca del cliente al servidor tráfico herramienta, tamaño de la ventana de TCP/IP y Asesor de rendimiento de servidor de Microsoft.

##  <a name="bkmk_tuning"></a>Cliente de la herramienta de servidor tráfico

El cliente de la herramienta de servidor tráfico \(ctsTraffic\) proporciona la capacidad para crear y comprobar el tráfico de red.

Para obtener más información y descargar la herramienta, consulta [ctsTraffic (tráfico de cliente y servidor)](http://ctstraffic.codeplex.com/).
  
##  <a name="bkmk_size"></a>Tamaño de la ventana de TCP/IP

Para adaptadores de 1 GB, la configuración que se muestra en la tabla anterior se deben proporcionar buen rendimiento debe NTttcp establece el tamaño de ventana predeterminado TCP en 64 K a través de un procesador lógico específico opción \(SO_RCVBUF\) para la conexión. Esto proporciona un buen rendimiento en una red de latencia baja.  

En cambio, para redes de alta latencia o adaptadores de 10 GB, el valor de tamaño de ventana TCP predeterminado para NTttcp genera menos de un rendimiento óptimo. En ambos casos, se debe ajustar el tamaño de ventana TCP para permitir que el producto del retraso de ancho de banda más grande.  

Estáticamente puede establecer el tamaño de ventana TCP en un gran valor mediante el **-rb** opción. Esta opción desactiva el ajuste automático de la ventana de TCP, y te recomendamos que uses, solo si el usuario comprende completamente el cambio resultante en el comportamiento de TCP/IP. De manera predeterminada, el tamaño de ventana TCP se establece en un valor suficiente y ajusta solo con una carga grande o a través de vínculos de alta latencia.  

##  <a name="bkmk_advisor"></a>Asesor de rendimiento de servidor de Microsoft

\(SPA\) Asesor de rendimiento de servidor de Microsoft ayuda a los administradores de TI a recopilar métricas para identificar, comparar y diagnosticar posibles problemas de rendimiento de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o implementación de Windows Server 2008. 

SPA genera informes completos de diagnósticos y de gráficos, y proporciona recomendaciones para ayudarte a rápidamente analizar los problemas y desarrollar acciones correctivas.  
  
 Para obtener más información y descargar el asesor, consulta [Asesor de rendimiento de servidor de Microsoft](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) en el centro de desarrollo de Hardware de Windows.

Para obtener vínculos a todos los temas de esta guía, consulte [optimización del rendimiento de red subsistema](net-sub-performance-top.md).