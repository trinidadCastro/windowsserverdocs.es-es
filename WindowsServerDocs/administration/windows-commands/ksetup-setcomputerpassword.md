---
title: 'ksetup: setcomputerpassword'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cb0c2ee36ed85ddfb015a80e86198fe788f8474
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724581"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup: setcomputerpassword



Establece la contraseña del equipo local.

## <a name="syntax"></a>Sintaxis

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de contraseña|Usa la contraseña proporcionada para establecer la cuenta de equipo en el equipo local.</br>La contraseña solo se puede establecer mediante una cuenta con privilegios administrativos. La contraseña puede tener de 1 a 156 caracteres alfanuméricos o especiales.|

## <a name="remarks"></a>Observaciones

Este comando afecta solo a la cuenta de equipo.

Debe reiniciar el equipo para que el cambio de contraseña surta efecto.

La contraseña de la cuenta de equipo no se muestra en el registro o como salida del comando **ksetup** .

## <a name="examples"></a>Ejemplos

Cambie la contraseña de la cuenta de equipo en el equipo local de IPops897 a IPop $897!.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)