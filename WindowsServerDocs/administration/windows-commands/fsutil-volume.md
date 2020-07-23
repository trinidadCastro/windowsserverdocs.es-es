---
title: fsutil volume
description: Artículo de referencia para el comando fsutil Volume, que desmonta un volumen, o bien, consulta la unidad de disco duro para determinar cuánto espacio libre está disponible actualmente en la unidad de disco duro o el archivo que usa un clúster determinado.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6c486e47bde08ad002e39cec81e72ace90946cd7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958117"
---
# <a name="fsutil-volume"></a>fsutil volume

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Desmonta un volumen o consulta la unidad de disco duro para determinar la cantidad de espacio libre disponible actualmente en la unidad de disco duro o el archivo que está utilizando un clúster determinado.

## <a name="syntax"></a>Sintaxis

```
fsutil volume [allocationreport] <volumepath>
fsutil volume [diskfree] <volumepath>
fsutil volume [dismount] <volumepath>
fsutil volume [filelayout] <volumepath> <fileID>
fsutil volume [list]
fsutil volume [querycluster] <volumepath> <cluster> [<cluster>] … …
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| allocationreport | Muestra información sobre cómo se usa el almacenamiento en un volumen determinado. |
| `<volumepath>` | Especifica la letra de unidad (seguida de dos puntos). |
| diskfree | Consulta la unidad de disco duro para determinar la cantidad de espacio disponible en ella. |
| desmontar | Desmonta un volumen. |
| filelayout | Muestra los metadatos NTFS del archivo especificado. |
| `<fileID>` | Especifica el identificador de archivo. |
| list | Enumera todos los volúmenes del sistema. |
| querycluster | Busca el archivo que está utilizando un clúster especificado. Puede especificar varios clústeres con el parámetro **querycluster** . |
| `<cluster>` | Especifica el número de clúster lógico (LCN). |

### <a name="examples"></a>Ejemplos

Para mostrar un informe de clústeres asignados, escriba:

```
fsutil volume allocationreport C:
```

Para desmontar un volumen de la unidad C, escriba:

```
fsutil volume dismount c:
```

Para consultar la cantidad de espacio libre de un volumen en la unidad C, escriba:

```
fsutil volume diskfree c:
```

Para mostrar toda la información sobre un archivo o archivos especificados, escriba:

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Para enumerar los volúmenes en disco, escriba:

```
fsutil volume list
```

Para buscar los archivos que usan los clústeres, especificados por los números de clústeres lógicos 50 y 0x2000, en la unidad C, escriba:

```
fsutil volume querycluster C: 50 0x2000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [Cómo funciona NTFS](/previous-versions/windows/it-pro/windows-server-2003/cc781134(v=ws.10))
