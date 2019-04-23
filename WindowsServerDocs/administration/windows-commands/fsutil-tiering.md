---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: los niveles de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859256"
---
# <a name="fsutil-tiering"></a>los niveles de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Permite la administración de las funciones de nivel de almacenamiento, como la configuración y deshabilitar las marcas y listado de los niveles.

## <a name="syntax"></a>Sintaxis

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|clearflags|Deshabilita las marcas de comportamiento de los niveles de un volumen.|
|\<volume>|Especifica el volumen.|
|/TrNH|Para volúmenes de almacenamiento en capas, hace que calor de recopilación para que se deshabilite.<br /><br>Solo se aplica a NTFS y ReFS.|
|queryflags|Consulta las marcas de comportamiento de los niveles de un volumen.|
|regionlist|Enumera las regiones en capas de un volumen y sus niveles de almacenamiento correspondiente.|
|setflags|Habilita las marcas de comportamiento de los niveles de un volumen.|
|tierlist|Enumera el tieres almacenamiento asociado a un volumen.|


### <a name="examples"></a>Ejemplos

Para consultar las marcas en el volumen C, escriba:

```
fsutil tiering clearflags C:
```

Para establecer las marcas en el volumen C, escriba:

```
fsutil tiering setflags C: /TrNH
```

Para borrar las marcas en el volumen C, escriba:

```
fsutil tiering clearflags C: /TrNH
```

Para enumerar las regiones del volumen C y sus niveles de almacenamiento correspondiente, escriba:

```
fsutil tiering regionlist C:
```

Para obtener una lista de los niveles de volumen C, escriba:

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

