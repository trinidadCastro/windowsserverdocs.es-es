---
title: mountvol
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3de8e5744c50acff3fdad0c7cf1dabf14fb144
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373577"
---
# <a name="mountvol"></a>mountvol



Crea, elimina o enumera un punto de montaje de volumen.

Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive >:] <Path>|Especifica el directorio NTFS existente donde residirá el punto de montaje.|
|@no__t 0VolumeName >|Especifica el nombre del volumen que es el destino del punto de montaje. El nombre del volumen utiliza la sintaxis siguiente, donde *GUID* es un identificador único global:</br>`\\\\?\Volume\{GUID}\`</br>Los corchetes {} son obligatorios.|
|/d|Quita el punto de montaje del volumen de la carpeta especificada.|
|/l|Muestra el nombre del volumen montado para la carpeta especificada.|
|/p|Quita el punto de montaje del volumen del directorio especificado, desmonta el volumen básico y desconecta el volumen básico, lo que hace que sea desmontable. Si otros procesos están usando el volumen, **Mountvol** cierra los identificadores abiertos antes de desmontar el volumen.|
|/r|Quita los directorios de punto de montaje de volumen y la configuración del registro para los volúmenes que ya no están en el sistema, evitando que se monten automáticamente y se les proporcionen los puntos de montaje de volumen anteriores cuando se vuelvan a agregar al sistema.|
|/n|Deshabilita el montaje automático de nuevos volúmenes básicos. Los nuevos volúmenes no se montan automáticamente cuando se agregan al sistema.|
|/e|Vuelve a habilitar el montaje automático de nuevos volúmenes básicos.|
|/s|Monta la partición del sistema EFI en la unidad especificada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   **Mountvol** le permite vincular volúmenes sin necesidad de una letra de unidad.
-   Los volúmenes que se desmontan con **/p** se enumeran en la lista de volúmenes como "no montado hasta que se crea un punto de montaje de volumen". Si el volumen tiene más de un punto de montaje, use **/d** para quitar los puntos de montaje adicionales antes de usar **/p**. Puede volver a hacer que se pueda montar el volumen básico si asigna un punto de montaje de volumen.
-   Si necesita expandir el espacio de volumen sin volver a formatear o reemplazar una unidad de disco duro, puede Agregar una ruta de acceso de montaje a otro volumen. La ventaja de usar un volumen con varias rutas de acceso de montaje es que puede acceder a todos los volúmenes locales mediante una sola letra de unidad (como `C:`). No es necesario recordar qué volumen corresponde a la letra de unidad, aunque todavía puede montar volúmenes locales y asignarles letras de unidad.

## <a name="BKMK_examples"></a>Example

Para crear un punto de montaje, escriba:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
