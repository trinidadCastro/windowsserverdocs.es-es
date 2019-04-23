---
title: Funcionalidades de adición, eliminación y actualización
description: Información del sistema le permite crear nuevas capacidades que aprovechan la funcionalidad y la recopilación de datos existente. Es importante que también tiene la compatibilidad de plataforma para administrar la adición, eliminación y las actualizaciones de estas capacidades. Este tema describe la funcionalidad de alto nivel para agregar, quitar y actualizar las capacidades de sistema de información.
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
ms.date: 7/31/2018
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880166"
---
# <a name="adding-removing-and-updating-capabilities"></a>Funcionalidades de adición, eliminación y actualización

>Se aplica a: Windows Server 2019

Información del sistema le permite crear nuevas capacidades que aprovechan la funcionalidad y la recopilación de datos existente. Sin embargo, una vez creadas estas capacidades, es igualmente importante que también tiene la compatibilidad de plataforma para administrar la adición, eliminación y las actualizaciones de estas capacidades. 

Este tema describe la funcionalidad de alto nivel para agregar, quitar y actualizar las capacidades de sistema de información. 

## <a name="adding-a-capability"></a>Agregar una funcionalidad
Sistema Insights le permite agregar nuevas capacidades en cualquier momento mediante el **agregar InsightsCapability** cmdlet. El **agregar InsightsCapability** requiere que especifique un nombre de función y la biblioteca de capacidad. La biblioteca de capacidad contiene la descripción de la capacidad, los orígenes de datos y lógica de predicción.

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

Una vez agregada una funcionalidad a la información del sistema, puede invocar y administrar la capacidad de usar PowerShell o Windows Admin Center inmediatamente. 

## <a name="updating-a-capability"></a>Una característica de actualización
Información del sistema también le permite actualizar una capacidad mediante la **actualización InsightsCapability** cmdlet.

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

Una característica de actualización le permite especificar una nueva biblioteca de funcionalidad, que le permite cambiar la descripción de la capacidad, los orígenes de datos y la lógica de predicción asociada con esa capacidad. Lo importante es que, una característica de actualización conserva toda la configuración y la información histórica sobre esa funcionalidad, incluidas las programaciones personalizadas, acciones y resultados de predicción histórica. 

## <a name="removing-a-capability"></a>Eliminación de una capacidad
También puede quitar funcionalidades en la información del sistema utilizando el **Remove-InsightsCapability** cmdlet. 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>No se puede quitar el valor predeterminado funciones de previsión.

Al quitar una funcionalidad permanentemente, elimina la capacidad y toda la información asociada, incluidos la programación, las acciones de corrección y últimos resultados de predicción. 

>[!TIP]
>Considere la posibilidad de deshabilitar una función en lugar de quitarlo si le preocupa eliminar permanentemente toda la información asociada con la capacidad. 

## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de descripción](understanding-capabilities.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)