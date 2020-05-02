---
title: Manage-BDE setidentifier
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dec19003f9a3421cfd2c73ba892f68aebfb8e133
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724092"
---
# <a name="manage-bde-setidentifier"></a>Manage-BDE: setidentifier



Establece el campo Identificador de unidad de la unidad en el valor especificado en la configuración **proporcionar los identificadores únicos de la organización** Directiva de grupo.

## <a name="syntax"></a>Sintaxis

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para ilustrar el uso del comando **-setidentifier** para establecer el campo de identificador de unidad BitLocker para C.
```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)
-   [Uso de agentes de recuperación de datos con BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)