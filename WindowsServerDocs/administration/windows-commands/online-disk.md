---
title: online disk
description: Artículo de referencia para el comando de disco en línea, que desconecta el disco sin conexión al estado en línea.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7313f5b5b8c0594e0706555a203248d760028806
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933552"
---
# <a name="online-disk"></a>online disk

Desconecta el disco sin conexión al estado en línea. En el caso de los discos básicos, este comando intenta poner en línea el disco seleccionado y todos los volúmenes del disco. En el caso de los discos dinámicos, este comando intenta poner en conexión todos los discos que no están marcados como externos en el equipo local. También intenta poner en línea todos los volúmenes del conjunto de discos dinámicos.

Si se conecta un disco dinámico de un grupo de discos y es el único disco del grupo, se vuelve a crear el grupo original y el disco se mueve a ese grupo. Si hay otros discos en el grupo y están en línea, el disco simplemente se vuelve a agregar al grupo. Si el grupo de un disco seleccionado contiene volúmenes reflejados o RAID-5, este comando también vuelve a sincronizar estos volúmenes.

> [!NOTE]
> Se debe seleccionar un disco para que el comando de **disco en línea** se ejecute correctamente. Use el comando [Seleccionar disco](select-disk.md) para seleccionar un disco y desplazar el foco a él.

> [!IMPORTANT]
> Este comando producirá un error si se usa en un disco de solo lectura.

## <a name="syntax"></a>Sintaxis

```
online disk [noerr]
```

### <a name="parameters"></a>Parámetros

Para obtener instrucciones sobre el uso de este comando, consulte [reactivación de un disco dinámico que falta o que está sin conexión](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732026(v=ws.11)).

| Parámetro | Descripción |
|--|--|
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para poner en línea el disco con el foco, escriba:

```
online disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
