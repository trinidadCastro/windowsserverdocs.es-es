---
title: fsutil dirty
description: Artículo de referencia para el comando fsutil Dirty, que consulta o establece el bit de integridad de un volumen.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: fa63ec550821f99dffa59f092bb0f0523bfb948b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030153"
---
# <a name="fsutil-dirty"></a>fsutil dirty

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta o establece el bit de integridad de un volumen. Cuando se establece el bit de integridad de un volumen, **Autochk** comprueba automáticamente si hay errores en el volumen la próxima vez que se reinicie el equipo.

## <a name="syntax"></a>Sintaxis

```
fsutil dirty {query | set} <volumepath>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| Query | Consulta el bit de integridad del volumen especificado. |
| set | Establece el bit de integridad del volumen especificado. |
| `<volumepath>` | Especifica el nombre de la unidad seguido de dos puntos o un GUID con el siguiente formato: `volume{GUID}` . |

#### <a name="remarks"></a>Observaciones

- El bit de integridad de un volumen indica que el sistema de archivos puede estar en un estado incoherente. El bit de integridad se puede establecer porque:

    - El volumen está en línea y tiene cambios pendientes.

    - Se realizaron cambios en el volumen y se apagó el equipo antes de que los cambios se confirmaran en el disco.

    - Se detectaron daños en el volumen.

- Si el bit de integridad se establece cuando se reinicia el equipo, **CHKDSK** se ejecuta para comprobar la integridad del sistema de archivos e intentar corregir los problemas con el volumen.

### <a name="examples"></a>Ejemplos

Para consultar el bit de integridad de la unidad C, escriba:

```
fsutil dirty query c:
```

- Si el volumen está sucio, se muestra el siguiente resultado: `Volume C: is dirty`

- Si el volumen no está sucio, se muestra el siguiente resultado: `Volume C: is not dirty`

Para establecer el bit de integridad en la unidad C, escriba:

```
fsutil dirty set C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
