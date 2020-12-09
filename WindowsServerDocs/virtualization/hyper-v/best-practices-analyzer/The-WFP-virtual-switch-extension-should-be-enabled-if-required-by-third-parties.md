---
title: La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
ms.date: 8/16/2016
ms.openlocfilehash: 00312413c8da02ce5221767667ecd941f0095776
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865164"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*La extensión de conmutador virtual de la plataforma de filtrado de Windows (WFP) está deshabilitada.*

## <a name="impact"></a>**Impacto**
*Es posible que algunas extensiones de conmutador virtual de terceros no funcionen correctamente en los siguientes conmutadores virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Solución**
*Use el cmdlet de Windows PowerShell, enable-VMSwitchExtension, para habilitar la plataforma de filtrado de Windows si es necesaria para las extensiones de terceros.*

### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar la plataforma de filtrado de Windows mediante Windows PowerShell

1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Ejecute este comando después de reemplazar external por el nombre del conmutador externo:

```
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform
```

## <a name="see-also"></a>Vea también
[Enable-VMSwitchExtension](/powershell/module/hyper-v/enable-vmswitchextension)
