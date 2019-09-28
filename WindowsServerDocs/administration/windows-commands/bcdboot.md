---
title: bcdboot
description: 'Temas de comandos de Windows para **BCDboot** : configure rápidamente una partición del sistema o repare el entorno de arranque ubicado en la partición del sistema.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383439"
---
# <a name="bcdboot"></a>bcdboot



Permite configurar rápidamente una partición del sistema o reparar el entorno de arranque ubicado en la partición del sistema. La partición del sistema se configura mediante la copia de un conjunto simple de archivos datos de la configuración de arranque (BCD) (BCD) en una partición vacía existente.

Para obtener más información sobre BCDboot, incluida información sobre dónde encontrar BCDboot y ejemplos de cómo usar este comando, consulte el tema [Opciones de la línea de comandos de BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

## <a name="syntax"></a>Sintaxis

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Fuentes|Especifica la ubicación del directorio de Windows que se va a usar como origen para copiar los archivos del entorno de arranque.|
|/l|Especifica la configuración regional. La configuración regional predeterminada es Inglés de EE. UU.|
|/s|Especifica la letra de volumen de la partición del sistema. El valor predeterminado es la partición del sistema identificada por el firmware.|

## <a name="BKMK_examples"></a>Example

Para obtener más ejemplos de cómo usar este comando, consulte el tema Opciones de la [línea de comandos de BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)