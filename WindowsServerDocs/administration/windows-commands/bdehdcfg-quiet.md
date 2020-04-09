---
title: bdehdcfg silencioso
description: Tema de comandos de Windows para **bdehdcfg Quiet**, que indica a bdehdcfg que no muestre todas las acciones y errores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9c24d8861476e6c1578af8245236d699b6ef6db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851048"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: Quiet

Informa a la herramienta de línea de comandos Bdehdcfg de que no se mostrarán todas las acciones y los errores en la interfaz de la línea de comandos. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

#### <a name="parameters"></a>Parámetros

Este comando no toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si se muestran indicaciones Yes/No (Y/N) durante la preparación de la unidad, se supondrá la respuesta "Yes". Para ver los errores que se produzcan durante la preparación de la unidad, revise el registro de eventos del sistema en el proveedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="examples"></a><a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **Quiet** .

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)