---
title: Manage-BDE setidentifier
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d7c42e6176f7ea204f5616946197dc71649339
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373883"
---
# <a name="manage-bde-setidentifier"></a>Manage-BDE: setidentifier



Establece el campo Identificador de unidad de la unidad en el valor especificado en la configuración **proporcionar los identificadores únicos de la organización** Directiva de grupo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Drive >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra el uso del comando **-setidentifier** para establecer el campo de identificador de unidad BitLocker para C.
```
manage-bde –setidentifier C:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)
-   [Uso de agentes de recuperación de datos con BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)