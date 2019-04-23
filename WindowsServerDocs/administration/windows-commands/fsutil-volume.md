---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: volumen de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: a8576dce4be639a516f8898e78bb6db12c91e171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882456"
---
# <a name="fsutil-volume"></a>volumen de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Desmonta un volumen, o consultas de la unidad de disco duro para determinar cuánto espacio libre está disponible actualmente en la unidad de disco duro o archivo que está usando un clúster determinado.

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
|allocationreport|Muestra información acerca de cómo se usa el almacenamiento en un volumen determinado.|
|\<VolumePath>|Especifica la letra de unidad (seguida de dos puntos).|
|diskfree|Consulta de la unidad de disco duro para determinar la cantidad de espacio libre en él.|
|desmontar|Desmonta un volumen.|
|filelayout|Muestra los metadatos NTFS para el archivo especificado.|
|\<fileid>|Especifica el identificador de archivo.|
|list|Enumera todos los volúmenes en el sistema.|
|querycluster|Busca el archivo que está usando un clúster especificado. Puede especificar varios clústeres con el **querycluster** parámetro.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|
|\<cluster>|Especifica el número de clúster lógico (LCN).|

## <a name="BKMK_examples"></a>Ejemplos
Para mostrar un informe de clústeres asignados, escriba:

```
fsutil volume allocationreport C:
```

Para desmontar un volumen en la unidad C, escriba:

```
fsutil volume dismount c:
```

Para consultar la cantidad de espacio libre de un volumen en la unidad C, escriba:

```
fsutil volume diskfree c:
```

Para mostrar toda la información sobre los archivos especificados, escriba:

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Para obtener una lista de los volúmenes de disco, escriba:

```
fsutil volume list
```

Para buscar los archivos que usan los clústeres, especificados por los números de clúster lógico 50 y 0 x 2000, en la unidad C, escriba:

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Funcionamiento de NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


