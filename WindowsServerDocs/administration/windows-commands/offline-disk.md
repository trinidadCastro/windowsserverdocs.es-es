---
title: offline disk
description: Artículo de referencia del comando disco sin conexión, que toma el disco en línea con el foco en el estado sin conexión.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8a746e09e5756eec1890d1715a88e60696cd49
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936756"
---
# <a name="offline-disk"></a>offline disk

Toma el disco en línea con el foco en el estado sin conexión. Si se desconecta un disco dinámico de un grupo de discos, el estado del disco cambia a **ausente** y el grupo muestra un disco sin conexión. El disco que falta se mueve al grupo no válido. Si el disco dinámico es el último disco del grupo, el estado del disco cambia a **sin conexión**y se quita el grupo vacío.

> [!NOTE]
> Se debe seleccionar un disco para que el comando de **disco sin conexión** se realice correctamente. Use el comando [Seleccionar disco](select-disk.md) para seleccionar un disco y desplazar el foco a él.
>
> Este comando también funciona en discos en el modo SAN online cambiando el modo SAN a sin conexión.

## <a name="syntax"></a>Sintaxis

```
offline disk [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:

```
offline disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
