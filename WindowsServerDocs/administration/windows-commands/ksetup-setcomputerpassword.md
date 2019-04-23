---
title: ksetup:setcomputerpassword
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831546"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



Establece la contraseña para el equipo local. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Contraseña >|Utiliza la contraseña proporcionada para establecer la cuenta de equipo en el equipo local.</br>La contraseña solo puede establecerse mediante el uso de una cuenta con privilegios administrativos. La contraseña puede estar entre 1 y 156 caracteres alfanuméricos o especiales.|

## <a name="remarks"></a>Comentarios

Este comando afecta solo la cuenta de equipo.

Debe reiniciar el equipo para que surta efecto el cambio de contraseña.

La contraseña de cuenta de equipo no se muestra en el registro o como resultado de la **ksetup** comando.

## <a name="BKMK_Examples"></a>Ejemplos

Cambiar la contraseña de cuenta de equipo en el equipo local de IPops897 a IPop$ 897!.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)