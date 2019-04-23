---
title: Administración de funcionalidades
description: Sistema Insights expone una variedad de opciones que se pueden configurar para cada capacidad, y se puede ajustar esta configuración a las necesidades específicas de la implementación de dirección. Este tema describe cómo administrar las distintas configuraciones para cada capacidad a través de Windows Admin Center o PowerShell, que proporciona ejemplos básicos de PowerShell y Windows Admin Center capturas de pantalla para mostrar cómo ajustar estos valores.
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868026"
---
# <a name="managing-capabilities"></a>Administración de funcionalidades

>Se aplica a: Windows Server 2019

En Windows Server 2019, sistema Insights expone una variedad de opciones que se pueden configurar para cada capacidad y se puede ajustar esta configuración a las necesidades específicas de la implementación de dirección. Este tema describe cómo administrar las distintas configuraciones para cada capacidad a través de Windows Admin Center o PowerShell, que proporciona ejemplos básicos de PowerShell y Windows Admin Center capturas de pantalla para mostrar cómo ajustar estos valores. 

>[!TIP]
>También puede usar estos breves vídeos que le ayudarán a empezar a trabajar y con confianza administrar información del sistema: [Introducción a información del sistema en 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Aunque en esta sección se proporciona ejemplos de PowerShell, puede usar el [documentación del sistema de PowerShell de Insights](https://aka.ms/systeminsightspowershell) para ver todos los conjuntos de cmdlets, parámetros y parámetro dentro de la información del sistema. 

## <a name="viewing-capabilities"></a>Capacidades de visualización

Para empezar a trabajar, puede enumerar todas las funcionalidades disponibles mediante el **Get InsightsCapability** cmdlet: 

```PowerShell
Get-InsightsCapability
``` 
Estas capacidades también están visibles en la extensión de la información del sistema:

![Página de información general de información del sistema que enumerar las funcionalidades disponibles](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Habilitación y deshabilitación de una capacidad
Cada capacidad se puede habilitar o deshabilitar. Deshabilitar una funcionalidad impide que esa capacidad que se invoca y para las capacidades no predeterminado, deshabilitar una función detiene toda la recopilación de datos para esa funcionalidad. De forma predeterminada, todas las funcionalidades están habilitadas y puede comprobar el estado de una capacidad mediante la **Get InsightsCapability** cmdlet. 

Para habilitar o deshabilitar una funcionalidad, use el **Enable InsightsCapability** y **Disable InsightsCapability** cmdlets:

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
También se puede activar o desactivar esta configuración seleccionada una funcionalidad en Windows Admin Center al hacer clic en el **habilitar** o **deshabilitar** botones.

### <a name="invoking-a-capability"></a>Invocar una función
Invocar una funcionalidad inmediatamente ejecuta la capacidad de recuperar una predicción y los administradores pueden invocar una función de cualquier momento haciendo clic en el **Invoke** botón en Windows Admin Center o mediante el  **InsightsCapability invocar** cmdlet:

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Para asegurarse de invocar una función no entra en conflicto con las operaciones críticas en el equipo, puede programar las predicciones en off horario.

## <a name="retrieving-capability-results"></a>Al recuperar los resultados de la capacidad
Una vez que se ha invocado una funcionalidad, los resultados más recientes son visibles utilizando **Get InsightsCapability** o **Get InsightsCapabilityResult**. Estos cmdlets de salida más reciente **estado** y **descripción del estado** de cada función, que describen el resultado de cada predicción. El **estado** y **descripción del estado** campos se describen en la [documento de descripción capacidades](understanding-capabilities.md). 

Además, puede usar el **Get InsightsCapabilityResult** cmdlet para ver los últimos resultados de predicción de 30 y recuperar los datos asociados con la predicción: 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
La extensión del sistema Insights automáticamente muestra el historial de predicción y analiza los resultados del resultado JSON, lo que le ofrece un gráfico intuitivo de alta fidelidad de cada previsión:

![Página de capacidad solo que muestra un gráfico de previsión y el historial de predicción](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Utilice el registro de eventos para recuperar resultados de capacidad
Información del sistema se registra un evento cada vez que termine de una capacidad de una predicción. Estos eventos están visibles en el **Microsoft-Windows-System-Insights/Admin** información detallada del sistema y canal publica un identificador de evento diferente para cada estado:   

| Estado de predicción | Id. de evento |
| --------------- | --------------- |
| Aceptar | 151 |
| Advertencia | 148 |
| Crítico | 150 |
| Error | 149 |
| Ninguno | 132 |

>[!TIP]
>Use [Azure Monitor](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar estos eventos y ver resultados de predicción en un grupo de máquinas.


## <a name="setting-a-capability-schedule"></a>Establecer una programación de capacidad
Además de las predicciones y a petición, puede configurar periódicas predicciones para cada capacidad para que la capacidad especificada se invoca automáticamente según una programación predefinida. Use la **Get InsightsCapabilitySchedule** cmdlet para ver las programaciones de capacidad: 

>[!TIP]
>Use el operador de canalización de PowerShell para ver información de todas las funcionalidades de devuelto por la **Get InsightsCapability** cmdlet.

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Predicciones periódicas están habilitadas de forma predeterminada aunque puede deshabilitar en cualquier momento mediante el **Enable InsightsCapabilitySchedule** y **Disable InsightsCapabilitySchedule** cmdlets:

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Cada capacidad predeterminada está programada para ejecutarse diariamente a las 3 a.m. Sin embargo, puede crear programaciones personalizadas para cada capacidad, y el sistema Insights admite una variedad de tipos de programación, que se pueden configurar mediante el **conjunto InsightsCapabilitySchedule** cmdlet: 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Dado que las características predeterminadas analizan datos diarios, se recomienda utilizar programaciones diarias para estas capacidades. Más información sobre las características predeterminadas [aquí](understanding-capabilities.md).

También puede usar Windows Admin Center para ver y establecer las programaciones para cada capacidad haciendo **configuración**. La programación actual se muestra en el **programación** ficha y, puede usar las herramientas de GUI para crear una nueva programación:

![Programación de configuración de la página que muestra actual](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Creación de las acciones de corrección
Información del sistema le permite dar comienzo a scripts de corrección personalizados en función del resultado de una función. Para cada capacidad, puede configurar un script de PowerShell personalizado para cada estado de predicción, permitiendo a los administradores realizar una acción correctora automáticamente, en lugar de requerir la intervención manual. 

Las acciones de corrección de ejemplo incluyen ejecución Liberador de espacio en disco, extender un volumen, ejecuta la desduplicación, en vivo migra máquinas virtuales y configurar Azure File Sync.

Puede ver las acciones para cada capacidad mediante la **Get InsightsCapabilityAction** cmdlet:

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Puede crear nuevas acciones o eliminar acciones existentes utilizando el **conjunto InsightsCapabilityAction** y **Remove-InsightsCapabilityAction** cmdlets. Cada acción se ejecuta con credenciales que se especifican en el **ActionCredential** parámetro.

>[!NOTE]
>En la versión inicial de la información del sistema, debe especificar scripts de corrección fuera de los directorios de usuario. Esto se corregirá en una próxima versión.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

También puede usar Windows Admin Center para establecer las acciones de corrección mediante el **acciones** pestaña dentro de la **configuración** página:

![Página de configuración en el usuario puede especificar las acciones de corrección](media/actions-page-contoso.png)


## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de descripción](understanding-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)