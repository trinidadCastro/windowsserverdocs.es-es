---
title: Preguntas más frecuentes del sistema Insights
description: Preguntas más frecuentes del sistema Insights
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851066"
---
# <a name="system-insights-faq"></a>Preguntas más frecuentes del sistema Insights

>Se aplica a: Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>¿Cómo puede usar información del sistema con Azure Monitor o System Center Operations Manager?

[Azure Monitor](https://azure.microsoft.com/services/monitor/) y [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) proporcionan información operativa a través de las implementaciones que le ayudarán a administrar su infraestructura. Información del sistema, por el contrario, es una característica de Windows Server que presenta capacidades de análisis predictivo local. Juntos, información del sistema y Azure Monitor o SCOM puede ayudar a las predicciones de superficie a través de una población de dispositivos:

 Azure Monitor o SCOM clave puede desactivar los eventos creados por la información del sistema, como sistema de información envía el resultado de cada predicción para el registro de eventos. Pueden mostrar estas predicciones específicas del equipo a todo un conjunto de servidores de Windows, lo que le permite tener una vista unificada de estas predicciones en un grupo de instancias de servidor. 
 
 Consulte los identificadores de canal y evento para cada predicción [aquí](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>¿Cómo se relacionan con información del sistema para el aprendizaje automático de Windows?

[Aprendizaje automático de Windows](https://docs.microsoft.com/windows/uwp/machine-learning/) es una plataforma que permite a los desarrolladores importar y puntuar modelos de aprendizaje automático entrenado previamente en dispositivos de Windows. Estos modelos se benefician de aceleración de hardware, y puede puntuar localmente. 

Información del sistema es una característica de Windows Server 2019 que ofrece funcionalidades de predicción locales junto con una experiencia de administración completa, incluida la integración de PowerShell y Windows Admin Center. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>¿Puedo usar información del sistema para el clúster? 

Sí. Información del sistema puede ejecutar de manera independiente en cada nodo del clúster de conmutación por error individuales y el comportamiento predeterminado del uso de las previsiones de información del sistema a través de redes, volumen, CPU y almacenamiento local. **También puede habilitar la previsión para el almacenamiento en clúster**, por lo que las capacidades de almacenamiento previsión predicción el uso de los volúmenes en clúster y almacenamiento. 

Puede administrar esta configuración en Windows Admin Center o PowerShell y la información más detallada sobre esta funcionalidad está disponible [aquí](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>¿Cuánto cuesta es ejecutar las funciones de forma predeterminada?

Cada capacidad predeterminada es económico ejecutar. Cada capacidad tardará más tiempo en ejecutarse como recopilar más datos, pero normalmente debe completar en unos segundos. 

## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de descripción](understanding-capabilities.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
