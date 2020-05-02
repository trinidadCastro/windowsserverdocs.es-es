---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil Sparse
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 3d0df4e8e8dc16818273393062989ef7c0455c51
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725452"
---
# <a name="fsutil-sparse"></a>Fsutil Sparse
> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7

Administra archivos dispersos.



## <a name="syntax"></a>Sintaxis

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                    Descripción                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  Consulta Sparse.                                                  |
|    queryrange     |                        Examina un archivo y busca rangos que pueden contener datos distintos de cero.                        |
|      setflag      |                                        Marca el archivo indicado como disperso.                                        |
|     SetRange      |                                   Rellena un intervalo especificado de un archivo con ceros.                                   |
|    <FileName>     | Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt. |
| <BeginningOffset> |                              Especifica el desplazamiento en el archivo que se va a marcar como disperso.                              |
|     <Length>      |                 Especifica la longitud de la región en el archivo que se va a marcar como dispersa (en bytes).                 |

## <a name="remarks"></a>Observaciones

-   Un archivo disperso es un archivo con una o más regiones de datos sin asignar. Un programa verá estas regiones sin asignar que contienen bytes con el valor cero, pero no se usa espacio en disco para representar estos ceros. Se asignan todos los datos significativos o distintos de cero, mientras que no se asignan todos los datos insignificantes (cadenas grandes de datos que se componen de ceros). Cuando se lee un archivo disperso, los datos asignados se devuelven como almacenados y se devuelven los datos sin asignar, de forma predeterminada, como ceros, de acuerdo con la especificación de los requisitos de seguridad C2. La compatibilidad con archivos dispersos permite cancelar la asignación de los datos desde cualquier parte del archivo.

-   En un archivo disperso, es posible que los intervalos grandes de ceros no requieran la asignación de disco. El espacio para los datos distintos de cero se asignará según sea necesario cuando se escriba el archivo.

-   Solo los archivos comprimidos o dispersos pueden tener intervalos de ceros conocidos para el sistema operativo.

-   Si el archivo es disperso o comprimido, NTFS puede desasignar el espacio en disco del archivo. Esto establece el intervalo de bytes en ceros sin extender el tamaño del archivo.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos
Para marcar un archivo denominado sample. txt en el directorio C:\Temp como disperso, escriba:

```
fsutil sparse setflag c:\temp\sample.txt 
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


