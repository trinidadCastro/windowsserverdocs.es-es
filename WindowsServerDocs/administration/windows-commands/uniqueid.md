---
title: uniqueid
description: Artículo de referencia de UniqueID, que muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco que tiene el foco.
ms.topic: reference
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73721316747ffd0380f178622d37dc4fdbefc2fb
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156297"
---
# <a name="uniqueid"></a>uniqueid

Muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco básico o dinámico con el foco. Se debe seleccionar un disco básico o dinámico para que esta operación se realice correctamente. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| ID. =`{<dword> | <GUID>}` | En el caso de los discos MBR, este parámetro especifica un valor de 4 bytes (DWORD) en formato hexadecimal para la firma. En el caso de los discos GPT, este parámetro especifica un GUID para el identificador. |
| noerr | Sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para mostrar la firma del disco MBR con el foco, escriba:

```
uniqueid disk
```

Para establecer la firma del disco MBR con el foco en el valor DWORD *5f1b2c36*, escriba:

```
uniqueid disk id=5f1b2c36
```

Para establecer el identificador del disco GPT con el foco en el valor de GUID baf784e7-6bbd-4cfb-AAAC-e86c96e166ee, escriba:

```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Seleccionar disco (comando)](select-disk.md)
