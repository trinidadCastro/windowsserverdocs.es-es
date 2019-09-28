---
title: 'administrar: estado de BDE'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 235db54ef2361c0e95c66b15a15be7f188fb74d9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373859"
---
# <a name="manage-bde-status"></a>Manage-BDE: estado



Proporciona la siguiente información acerca de todas las unidades del equipo; tanto si están protegidos con BitLocker como si no:
-   Tamaño
-   Versión de BitLocker
-   Estado de la conversión
-   Porcentaje cifrado
-   Encryption method
-   Estado de protección
-   Estado de bloqueo
-   Campo de identificación
-   Protectores de clave

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Drive >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-protectionaserrorlevel|Hace que la herramienta de línea de comandos Manage-BDE envíe el código de retorno 0 cuando el volumen está protegido y 1 cuando el volumen está desprotegido. se usa normalmente para los scripts por lotes para determinar si una unidad está protegida por BitLocker. También puede usar **-p** como una versión abreviada de este comando.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra el uso del comando **-status** para mostrar el estado de la unidad C.
```
manage-bde –status C:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)