---
title: Herramientas de rendimiento de las cargas de trabajo de la red
description: En este tema forma parte de la Guía de ajuste de rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824936"
---
# <a name="performance-tools-for-network-workloads"></a>Herramientas de rendimiento de las cargas de trabajo de la red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre herramientas de rendimiento.

Este tema contiene las secciones sobre el cliente de Microsoft Server Performance Advisor, tamaño de la ventana de TCP/IP y herramienta de tráfico del servidor.

##  <a name="bkmk_tuning"></a> Cliente a la herramienta de tráfico del servidor

El cliente para el tráfico del servidor \(ctsTraffic\) herramienta le proporciona la capacidad de crear y comprobar el tráfico de red.

Para obtener más información y descargar la herramienta, consulte [ctsTraffic (tráfico de cliente a servidor)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a> Tamaño de la ventana de TCP/IP

Para los adaptadores de 1 GB, la configuración que se muestra en la tabla anterior debe proporcionar un buen rendimiento porque NTttcp establece el tamaño de ventana TCP predeterminado a 64 K a través de una opción específica de procesador lógico \(SO_RCVBUF\) para la conexión. Esto proporciona un buen rendimiento en una red de baja latencia.  

En cambio, para redes de alta latencia o adaptadores de 10 GB, el valor de tamaño de ventana TCP predeterminado para NTttcp produce menor que un rendimiento óptimo. En ambos casos, debe ajustar el tamaño de ventana TCP para permitir que el producto del retraso de ancho de banda mayor.  

Estáticamente, puede establecer el tamaño de ventana TCP en un valor grande mediante el **-rb** opción. Esta opción deshabilita la optimización automática de la ventana TCP, y se recomienda utilizarlo sólo si el usuario comprenda completamente el cambio resultante en el comportamiento de TCP/IP. De forma predeterminada, el tamaño de ventana TCP se establece en un valor suficiente y ajusta sólo bajo mucha carga o a través de vínculos de alta latencia.  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) ayuda a los administradores de TI recopilan métricas para identificar, comparar y diagnosticar posibles problemas de rendimiento en un Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o implementación de Windows Server 2008. 

SPA genera gráficos e informes de diagnóstico completos y proporciona recomendaciones para ayudarle rápidamente problemas de analizar y desarrollar acciones correctivas.  
  
 Para obtener más información y descargar el Asesor de, consulte [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) en el centro de desarrollo de Hardware de Windows.

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste de rendimiento del subsistema de red](net-sub-performance-top.md).