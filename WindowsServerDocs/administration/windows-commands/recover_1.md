---
title: recuperar (DiskPart)
description: Artículo de referencia para el comando de recuperación de DiskPart, que actualiza el estado de todos los discos de un grupo de discos, intenta recuperar discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y los volúmenes RAID-5 que tienen datos obsoletos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03b996db5d8c6d270a35bf1273570e3757258658
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519684"
---
# <a name="recover-diskpart"></a>recuperar (DiskPart)

Actualiza el estado de todos los discos de un grupo de discos, intenta recuperar discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y los volúmenes RAID-5 que tienen datos obsoletos. Este comando funciona en discos con errores o con errores. También funciona en los volúmenes con errores, con errores o con errores de redundancia.

Este comando funciona en grupos de discos dinámicos. Si este comando se usa en un grupo con un disco básico, no devolverá un error, pero no se realizará ninguna acción.

> [!NOTE]
> Para que esta operación se realice correctamente, debe seleccionarse un disco que forme parte de un grupo de discos. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
recover [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para recuperar el grupo de discos que contiene el disco que tiene el foco, escriba:

```
recover
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
