---
title: fsutil tiering
description: Artículo de referencia para el comando fsutil tiering, que permite la administración de funciones de nivel de almacenamiento, como la configuración y deshabilitación de marcas y la lista de niveles.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 90b92d7f0694c6d43e4737a64d72c288a2069098
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032983"
---
# <a name="fsutil-tiering"></a>fsutil tiering

> Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016 y Windows 10

Habilita la administración de funciones de nivel de almacenamiento, como la configuración y deshabilitación de marcas y la lista de niveles.

## <a name="syntax"></a>Sintaxis

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| clearflags | Deshabilita las marcas de comportamiento de niveles de un volumen. |
| `<volume>` | Especifica el volumen. |
| /trnh | En el caso de los volúmenes con almacenamiento en capas, hace que la recopilación térmica se deshabilite.<p>Solo se aplica a NTFS y ReFS. |
| queryflags | Consulta las marcas de comportamiento de niveles de un volumen. |
| regionlist | Enumera las regiones en capas de un volumen y sus respectivos niveles de almacenamiento. |
| SetFlags | Habilita las marcas de comportamiento de niveles de un volumen. |
| tierlist | Enumera las capas de almacenamiento asociadas a un volumen. |

### <a name="examples"></a>Ejemplos

Para consultar las marcas en el volumen C, escriba:

```
fsutil tiering queryflags C:
```

Para establecer las marcas en el volumen C, escriba:

```
fsutil tiering setflags C: /trnh
```

Para borrar las marcas del volumen C, escriba:

```
fsutil tiering clearflags C: /trnh
```

Para enumerar las regiones del volumen C y sus respectivas capas de almacenamiento, escriba:

```
fsutil tiering regionlist C:
```

Para enumerar los niveles del volumen C, escriba:

```
fsutil tiering tierlist C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
