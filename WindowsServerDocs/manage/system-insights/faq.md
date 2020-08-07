---
title: Preguntas más frecuentes sobre System Insights
description: Preguntas más frecuentes sobre System Insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 9f746e71b64497835fc5f0f90e9b46c03b63fd15
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972002"
---
# <a name="system-insights-faq"></a>Preguntas más frecuentes sobre System Insights

>Se aplica a: Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>¿Cómo se puede usar System Insights con Azure Monitor o System Center Operations Manager?

[Azure monitor](https://azure.microsoft.com/services/monitor/) y [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) proporcionan información operativa a través de las implementaciones para ayudarle a administrar la infraestructura. Por el contrario, System Insights es una característica de Windows Server que presenta las capacidades de análisis predictivo local. Juntos, System Insights y Azure Monitor o SCOM pueden mostrar las predicciones en una población de dispositivos:

 Azure Monitor o SCOM pueden aprovechar los eventos creados por System Insights, ya que System Insights genera el resultado de cada predicción en el registro de eventos. Pueden exponer estas predicciones específicas del equipo en una flota de servidores de Windows, lo que le permite tener una vista unificada de estas predicciones en un grupo de instancias de servidor.

 Vea [aquí](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results)los identificadores de canal y evento para cada predicción.

## <a name="how-does-system-insights-relate-to-windows-ml"></a>¿Cómo se relaciona System Insights con Windows ML?

[Windows ml](https://docs.microsoft.com/windows/uwp/machine-learning/) es una plataforma que permite a los desarrolladores importar y puntuar modelos de aprendizaje automático entrenados previamente en dispositivos Windows. Estos modelos se benefician de la aceleración de hardware y pueden puntuarse localmente.

System Insights es una característica de Windows Server 2019 que ofrece funcionalidades de predicción locales junto con una experiencia de administración completa, incluida la integración del centro de administración de Windows y PowerShell.

## <a name="can-i-use-system-insights-for-my-cluster"></a>¿Puedo usar System Insights para mi clúster?

Sí. System Insights se puede ejecutar de forma independiente en cada nodo de clúster de conmutación por error individual, y el comportamiento predeterminado de System Insights pronostica el uso en el almacenamiento local, el volumen, la CPU y la red. **También puede habilitar la previsión para el almacenamiento en clúster**, por lo que las funcionalidades de previsión de almacenamiento predicen el uso de volúmenes y almacenamiento en clúster.

Puede administrar esta configuración en el centro de administración de Windows o PowerShell, y encontrará información más detallada sobre esta funcionalidad [aquí](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).


## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>¿Cuánto cuesta ejecutar las capacidades predeterminadas?

Cada capacidad predeterminada es barata de ejecutarse. Cada capacidad tardará más tiempo en ejecutarse cuando recopile más datos, pero normalmente deben completarse en unos pocos segundos.

## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Descripción de funcionalidades](understanding-capabilities.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
