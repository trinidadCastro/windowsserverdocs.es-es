---
title: mountvol
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e31a167d98203b684917aceee2603a29dd478a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846326"
---
# <a name="mountvol"></a>mountvol



Crea, elimina o muestra un punto de montaje del volumen.

Para obtener ejemplos de cómo utilizar este comando, consulte [ejemplos](#BKMK_examples).

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
|[\<Drive>:]<Path>|Especifica el directorio NTFS donde residirá el punto de montaje.|
|\<VolumeName>|Especifica el nombre del volumen que es el destino del punto de montaje. El nombre del volumen usa la sintaxis siguiente, donde *GUID* es un identificador único global:</br>`\\\\?\Volume\{GUID}\`</br>Se requieren las llaves {}.|
|/d|Quita el punto de montaje de volumen de la carpeta especificada.|
|/l|Muestra el nombre de volumen montado para la carpeta especificada.|
|/p|Quita el punto de montaje del volumen del directorio especificado, desmonta el volumen básico y pone el volumen básico sin conexión, lo que puede montar. Si otros procesos están usando el volumen, **mountvol** cerrará los identificadores abiertos antes de desmontar el volumen.|
|/r|Quita los directorios del punto de montaje de volumen y la configuración del registro para los volúmenes que ya no están en el sistema, impide que se monten automáticamente y reciban su volumen anteriores puntos cuando se agrega al sistema de montaje.|
|/n|Deshabilita el montaje automático de nuevos volúmenes básicos. Los volúmenes nuevos no se montan automáticamente cuando se agregan al sistema.|
|/e|Vuelve a habilitar el montaje automático de nuevos volúmenes básicos.|
|/s|Monta la partición del sistema EFI en la unidad especificada. Está disponible en equipos basados en Itanium sólo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   **Mountvol** permite vincular los volúmenes sin necesidad de una letra de unidad.
-   Los volúmenes que se desmontan utilizando **/p** se muestran en la lista de volúmenes como "Se CREÓ no montado hasta un punto de montaje". Si el volumen tiene más de un montaje de punto, utilice **/d** para quitar los puntos de montaje adicionales antes de usar **/p**. Puede hacer que se puede montar nuevamente el volumen básico mediante la asignación de un punto de montaje del volumen.
-   Si necesita ampliar el espacio del volumen sin volver a formatear o sustituir un disco duro, puede agregar una ruta de acceso de montaje a otro volumen. La ventaja de uso de un volumen con varias rutas de acceso de montaje es que puede tener acceso a todos los volúmenes locales mediante el uso de una letra de unidad (como `C:`). No es necesario que recuerde qué volumen corresponde a qué letra de unidad, aunque todavía puede montar volúmenes locales y asignarles letras de unidad.

## <a name="BKMK_examples"></a>Ejemplos

Para crear un punto de montaje, escriba:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)