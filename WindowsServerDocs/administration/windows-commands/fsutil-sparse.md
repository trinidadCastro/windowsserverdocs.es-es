---
title: fsutil sparse
description: Artículo de referencia para el comando fsutil Sparse, que administra archivos dispersos.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c765b096f1b41b211d3a779d8f838aa56f31aeb8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925212"
---
# <a name="fsutil-sparse"></a>fsutil sparse

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra archivos dispersos. Un archivo disperso es un archivo con una o más regiones de datos sin asignar.

Un programa ve estas regiones sin asignar que contienen bytes con un valor cero y que no hay espacio en disco que represente ceros. Cuando se lee un archivo disperso, los datos asignados se devuelven como almacenados y se devuelven los datos sin asignar, de forma predeterminada, como ceros, de acuerdo con la especificación de los requisitos de seguridad C2. La compatibilidad con archivos dispersos permite cancelar la asignación de los datos desde cualquier parte del archivo.

## <a name="syntax"></a>Sintaxis

```
fsutil sparse [queryflag] <filename>
fsutil sparse [queryrange] <filename>
fsutil sparse [setflag] <filename>
fsutil sparse [setrange] <filename> <beginningoffset> <length>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| queryflag | Consulta Sparse. |
| queryrange | Examina un archivo y busca rangos que pueden contener datos distintos de cero. |
| setflag | Marca el archivo indicado como disperso. |
| SetRange | Rellena un intervalo especificado de un archivo con ceros. |
| `<filename>` | Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo *C:\documents\filename.txt*. |
| `<beginningoffset>` | Especifica el desplazamiento en el archivo que se va a marcar como disperso. |
| `<length>` | Especifica la longitud de la región en el archivo que se va a marcar como dispersa (en bytes). |

#### <a name="remarks"></a>Comentarios

- Se asignan todos los datos significativos o distintos de cero, mientras que no se asignan todos los datos no significativos (cadenas grandes de datos que se componen de ceros).

- En un archivo disperso, es posible que los intervalos grandes de ceros no requieran la asignación de disco. El espacio para los datos distintos de cero se asigna según sea necesario cuando se escribe el archivo.

- Solo los archivos comprimidos o dispersos pueden tener intervalos de ceros conocidos para el sistema operativo.

- Si el archivo es disperso o comprimido, NTFS puede anular la asignación del espacio en disco del archivo. Esto establece el intervalo de bytes en ceros sin extender el tamaño del archivo.

### <a name="examples"></a>Ejemplos

Para marcar un archivo llamado *sample.txt* en el directorio *c:\temp* como disperso, escriba:

```
fsutil sparse setflag c:\temp\sample.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
