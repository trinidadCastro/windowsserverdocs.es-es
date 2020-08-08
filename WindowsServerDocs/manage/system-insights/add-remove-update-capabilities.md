---
title: Funcionalidades de adición, eliminación y actualización
description: System Insights le permite crear nuevas capacidades que aprovechan la funcionalidad de administración y recopilación de datos existente. Es importante que también tenga compatibilidad con la plataforma para administrar la adición, eliminación y actualización de estas funcionalidades. En este tema se describe la funcionalidad de alto nivel para agregar, quitar y actualizar funcionalidades en System Insights.
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 17d31b480e013cf0276041a88a86530448071ca5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958693"
---
# <a name="adding-removing-and-updating-capabilities"></a>Funcionalidades de adición, eliminación y actualización

>Se aplica a: Windows Server 2019

System Insights le permite crear nuevas capacidades que aprovechan la funcionalidad de administración y recopilación de datos existente. Sin embargo, una vez creadas estas funcionalidades, es igualmente importante que también tenga la compatibilidad con la plataforma para administrar la adición, eliminación y actualización de estas funcionalidades.

En este tema se describe la funcionalidad de alto nivel para agregar, quitar y actualizar funcionalidades en System Insights.

## <a name="adding-a-capability"></a>Agregar una funcionalidad
System Insights le permite agregar nuevas funcionalidades en cualquier momento mediante el cmdlet **Add-InsightsCapability** . **Add-InsightsCapability** requiere que se especifique un nombre de funcionalidad y la biblioteca de funciones. La biblioteca de funcionalidad contiene la descripción de la funcionalidad, los orígenes de datos y la lógica de predicción.

```PowerShell
Add-InsightsCapability -Name Sample capability -Library C:\SampleCapability.dll
```

Una vez agregada una funcionalidad a System Insights, puede invocar y administrar inmediatamente la funcionalidad mediante PowerShell o el centro de administración de Windows.

## <a name="updating-a-capability"></a>Actualizar una funcionalidad
System Insights también le permite actualizar una funcionalidad con el cmdlet **Update-InsightsCapability** .

```PowerShell
Update-InsightsCapability -Name Sample capability -Library C:\SampleCapabilityv2.dll
```

La actualización de una funcionalidad le permite especificar una nueva biblioteca de funciones, que le permite cambiar la descripción de la funcionalidad, los orígenes de datos y la lógica de predicción asociada a esa capacidad. Lo importante es que la actualización de una funcionalidad conserva toda la configuración e información histórica sobre esa capacidad, incluidas las programaciones personalizadas, las acciones y los resultados de predicción históricos.

## <a name="removing-a-capability"></a>Quitar una funcionalidad
También puede quitar funcionalidades de System Insights mediante el cmdlet **Remove-InsightsCapability** .

```PowerShell
Remove-InsightsCapability -Name Sample capability
```
>[!NOTE]
>No se pueden quitar las funciones predeterminadas de previsión.

Al quitar una funcionalidad, se elimina permanentemente la funcionalidad y toda la información asociada, incluida la programación, las acciones correctivas y los resultados de predicción anteriores.

>[!TIP]
>Considere la posibilidad de deshabilitar una funcionalidad en lugar de quitarla si le preocupa eliminar permanentemente toda la información asociada a la funcionalidad.

## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Descripción de funcionalidades](understanding-capabilities.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)