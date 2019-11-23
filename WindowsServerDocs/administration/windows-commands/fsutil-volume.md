---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil Volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c4496cfec94823ae177bc6de4fac83dc977fb61d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376699"
---
# <a name="fsutil-volume"></a>Fsutil Volume
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Desmonta un volumen o consulta la unidad de disco duro para determinar la cantidad de espacio libre disponible actualmente en la unidad de disco duro o el archivo que está utilizando un clúster determinado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|allocationreport|Muestra información sobre cómo se usa el almacenamiento en un volumen determinado.|
|\<VolumePath >|Especifica la letra de unidad (seguida de dos puntos).|
|diskfree|Consulta la unidad de disco duro para determinar la cantidad de espacio disponible en ella.|
|desmontar|Desmonta un volumen.|
|filelayout|Muestra los metadatos NTFS del archivo especificado.|
|\<fileid >|Especifica el identificador de archivo.|
|list|Enumera todos los volúmenes del sistema.|
|querycluster|Busca el archivo que está utilizando un clúster especificado. Puede especificar varios clústeres con el parámetro **querycluster** .<br /><br />Este parámetro se aplica a: Windows Server 2008 R2 y Windows 7.|
|> de clúster de \<|Especifica el número de clúster lógico (LCN).|

## <a name="BKMK_examples"></a>Example
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

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Cómo funciona NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


