---
title: Herramientas de rendimiento de las cargas de trabajo de la red
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: 09e775bfe956d67adbd70cf4ce3f9461e1c37cf5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405531"
---
# <a name="performance-tools-for-network-workloads"></a>Herramientas de rendimiento de las cargas de trabajo de la red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las herramientas de rendimiento.

Este tema contiene secciones acerca de la herramienta de tráfico de cliente a servidor, el tamaño de la ventana de TCP/IP y el asesor de rendimiento de servidor de Microsoft.

##  <a name="bkmk_tuning"></a>Herramienta de tráfico de cliente a servidor

La herramienta de tráfico de cliente a servidor \(ctsTraffic @ no__t-1 le proporciona la capacidad de crear y comprobar el tráfico de red.

Para obtener más información y para descargar la herramienta, consulte [ctsTraffic (tráfico de cliente a servidor)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a>Tamaño de la ventana TCP/IP

En el caso de los adaptadores de 1 GB, los valores que se muestran en la tabla anterior deben proporcionar un buen rendimiento, ya que NTttcp establece el tamaño predeterminado de la ventana de TCP en 64 K a través de una opción de procesador lógico específico \(SO_RCVBUF @ no__t-1 para la conexión. Esto proporciona un buen rendimiento en una red de baja latencia.  

Por el contrario, en las redes de alta latencia o en los adaptadores de 10 GB, el valor predeterminado de tamaño de la ventana TCP para NTttcp produce un rendimiento inferior al óptimo. En ambos casos, debe ajustar el tamaño de la ventana TCP para permitir el producto de retraso de ancho de banda mayor.  

Puede establecer de forma estática el tamaño de la ventana TCP en un valor grande mediante la opción **-RB** . Esta opción deshabilita el ajuste automático de la ventana de TCP y se recomienda utilizarla solo si el usuario entiende totalmente el cambio resultante en el comportamiento de TCP/IP. De forma predeterminada, el tamaño de la ventana TCP se establece en un valor suficiente y solo se ajusta bajo una carga pesada o a través de vínculos de latencia alta.  

##  <a name="bkmk_advisor"></a>Asesor de rendimiento de servidor de Microsoft

Microsoft Server Performance Advisor \(SPA @ no__t-1 ayuda a los administradores de ti a recopilar métricas para identificar, comparar y diagnosticar posibles problemas de rendimiento en Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Implementación de Windows Server 2008. 

SPA genera informes y gráficos de diagnóstico completos y proporciona recomendaciones que le ayudarán a analizar rápidamente los problemas y a desarrollar acciones correctivas.  
  
 Para obtener más información y descargar el asesor, consulte [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) en el centro de desarrollo de hardware de Windows.

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).