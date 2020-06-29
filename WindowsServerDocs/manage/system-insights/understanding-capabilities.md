---
title: Descripción de funcionalidades
description: En este tema se define el concepto de capacidades de System Insights y se presentan las capacidades predeterminadas disponibles en Windows Server 2019.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: c6738e6e914d97c70aa31af2fe3b6987b0b9ea33
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471750"
---
# <a name="understanding-capabilities"></a>Descripción de funcionalidades

>Se aplica a: Windows Server 2019

En este tema se define el concepto de capacidades de System Insights y se presentan las capacidades predeterminadas disponibles en Windows Server 2019.

En este tema también se describen los orígenes de datos, las escalas de tiempo de predicción y los Estados de predicción usados para las capacidades predeterminadas.

## <a name="capability-overview"></a>Introducción a las capacidades
Una funcionalidad de System Insights es un modelo de aprendizaje automático o de estadísticas que analiza los datos del sistema para ayudarle a obtener un mayor conocimiento sobre el funcionamiento de la implementación. System Insights incorpora un conjunto inicial de capacidades predeterminadas y permite agregar nuevas capacidades de forma dinámica, sin necesidad de actualizar el sistema operativo.

>[!NOTE]
>[Aquí](adding-and-developing-capabilities.md)encontrará documentación detallada en la que se explica cómo crear, agregar y actualizar capacidades, y [el documento capacidades de administración](managing-capabilities.md) proporciona más información de alto nivel sobre esta funcionalidad.

Además, cada funcionalidad se ejecuta localmente en una instancia de Windows Server y cada capacidad se puede administrar individualmente.

### <a name="capability-outputs"></a>Salidas de funcionalidad
Cuando se invoca una funcionalidad, proporciona una salida para ayudar a explicar el resultado de su análisis o predicción. Cada salida debe contener un **Estado** y una **Descripción del estado** para describir la predicción, y cada resultado puede contener opcionalmente datos específicos de la funcionalidad asociados a la predicción. La **Descripción del estado** ayuda a proporcionar una explicación contextual del **Estado**y la capacidad informa de un estado **correcto**, de **ADVERTENCIA**o **crítico** . Además, una capacidad puede usar un **error** o el estado **None** si no se realizó ninguna predicción. Juntos, estos son los Estados de funcionalidad y sus significados básicos:

- **Correcto** : todo es correcto.
- **ADVERTENCIA** : no se requiere atención inmediata, pero debe echar un vistazo.
- **Crítico** : debe echar un vistazo pronto.
- **Error** : un problema desconocido hizo que se produjera un error en la funcionalidad.
- **Ninguno** : no se ha realizado ninguna predicción. Esto puede deberse a la falta de datos o a cualquier otro motivo específico de la funcionalidad para no realizar una predicción.

Además, los datos específicos de la funcionalidad contenidos en el resultado se colocarán en un archivo JSON accesible para el usuario y la ruta de acceso del archivo se [puede encontrar con PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="default-capabilities"></a>Capacidades predeterminadas
En Windows Server 2019, System Insights presenta cuatro funciones predeterminadas centradas en la previsión de la capacidad:

- **Previsión** de la capacidad de CPU: pronostica el uso de CPU.
- **Previsión** de la capacidad de red: pronostica el uso de red para cada adaptador de red.
- **Previsión de consumo de almacenamiento total** : pronostica el consumo total de almacenamiento en todas las unidades locales.
- **Previsión del consumo de volumen** : pronostica el consumo de almacenamiento de cada volumen.

Cada capacidad analiza los datos históricos anteriores para predecir el uso futuro, y **todas las capacidades de previsión están diseñadas para pronosticar tendencias a largo plazo en lugar de un comportamiento a corto plazo**, ayudando a los administradores a aprovisionar correctamente el hardware y ajustar sus cargas de trabajo para evitar la contención de recursos en el futuro. Dado que estas funcionalidades se centran en el uso a largo plazo, estas funciones analizan los datos diarios.

### <a name="forecasting-model"></a>Modelo de previsión
Las funciones predeterminadas usan un modelo de previsión para predecir el uso futuro y, para cada predicción, el modelo se entrena localmente en los datos de la máquina. Este modelo se ha diseñado para ayudar a detectar tendencias a largo plazo y el reciclaje en cada instancia de Windows Server permite que la capacidad se adapte al comportamiento específico y a los matices del uso de cada máquina.

>[!NOTE]
>Determinar el tipo de modelo que se usa para probar muchos modelos mediante un conjunto de elementos que contiene decenas de miles de máquinas. Después de analizar y ajustar estos modelos, decidimos usar un modelo de predicción de regresión automática, ya que genera predicciones de alta precisión y visualmente intuitivas, a la vez que no requiere demasiado tiempo para entrenar. Sin embargo, este modelo requiere tres semanas de datos de entrenamiento, por lo que cada función usa una tendencia lineal básica hasta que haya tres semanas de datos disponibles.

### <a name="forecasting-timelines"></a>Previsión de escalas de tiempo
Las funciones predeterminadas pronostican un determinado número de días en el futuro en función del número de días que se han recopilado los datos. En la tabla siguiente se muestran las escalas de tiempo de predicción de estas capacidades:

| Tamaño de los datos de entrada | Predecir duración |
| --------------- | --------------- |
| 0-5 días | No se realiza ninguna predicción. |
| 6-180 días | 1/3 * tamaño de los datos de entrada |
| 180-365 días | 60 días |

### <a name="forecasting-data"></a>Previsión de datos
Cada capacidad analiza los datos diarios para pronosticar el uso futuro. Sin embargo, la CPU, las redes e incluso el uso de almacenamiento pueden cambiar con frecuencia a lo largo del día, ajustando dinámicamente a las cargas de trabajo en el equipo. Dado que el uso no es constante a lo largo del día, es importante representar correctamente el uso diario en un único punto de datos. En la tabla siguiente se detallan los puntos de datos específicos y cómo se procesan los datos:


| Nombre de la prestación | Orígenes de datos | Lógica de filtrado |
| --------------- | -------------- | ---------------- |
 Previsión del consumo de volumen          | Tamaño del volumen                    | Uso diario máximo
 Previsión total del consumo de almacenamiento   | Suma de los tamaños de volumen, suma de los tamaños de disco              | Uso diario máximo
 Previsión de la capacidad de CPU                | % de tiempo de procesador  | Promedio máximo de 2 horas por día
 Previsión de la capacidad de red         | Bytes totales por segundo         | Promedio máximo de 2 horas por día

Al evaluar la lógica de filtrado anterior, es importante tener en cuenta que cada funcionalidad intenta informar a los administradores cuando el uso futuro va a superar la capacidad disponible, aunque momentáneamente la CPU alcanza el 100% de uso, puede que el uso de la CPU no haya causado una degradación significativa del rendimiento o la contención de recursos. En cuanto a CPU y redes, debe haber un uso elevado sostenido en lugar de picos momentáneos. Sin embargo, si se calcula el promedio del uso de la CPU y de la red a lo largo del día, se perdería información de uso importante, ya que algunas horas de uso intensivo de la CPU o de la red pueden afectar de forma significativa al rendimiento de las cargas de trabajo críticas. La media máxima de 2 horas durante cada día evita estos extremos y todavía genera datos significativos para cada capacidad de análisis.

En el caso del uso de almacenamiento total y de volumen, sin embargo, el uso de almacenamiento no puede superar la capacidad disponible, incluso en breve, por lo que se usa el uso diario máximo para estas funcionalidades.

### <a name="forecasting-statuses"></a>Estados de previsión
Todas las funcionalidades de System Insights deben generar un estado asociado a cada predicción. Cada capacidad predeterminada utiliza la lógica siguiente para definir cada estado de predicción:
- **OK**: la previsión no supera la capacidad disponible.
- **ADVERTENCIA**: la previsión supera la capacidad disponible en los próximos 30 días.
- **Crítico**: la previsión supera la capacidad disponible en los próximos 7 días.
- **Error**: se produjo un error inesperado en la funcionalidad.
- **Ninguno**: no hay suficientes datos para hacer una predicción. Esto puede deberse a una falta de datos o a que no se ha incomunicado ningún dato recientemente.

>[!NOTE]
>Si una capacidad se pronostica en varias instancias, como varios volúmenes o adaptadores de red, el estado refleja el estado más grave en todas las instancias. Los Estados individuales de cada volumen o adaptador de red están visibles en el centro de administración de Windows o en los datos contenidos en la salida de cada funcionalidad. Para obtener instrucciones sobre cómo analizar la salida JSON de las funciones predeterminadas, visite [este blog](https://aka.ms/systeminsights-mitigationscripts).


## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)
