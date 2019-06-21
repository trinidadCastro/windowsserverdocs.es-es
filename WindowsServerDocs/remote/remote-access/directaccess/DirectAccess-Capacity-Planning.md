---
title: Planeación de la capacidad de DirectAccess
description: Puede utilizar este tema para un informe en el rendimiento del servidor de DirectAccess para Windows Server 2012 para ayudarle a planear la capacidad de DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6f0f4a089dd8e99bb9f9815f0900a3c53c9d1ba
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281988"
---
# <a name="directaccess-capacity-planning"></a>Planeación de la capacidad de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este documento es un informe sobre el rendimiento del servidor de DirectAccess de Windows Server 2012. Las pruebas se realizaron con la intención de conocer la capacidad de rendimiento al usar hardware informático de tecnología avanzada y hardware informático de bajo perfil. El mejor o peor rendimiento de la CPU dependía del rendimiento del tráfico de red y de los tipos de clientes utilizados. Una implementación de DirectAccess típica (base de estas pruebas) consta de 1/3 (30 %) de clientes IPHTTPS y de 2/3 (70 %) de clientes Teredo. El rendimiento de los clientes Teredo es superior al de los clientes IPHTTPS en parte porque Windows Server 2012 hace uso del ajuste de escala en lado de recepción (RSS), lo que permite usar todos los núcleos de la CPU. Al estar RSS habilitado, la tecnología Hyper-Threading está deshabilitada en estas pruebas. Además, TCP/IP en Windows Server 2012 admite el tráfico UDP, de modo que los clientes Teredo pueden realizar tareas de equilibrio de carga en las CPU.  
  
Los datos se recabaron de un servidor de bajo perfil (4 núcleos, 4 GB) y de hardware que es más habitual encontrar en un servidor de tecnología avanzada (8 núcleos, 8 GB).  A continuación aparece una captura de pantalla del nuevo administrador de tareas de Windows 8 en hardware de bajo perfil con 750 clientes (562 Teredo, 188 IPHTTPS) ejecuta aproximadamente 77 Mbits/seg. Esto es para simular usuarios que no presentan credenciales de tarjeta inteligente.  
  
Estos resultados de prueba arrojan que el rendimiento de Teredo es mejor que el de IPHTTPS en Windows 8 pero, también, que el uso de ancho de banda tanto de Teredo como de IPHTTPS ha mejorado en comparación con Windows 7.  
  
![Resultados de la prueba](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)  
  
## <a name="high-end-hardware-test-environment"></a>Entorno de prueba de hardware de tecnología avanzada  
En el siguiente gráfico se muestran los resultados del entorno de prueba de rendimiento de hardware de tecnología avanzada. Todos los análisis y resultados de las pruebas se detallan en este documento.  
  
||||  
|-|-|-|  
|Configuración - Hardware|Hardware de bajo perfil (4 GB de RAM, 4 núcleos)|Hardware de tecnología avanzada (8 GB, 8 núcleos)|  
|Doble túnel<br /><br />: PKI<br /><br />-DNS64/NAT64 incluido|750 conexiones simultáneas al 50 % de CPU, 50 % de memoria y rendimiento de la NIC de Corpnet a 75 Mbps. El objetivo máximo es de 1000 usuarios al 50 % de la CPU.|1500 conexiones simultáneas al 50 % de CPU, 50 % de memoria y rendimiento de la NIC de Corpnet a 150 Mbps.|  
## <a name="test-environment"></a>Entorno de prueba

**Topología de la herramienta de rendimiento**  
  
![Entorno de prueba](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)  
  
El banco del entorno de prueba del rendimiento está compuesto por 5 equipos. En la prueba de bajo perfil se usó un servidor de DirectAccess de 4 núcleos y 4 GB y, en la de tecnología avanzada, un servidor de DirectAccess de 8 núcleos y 16 GB. En los entornos tanto de bajo perfil como de tecnología avanzada se usó lo siguiente: un servidor back-end (remitente) y dos equipos cliente (receptores).  Los receptores están divididos entre los dos equipos cliente, ya que, de lo contrario, estarían enlazados a la CPU y ello limitaría el número de clientes y el ancho de banda. En el lado receptor, un simulador simula cientos de clientes (tanto HTTPS como Teredo). Tanto IPsec como DOSp están configurados. RSS está habilitado en el servidor de DirectAccess. El tamaño de cola de RSS está establecido en 8.  Si RSS no se configura, se haría un uso muy elevado de un solo procesador, mientras que el resto de los núcleos estarían infrautilizados. Otro aspecto destacable es que el servidor de DirectAccess es un equipo de 4 núcleos donde la tecnología Hyper-Threading está desactivada.  Esto es así porque RSS funciona únicamente en núcleos físicos y el uso de Hyper-Threading arrojaría resultados sesgados (dicho de otro modo, no todos los núcleos se cargarían uniformemente).  
  
## <a name="testing-results-for-low-end-hardware"></a>Resultados de las pruebas en hardware de bajo perfil:

Las pruebas se realizaron con 1000 y con 750 clientes.  En todos los casos, la distribución del tráfico fue de 70 % para Teredo y de 30 % para IPHTTPS.  En todas las pruebas se usó tráfico TCP a través de Nat64 mediante dos túneles IPsec por cliente.  En todas las pruebas, el uso de memoria fue moderado y el uso de CPU, aceptable.  
  
**Resultados de pruebas individuales:**  
  
En las siguientes secciones se detallan las pruebas una a una. Cada título de sección denota los elementos clave de cada prueba, y tras él se muestra un resumen de los resultados y un gráfico con los datos detallados de los resultados.  
  
**Rendimiento de bajo perfil:  750 clientes, 70/30, rendimiento de rendimiento de 84,17 Mbits/s::**  
  
Las tres pruebas siguientes representan el hardware de bajo perfil.  En la siguiente serie de pruebas hubo 750 clientes con un rendimiento de 84,17 Mbits/s y una distribución del tráfico de 562 para Teredo y 188 para IPHTTPS. La MTU de Teredo se estableció en 1472 y la derivación de Teredo estaba habilitada. La media de uso de CPU fue de 46,42 % en las tres pruebas, mientras que la media de uso de memoria (expresada como un porcentaje de bytes asignados de los 4 GB de memoria total disponible) fue de 25,95 %.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Escenario**|**Cpuavg (del recuento)**|**Mbit/s (corporativo)**|**Mbit/s (internet lado)**|**QMSA activa**|**MMSA activa**|**Uso de memoria (sistema de 4 GB)**|  
|**HW de bajo perfil.  562 clientes Teredo.  188 clientes IPHTTPS.**|47.7472542|84.3|119.13|1502.05|1502.1|26.27 %|  
|**HW de bajo perfil.  562 clientes Teredo.  188 clientes IPHTTPS.**|46.3889778|84.146|118.73|1501.25|1501.2|25,90 %|  
|**HW de bajo perfil.  562 clientes Teredo.  188 clientes IPHTTPS.**|45.113082|84.0494|118.43|1546.14|1546.1|25,68 %|  
  
**1000 clientes, 70/30, rendimiento de 78 Mbits/s::**  
  
Las tres pruebas siguientes representan el rendimiento de hardware de bajo perfil. En la siguiente serie de pruebas hubo 1000 clientes con un rendimiento medio de aproximadamente 78,64 Mbits/s y una distribución del tráfico de 700 para Teredo y 300 para IPHTTPS.  La MTU de Teredo se estableció en 1472 y la derivación de Teredo estaba habilitada.  La media de uso de CPU fue de alrededor de 50,7 %, mientras que la media de uso de memoria (expresada como un porcentaje de bytes asignados de los 4 GB de memoria total disponible) fue de aproximadamente 28,7 %.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Escenario**|**Cpuavg (del recuento)**|**Mbit/s (corporativo)**|**Mbit/s (internet lado)**|**QMSA activa**|**MMSA activa**|**Uso de memoria (sistema de 4 GB)**|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|51.28406247|78.6432|113.19|2002.42|1502.1|25,59 %|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|51.06993128|78.6402|113.22|2001.4|1501.2|30,87 %|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|49.75235617|78.6387|113.2|2002.6|1546.1|30,66 %|  
  
**1000 clientes, 70/30, rendimiento de 109 Mbits/s::**  
  
En la siguiente serie de pruebas hubo 1000 clientes con un rendimiento medio de aproximadamente 109,2 Mbits/s y una distribución del tráfico de 700 para Teredo y 300 para IPHTTPS. La MTU de Teredo se estableció en 1472 y la derivación de Teredo estaba habilitada. La media de uso de CPU fue de alrededor de 59,06 % en las tres pruebas, mientras que la media de uso de memoria (expresada como un porcentaje de bytes asignados de los 4 GB de memoria total disponible) fue de 27,34 %.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Escenario**|**Cpuavg (del recuento)**|**Mbit/s (corporativo)**|**Mbit/s (internet lado)**|**QMSA activa**|**MMSA activa**|**Uso de memoria (sistema de 4 GB)**|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|59.81640675|108.305|153.14|2001.64|2001.6|24,38 %|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|59.46473798|110.969|157.53|2005.22|2005.2|28,72 %|  
|**HW de bajo perfil.  700 clientes Teredo.  300 clientes IPHTTPS.**|57.89089768|108.305|153.14|1999.53|2018.3|24,38 %|  
  
## <a name="testing-results-for-high-end-hardware"></a>Resultados de las pruebas en hardware de tecnología avanzada:  
Las pruebas se realizaron con 1500 clientes. La distribución del tráfico fue de 70 % para Teredo y de 30 % para IPHTTPS. En todas las pruebas se usó tráfico TCP a través de Nat64 mediante dos túneles IPsec por cliente. En todas las pruebas, el uso de memoria fue moderado y el uso de CPU, aceptable.  
  
**Resultados de pruebas individuales:**  
  
En las siguientes secciones se detallan las pruebas una a una. Cada título de sección denota los elementos clave de cada prueba, y tras él se muestra un resumen de los resultados y un gráfico con los datos detallados de los resultados.  
  
**1500 clientes, distribución 70/30, rendimiento de aproximadamente 153,2 Mbits/seg.**  
  
Las cinco pruebas siguientes representan el hardware de tecnología avanzada. En las siguientes series de pruebas hubo 1500 clientes con un rendimiento medio de aproximadamente 153,2 Mbits/s y una distribución del tráfico de 1050 para Teredo y 450 para IPHTTPS. La media de uso de CPU fue de 50,68 % en las cinco pruebas, mientras que la media de uso de memoria (expresada como un porcentaje de bytes asignados de los 8 GB de memoria total disponible) fue de 22,25 %.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Escenario**|**Cpuavg (del recuento)**|**Mbit/s (corporativo)**|**Mbit/s (internet lado)**|**QMSA activa**|**MMSA activa**|**Uso de memoria (sistema de 4 GB)**|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|51.712437|157.029|216.29|3000.31|3046|21,58 %|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|48.86020205|151.012|206.53|3002.86|3045.3|21,15 %|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|52.23979519|155.511|213.45|3001.15|3002.9|22,90 %|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|51.26269767|155.09|212.92|3000.74|3002.4|22,91 %|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|50.15751307|154.772|211.92|3000.9|3002.1|22,93 %|  
|**HW de tecnología avanzada.  1050 clientes Teredo.  450 clientes IPHTTPS.**|49.83665607|145.994|201.92|3000.51|3006|22,03 %|  
  
![Resultados de pruebas de hardware de gama alta](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)  
  


