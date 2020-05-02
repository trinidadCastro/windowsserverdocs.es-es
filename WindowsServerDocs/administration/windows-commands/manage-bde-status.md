---
title: 'administrar: estado de BDE'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1bf42da356d8326f459066fc168bbd38b7765b0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724085"
---
# <a name="manage-bde-status"></a>Manage-BDE: estado



Proporciona la siguiente información acerca de todas las unidades del equipo; tanto si están protegidos con BitLocker como si no:
-   Size
-   Versión de BitLocker
-   Estado de la conversión
-   Porcentaje cifrado
-   Encryption method
-   Estado de protección
-   Estado de bloqueo
-   Campo de identificación
-   Protectores de clave



## <a name="syntax"></a>Sintaxis

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|-protectionaserrorlevel|Hace que la herramienta de línea de comandos Manage-BDE envíe el código de retorno 0 cuando el volumen está protegido y 1 cuando el volumen está desprotegido. se usa normalmente para los scripts por lotes para determinar si una unidad está protegida por BitLocker. También puede usar **-p** como una versión abreviada de este comando.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Ilustra el uso del comando **-status** para mostrar el estado de la unidad C.
```
manage-bde –status C:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)