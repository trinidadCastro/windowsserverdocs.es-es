---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: fsutil sparse
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b1bc4e45ed2a2b06c72318e0999988ed8f016c40
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438970"
---
# <a name="fsutil-sparse"></a>fsutil sparse
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra los archivos dispersos.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

## <a name="parameters"></a>Parámetros

|     Parámetro     |                                                    Descripción                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  Consultas dispersas.                                                  |
|    queryrange     |                        Analiza un archivo y busca los intervalos que pueden contener datos distinto de cero.                        |
|      setflag      |                                        Marca el archivo indicado como dispersa.                                        |
|     SetRange      |                                   Rellena un intervalo especificado de un archivo con ceros.                                   |
|    <FileName>     | Especifica la ruta de acceso completa al archivo incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt. |
| <BeginningOffset> |                              Especifica el desplazamiento dentro del archivo para marcar como sparse.                              |
|     <Length>      |                 Especifica la longitud de la región en el archivo esté marcado como dispersa (en bytes).                 |

## <a name="remarks"></a>Comentarios

-   Un archivo disperso es un archivo con una o varias regiones de datos sin asignar en ella. Un programa verá estas regiones sin asignar como que contiene los bytes con valor cero, pero no hay espacio en disco se utiliza para representar estos ceros. Se asigna todos los datos significativos o distinto de cero, mientras que todos los datos nonmeaningful (cadenas grandes de datos que se componen de ceros) no está asignada. Cuando se lee un archivo disperso, datos asignados se devuelven como almacenados y sin asignar se devuelven datos, de forma predeterminada, como ceros, de acuerdo con la especificación de requisito de seguridad C2. Compatibilidad con archivos dispersos permite que los datos cancelar la asignación desde cualquier lugar en el archivo.

-   En un archivo disperso, grandes intervalos de ceros pueden no requerir asignación de disco. Según sea necesario cuando se escribe el archivo, se asignará el espacio de datos distinto de cero.

-   Solo los archivos dispersos o comprimidos pueden tener intervalos con ceros sabe que el sistema operativo.

-   Si el archivo es dispersos o comprimidos, NTFS puede desasignar el espacio en disco en el archivo. Esto establece el intervalo de bytes a ceros sin tener que ampliar el tamaño del archivo.

## <a name="BKMK_examples"></a>Ejemplos
Para marcar un archivo denominado Sample.txt en el directorio C:\Temp como disperso, escriba:

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


