---
title: 'administrar: estado de BDE'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c8248333944b030dc8868ba4408024727a08c3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839848"
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

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<unidad >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-protectionaserrorlevel|Hace que la herramienta de línea de comandos Manage-BDE envíe el código de retorno 0 cuando el volumen está protegido y 1 cuando el volumen está desprotegido. se usa normalmente para los scripts por lotes para determinar si una unidad está protegida por BitLocker. También puede usar **-p** como una versión abreviada de este comando.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|Nombre del \<>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el ejemplo siguiente se muestra el uso del comando **-status** para mostrar el estado de la unidad C.
```
manage-bde –status C:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)