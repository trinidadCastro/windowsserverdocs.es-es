---
title: mountvol
description: Artículo de referencia del comando mountvol, que crea, elimina o enumera un punto de montaje de volumen.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1617149fac677069d97b5b7c1353e85b4e1fea14
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936327"
---
# <a name="mountvol"></a>mountvol

Crea, elimina o enumera un punto de montaje de volumen. También puede vincular volúmenes sin necesidad de una letra de unidad.

## <a name="syntax"></a>Sintaxis

```
mountvol [<drive>:]<path volumename>
mountvol [<drive>:]<path> /d
mountvol [<drive>:]<path> /l
mountvol [<drive>:]<path> /p
mountvol /r
mountvol [/n|/e]
mountvol <drive>: /s
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[<drive>:]<path>` | Especifica el directorio NTFS existente donde residirá el punto de montaje. |
| `<volumename>` | Especifica el nombre del volumen que es el destino del punto de montaje. El nombre del volumen utiliza la sintaxis siguiente, donde *GUID* es un identificador único global: `\\?\volume\{GUID}\` . Los corchetes `{ }` son obligatorios. |
| /d | Quita el punto de montaje del volumen de la carpeta especificada. |
| /l | Muestra el nombre del volumen montado para la carpeta especificada. |
| /p | Quita el punto de montaje del volumen del directorio especificado, desmonta el volumen básico y desconecta el volumen básico, lo que hace que sea desmontable. Si otros procesos están usando el volumen, **Mountvol** cierra los identificadores abiertos antes de desmontar el volumen. |
| /r | Quita los directorios de punto de montaje de volumen y la configuración del registro para los volúmenes que ya no están en el sistema, evitando que se monten automáticamente y se les proporcionen los puntos de montaje de volumen anteriores cuando se vuelvan a agregar al sistema. |
| /n | Deshabilita el montaje automático de nuevos volúmenes básicos. Los nuevos volúmenes no se montan automáticamente cuando se agregan al sistema. |
| /e | Vuelve a habilitar el montaje automático de nuevos volúmenes básicos. |
| /s | Monta la partición del sistema EFI en la unidad especificada. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

- Si desmonta el volumen mientras usa el parámetro **/p** , la lista de volúmenes mostrará el volumen como no montado hasta que se cree un punto de montaje de volumen.

- Si el volumen tiene más de un punto de montaje, use **/d** para quitar los puntos de montaje adicionales antes de usar **/p**. Puede volver a hacer que se pueda montar el volumen básico si asigna un punto de montaje de volumen.

- Si necesita expandir el espacio de volumen sin volver a formatear o reemplazar una unidad de disco duro, puede Agregar una ruta de acceso de montaje a otro volumen. La ventaja de usar un volumen con varias rutas de acceso de montaje es que puede acceder a todos los volúmenes locales mediante una sola letra de unidad (como `C:` ). No es necesario recordar qué volumen corresponde a la letra de unidad, aunque todavía puede montar volúmenes locales y asignarles letras de unidad.

## <a name="examples"></a>Ejemplos

Para crear un punto de montaje, escriba:

```
mountvol \sysmount \\?\volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
