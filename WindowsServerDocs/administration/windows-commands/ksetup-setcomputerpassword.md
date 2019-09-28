---
title: 'ksetup: setcomputerpassword'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1d3742476385eb770c9cb5c798c1f6ab27c74f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374943"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup: setcomputerpassword



Establece la contraseña del equipo local. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Password >|Usa la contraseña proporcionada para establecer la cuenta de equipo en el equipo local.</br>La contraseña solo se puede establecer mediante una cuenta con privilegios administrativos. La contraseña puede tener de 1 a 156 caracteres alfanuméricos o especiales.|

## <a name="remarks"></a>Comentarios

Este comando afecta solo a la cuenta de equipo.

Debe reiniciar el equipo para que el cambio de contraseña surta efecto.

La contraseña de la cuenta de equipo no se muestra en el registro o como salida del comando **ksetup** .

## <a name="BKMK_Examples"></a>Example

Cambie la contraseña de la cuenta de equipo en el equipo local de IPops897 a IPop $897!.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)