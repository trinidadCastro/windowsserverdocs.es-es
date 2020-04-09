---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: Fsutil Dirty
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: cf3685bae9ed76ede4da6df244139437d92250c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844338"
---
# <a name="fsutil-dirty"></a>Fsutil Dirty
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Consulta o establece el bit de integridad de un volumen. Cuando se establece el bit de integridad de un volumen, **Autochk** comprueba automáticamente si hay errores en el volumen la próxima vez que se reinicie el equipo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil dirty {query | set} <VolumePath>
```

### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                 Descripción                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     consulta     |                                  Consulta el bit de integridad del volumen especificado.                                   |
|      set      |                                    Establece el bit de integridad del volumen especificado.                                    |
| \<VolumePath > | Especifica el nombre de la unidad seguido de dos puntos o un GUID con el siguiente formato: **volumen {** <em>GUID</em> **}** . |

## <a name="remarks"></a>Comentarios

-   El bit de integridad de un volumen indica que el sistema de archivos puede estar en un estado incoherente. El bit de integridad se puede establecer porque:

    -   El volumen está en línea y tiene cambios pendientes.

    -   Se realizaron cambios en el volumen y se apagó el equipo antes de que los cambios se confirmaran en el disco.

    -   Se detectaron daños en el volumen.

-   Si el bit de integridad se establece cuando se reinicia el equipo, **CHKDSK** se ejecuta para comprobar la integridad del sistema de archivos e intentar corregir los problemas con el volumen.

## <a name="examples"></a><a name="BKMK_examples"></a>Example
Para consultar el bit de integridad de la unidad C, escriba:

```
fsutil dirty query c:
```

-   Si el volumen está sucio, se muestra el siguiente resultado:

    `Volume C: is dirty`

-   Si el volumen no está sucio, se muestra el siguiente resultado:

    `Volume C: is not dirty`

Para establecer el bit de integridad en la unidad C, escriba:

```
fsutil dirty set C:
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


