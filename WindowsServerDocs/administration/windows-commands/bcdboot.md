---
title: bcdboot
description: 'Tema de los comandos de Windows para **bcdboot** : rápidamente configurar una partición del sistema o reparar el entorno de arranque que se encuentra en la partición del sistema.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825886"
---
# <a name="bcdboot"></a>bcdboot



Le permite configurar rápidamente una partición del sistema o para reparar el entorno de arranque que se encuentra en la partición del sistema. La partición del sistema se configura mediante la copia de un conjunto simple de archivos de datos de configuración de arranque (BCD) en una partición vacía existente.

Para obtener más información acerca de BCDboot, incluida la información sobre dónde encontrar BCDboot y ejemplos de cómo utilizar este comando, consulte el [opciones de línea de comandos de BCDboot](https://technet.microsoft.com/library/hh824874.aspx) tema.

## <a name="syntax"></a>Sintaxis

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Código fuente|Especifica la ubicación del directorio Windows para usar como origen para copiar archivos del entorno de arranque.|
|/l|Especifica la configuración regional. La configuración regional predeterminada es inglés de Estados Unidos.|
|/s|Especifica la letra de volumen de la partición del sistema. El valor predeterminado es la partición del sistema identificada por el firmware.|

## <a name="BKMK_examples"></a>Ejemplos

Para obtener más ejemplos de cómo utilizar este comando, consulte el [opciones de línea de comandos de BCDboot](https://technet.microsoft.com/library/hh824874.aspx) tema.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)