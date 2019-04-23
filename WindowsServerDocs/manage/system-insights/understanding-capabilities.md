---
title: Descripción de funcionalidades
description: En este tema define el concepto de capacidades en información del sistema y se presenta las características predeterminadas disponibles en Windows Server 2019.
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
ms.date: 6/05/2018
ms.openlocfilehash: 21932a3e45d7fc6711400eb30c63c3412bbc116e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830976"
---
# <a name="understanding-capabilities"></a>Descripción de funcionalidades

>Se aplica a: Windows Server 2019

En este tema define el concepto de capacidades en información del sistema y se presenta las características predeterminadas disponibles en Windows Server 2019. 

En este tema también se describe los orígenes de datos, escalas de tiempo de predicción y los Estados de predicción que usa para las características predeterminadas. 

## <a name="capability-overview"></a>Introducción a las capacidades
Una funcionalidad de información del sistema es un aprendizaje automático o modelo de estadísticas que analiza los datos del sistema para que le ayudarán a tener aumentó información detallada sobre el funcionamiento de la implementación. Información del sistema presenta un conjunto inicial de características predeterminadas y permite agregar nuevas capacidades de forma dinámica, sin necesidad de actualizar el sistema operativo. 

>[!NOTE]
>Documentación detallada que se explica cómo crear, agregar y capacidades de actualización está disponible [aquí](adding-and-developing-capabilities.md), y [el documento de capacidades de administración](managing-capabilities.md) proporciona información general sobre esto funcionalidad.

Además, cada función se ejecuta localmente en una instancia del servidor de Windows, y se puede administrar individualmente cada capacidad.

### <a name="capability-outputs"></a>Salidas de capacidad
Cuando se invoca una funcionalidad, proporciona una salida para ayudar a explicar el resultado de su análisis o predicción. Cada salida debe contener un **estado** y un **descripción del estado** describir la predicción y cada resultado opcionalmente puede contener datos específicos de la funcionalidad asociadas con la predicción. El **descripción del estado** ayuda a proporcionar una explicación contextual para la **estado**, y la funcionalidad de informes una **Aceptar**, **advertencia**, o **crítico** estado. Además, puede usar una funcionalidad un **Error** o **ninguno** estado si se ha realizado ninguna predicción. Juntos, estos son los Estados de funcionalidad y su significado básico: 

- **Aceptar** -todo es correcto.
- **Advertencia** : no requiere ninguna atención inmediata, pero debería echar un vistazo. 
- **Crítica** -debería echar un vistazo en breve. 
- **Error** -un problema desconocido provocó la capacidad de producir un error. 
- **Ninguno** -se ha realizado ninguna predicción. Esto puede deberse a falta de datos o cualquier otro motivo de la funcionalidad específica para no realizar una predicción. 

Además, los datos específicos de la funcionalidad contenidos en el resultado se colocará en un archivo JSON accesibles para el usuario y la ruta de acceso de archivo [puede encontrarse con PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Funciones predeterminadas
En Windows Server 2019, información del sistema presenta cuatro funciones predeterminadas centrados en la previsión de capacidad:

- **Previsiones de capacidad de CPU** -uso de CPU de las previsiones. 
- **Previsiones de capacidad de red** -previsiones de uso para cada adaptador de red de red. 
- **Previsión de consumo de almacenamiento total** -consumo de almacenamiento total de las previsiones en todas las unidades locales. 
- **Previsión de consumo de volumen** -previsiones de consumo de almacenamiento para cada volumen.

Analiza cada funcionalidad más allá de los datos históricos para predecir el futuro, y **todas las funcionalidades de predicción están diseñados para predecir tendencias a largo plazo, en lugar de comportamiento a corto plazo**, ayudando a los administradores correctamente Aprovisionamiento de hardware y optimizar sus cargas de trabajo para evitar la contención de recursos futuro. Dado que estas capacidades se centran en el uso a largo plazo, estas capacidades analizar datos diarios. 

### <a name="forecasting-model"></a>Modelo de previsión
Las características predeterminadas usan un modelo de pronóstico para predecir el futuro, y para cada predicción, el modelo se entrena localmente en los datos de la máquina. Este modelo está diseñado para ayudar a detectar tendencias a plazo más largo y reciclaje en cada instancia del servidor de Windows permite la capacidad de adaptarse al comportamiento específico y los matices de uso de cada máquina.

>[!NOTE]
>Determinar qué tipo de modelo que se usa necesario probar muchos modelos con un conjunto de datos que contienen decenas de miles de máquinas. Después de analizar y ajustar estos modelos, decidimos usar un modelo de previsión con regresión automática, tal como lo genera predicciones muy precisos y visualmente intuitivas no sólo requiere demasiado tiempo para entrenar. Este modelo, sin embargo, requiere tres semanas de datos de entrenamiento, por lo que cada función usa una tendencia lineal básica hasta tres semanas de los datos están disponibles.

### <a name="forecasting-timelines"></a>Previsión de las escalas de tiempo
Las características predeterminadas de previsión un número determinado de días en el futuro en función del número de días para el que se han recopilado los datos. La siguiente tabla muestra las escalas de tiempo de predicción de estas capacidades:

| Tamaño de datos de entrada | Predecir duración |
| --------------- | --------------- |
| 0-5 días | No se realiza ninguna predicción. |
| 6-180 días | 1/3 * tamaño de los datos de entrada |
| 180-365 días | 60 días | 

### <a name="forecasting-data"></a>Datos de previsión
Cada capacidad analiza los datos diarios para prever el uso futuro. Uso de almacenamiento de CPU, red e incluso, sin embargo, puede cambiar con frecuencia a lo largo del día, ajustar de forma dinámica las cargas de trabajo en el equipo. Dado que uso no constante a lo largo del día, es importante representar correctamente el uso diario en un único punto de datos. En la tabla siguiente se describe los puntos de datos específicos y cómo se procesan los datos:


| Nombre de función | Orígenes de datos | Lógica de filtrado |
| --------------- | -------------- | ---------------- |
 Previsión de consumo de volumen          | Tamaño del volumen                    | Uso diario máximo              
 Previsión de consumo de almacenamiento total   | Suma de los tamaños de volumen, la suma de los tamaños de disco              | Uso diario máximo             
 Previsiones de capacidad de CPU                | % Processor Time  | Promedio máximo de 2 horas al día   
 Previsiones de capacidad de red         | Bytes totales/seg.         | Promedio máximo de 2 horas al día  

Al evaluar la lógica de filtrado anterior, es importante tener en cuenta que cada capacidad busca informar a los administradores cuando uso futuro superará significativamente la capacidad disponible: aunque la CPU alcanza momentáneamente el 100% de uso, no puede tener el uso de CPU la causa de contención de recursos o la degradación del rendimiento significativa. Para la CPU y las redes, a continuación, debe haber un uso elevado de sostenido en lugar de picos momentáneas. Calcular el promedio de CPU y uso a lo largo de todo el día de la red, sin embargo, perdería información de uso importantes, como unas pocas horas de CPU alta o redes de uso podrían afectar significativamente el rendimiento de las cargas de trabajo críticas. El promedio máximo de 2 horas durante cada día evita estos extremos y todavía genera datos significativos para cada capacidad analizar.

Para el volumen y el uso de almacenamiento total, sin embargo, el uso de almacenamiento no puede superar la capacidad disponible, ni siquiera momentáneamente, por lo que se usa el máximo uso diario para estas capacidades. 

### <a name="forecasting-statuses"></a>Estados de previsión
Todas las funcionalidades de la información del sistema deben salida un estado asociado con cada predicción. Cada capacidad predeterminada usa la lógica siguiente para definir cada estado de predicción:
- **ACEPTAR**: La previsión no supera la capacidad disponible.
- **Advertencia**: La previsión supera la capacidad disponible en los próximos 30 días. 
- **Crítico**: La previsión supera la capacidad disponible en los próximos 7 días. 
- **Error**: La capacidad de produjo un error inesperado. 
- **Ninguna**: No hay suficientes datos para realizar una predicción. Esto puede deberse a falta de datos o porque no se ha notificado ningún dato recientemente.

>[!NOTE]
>Si un previsiones de capacidad en varias instancias - como varios volúmenes o adaptadores de red: el estado refleja el estado más grave en todas las instancias. Los Estados individuales para cada adaptador de red o de volumen son visibles en Windows Admin Center o dentro de los datos contenidos en la salida de cada función. Para obtener instrucciones sobre cómo analizar la salida JSON de las capacidades de forma predeterminada, visite [este blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)
