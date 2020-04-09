---
title: reinicio de bdehdcfg
description: Comando comandos de Windows para el **reinicio de bdehdcfg**, que indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae6f8d31c09feddf8f994c28d34e4e1b08cc322
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851038"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: reinicio

Informa a la herramienta de línea de comandos Bdehdcfg de que el equipo debe reiniciarse una vez finalizada la preparación de la unidad. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

#### <a name="parameters"></a>Parámetros

Este comando no toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si otros usuarios han iniciado sesión en el equipo y no se ha especificado el comando **Quiet** , se mostrará un símbolo del sistema para confirmar que el equipo debe reiniciarse.

## <a name="examples"></a><a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **restart** .

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)