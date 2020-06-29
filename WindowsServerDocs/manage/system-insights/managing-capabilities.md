---
title: Administración de funcionalidades
description: System Insights expone una variedad de opciones de configuración que se pueden configurar para cada capacidad, y estas opciones se pueden optimizar para satisfacer las necesidades específicas de la implementación. En este tema se describe cómo administrar las distintas opciones de configuración de cada funcionalidad a través del centro de administración de Windows o PowerShell, y se proporcionan ejemplos básicos de PowerShell y capturas de pantallas del centro de administración de Windows para mostrar cómo ajustar esta configuración.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 66745440094ccf55b774727320d59074139a7f33
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471787"
---
# <a name="managing-capabilities"></a>Administración de funcionalidades

>Se aplica a: Windows Server 2019

En Windows Server 2019, System Insights expone una variedad de opciones de configuración que se pueden configurar para cada capacidad, y estas opciones se pueden optimizar para satisfacer las necesidades específicas de la implementación. En este tema se describe cómo administrar las distintas opciones de configuración de cada funcionalidad a través del centro de administración de Windows o PowerShell, y se proporcionan ejemplos básicos de PowerShell y capturas de pantallas del centro de administración de Windows para mostrar cómo ajustar esta configuración.

>[!TIP]
>También puede usar estos breves vídeos para ayudarle a empezar a trabajar y administrar con confianza System Insights: [Introducción a System Insights en 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Aunque en esta sección se proporcionan ejemplos de PowerShell, puede usar la [documentación de PowerShell de System Insights](https://aka.ms/systeminsightspowershell) para ver todos los cmdlets, parámetros y conjuntos de parámetros de System Insights.

## <a name="viewing-capabilities"></a>Funciones de visualización

Para empezar, puede enumerar todas las funcionalidades disponibles mediante el cmdlet **Get-InsightsCapability** :

```PowerShell
Get-InsightsCapability
```
Estas funcionalidades también están visibles en la extensión de System Insights:

![Página de información general de System Insights lista de funcionalidades disponibles](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Habilitación y deshabilitación de una funcionalidad
Cada capacidad puede estar habilitada o deshabilitada. Deshabilitar una funcionalidad impide que se invoque esa funcionalidad y, para las capacidades no predeterminadas, la deshabilitación de una funcionalidad detiene toda la recopilación de datos de esa capacidad. De forma predeterminada, todas las funcionalidades están habilitadas y puede comprobar el estado de una funcionalidad con el cmdlet **Get-InsightsCapability** .

Para habilitar o deshabilitar una funcionalidad, use los cmdlets **enable-InsightsCapability** y **Disable-InsightsCapability** :

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
```
Esta configuración también se puede alternar seleccionando una capacidad en el centro de administración de Windows al hacer clic en los botones **Habilitar** o **deshabilitar** .

### <a name="invoking-a-capability"></a>Invocar una funcionalidad
Al invocar una funcionalidad, se ejecuta inmediatamente la funcionalidad para recuperar una predicción, y los administradores pueden invocar una funcionalidad en cualquier momento haciendo clic en el botón **invocar** en el centro de administración de Windows o mediante el cmdlet **Invoke-InsightsCapability** :

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Para asegurarse de que la invocación de una funcionalidad no entra en conflicto con las operaciones críticas en su equipo, considere la posibilidad de programar predicciones fuera del horario comercial.

## <a name="retrieving-capability-results"></a>Recuperando resultados de funcionalidad
Una vez que se ha invocado una funcionalidad, los resultados más recientes son visibles mediante **Get-InsightsCapability** o **Get-InsightsCapabilityResult**. Estos cmdlets generan el **Estado** y la **Descripción de estado** más recientes de cada capacidad, que describen el resultado de cada predicción. Los campos de **Estado** y **Descripción del estado** se describen con más detalle en el [documento información sobre capacidades](understanding-capabilities.md).

Además, puede usar el cmdlet **Get-InsightsCapabilityResult** para ver los últimos 30 resultados de predicción y recuperar los datos asociados a la predicción:

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
La extensión System Insights muestra automáticamente el historial de predicción y analiza los resultados del resultado JSON, lo que le ofrece un gráfico intuitivo y de alta fidelidad de cada previsión:

![Página de funcionalidad única que muestra un gráfico de previsión y el historial de predicción](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Usar el registro de eventos para recuperar los resultados de funcionalidad
System Insights registra un evento cada vez que una funcionalidad finaliza una predicción. Estos eventos están visibles en el canal **Microsoft-Windows-System-Insights/admin** y System Insights publica un identificador de evento diferente para cada Estado:

| Estado de predicción | Id. de evento |
| --------------- | --------------- |
| Aceptar | 151 |
| Advertencia | 148 |
| Crítico | 150 |
| Error | 149 |
| None | 132 |

>[!TIP]
>Use [Azure monitor](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar estos eventos y ver los resultados de predicción en un grupo de máquinas.


## <a name="setting-a-capability-schedule"></a>Establecer una programación de capacidad
Además de las predicciones a petición, puede configurar predicciones periódicas para cada funcionalidad, de modo que la capacidad especificada se invoque automáticamente según una programación predefinida. Use el cmdlet **Get-InsightsCapabilitySchedule** para ver las programaciones de capacidad:

>[!TIP]
>Use el operador de canalización de PowerShell para ver la información de todas las funcionalidades devueltas por el cmdlet **Get-InsightsCapability** .

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Las predicciones periódicas están habilitadas de forma predeterminada aunque se pueden deshabilitar en cualquier momento mediante los cmdlets **enable-InsightsCapabilitySchedule** y **Disable-InsightsCapabilitySchedule** :

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Cada función predeterminada está programada para ejecutarse cada día en 3 a.m.. Sin embargo, puede crear programaciones personalizadas para cada capacidad y System Insights admite una variedad de tipos de programación, que se pueden configurar mediante el cmdlet **set-InsightsCapabilitySchedule** :

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30
```
>[!NOTE]
>Dado que las funciones predeterminadas analizan los datos diarios, se recomienda usar las programaciones diarias para estas capacidades. Obtenga más información sobre las funcionalidades predeterminadas [aquí](understanding-capabilities.md).

También puede usar el centro de administración de Windows para ver y establecer programaciones para cada funcionalidad haciendo clic en **configuración**. La programación actual se muestra en la pestaña **programación** y puede usar las herramientas de la GUI para crear una nueva programación:

![Página de configuración que muestra la programación actual](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Crear acciones correctivas
System Insights le permite iniciar scripts de corrección personalizados basados en el resultado de una capacidad. Para cada capacidad, puede configurar un script de PowerShell personalizado para cada estado de predicción, lo que permite a los administradores tomar medidas correctivas automáticamente, en lugar de requerir la intervención manual.

Las acciones correctivas de ejemplo incluyen la ejecución del liberador de espacio en disco, la extensión de un volumen, la ejecución de la desduplicación, la migración en vivo de máquinas virtuales y la configuración de Azure File Sync.

Puede ver las acciones de cada funcionalidad mediante el cmdlet **Get-InsightsCapabilityAction** :

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Puede crear nuevas acciones o eliminar acciones existentes mediante los cmdlets **set-InsightsCapabilityAction** y **Remove-InsightsCapabilityAction** . Cada acción se ejecuta con credenciales que se especifican en el parámetro **ActionCredential** .

>[!NOTE]
>En la versión inicial de System Insights, debe especificar scripts de corrección fuera de los directorios de usuario. Esto se corregirá en una próxima versión.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

También puede usar el centro de administración de Windows para establecer acciones correctivas mediante la pestaña **acciones** de la página **configuración** :

![Página de configuración en la que el usuario puede especificar acciones correctivas](media/actions-page-contoso.png)


## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Descripción de funcionalidades](understanding-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)