---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil, transacción
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 286660baad699e21abe751a9cb956b1ac7613e80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376859"
---
# <a name="fsutil-transaction"></a>Fsutil, transacción
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Administra las transacciones NTFS.

Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxis

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parámetros

| Parámetro  |                                                                                                                                                     Descripción                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   promete   |                                                                                                                      Marca el final de una transacción especificada implícita o explícita correcta.                                                                                                                      |
|   <GUID>   |                                                                                                                               Especifica el valor de GUID que representa una transacción.                                                                                                                               |
|  FileInfo  |                                                                                                                              Muestra información de transacción para el archivo especificado.                                                                                                                               |
| <Filename> |                                                                                                                                         Especifica la ruta de acceso completa y el nombre de archivo.                                                                                                                                          |
|    list    |                                                                                                                                 Muestra una lista de las transacciones que se están ejecutando actualmente.                                                                                                                                  |
|   query    | Muestra información de la transacción especificada.<br /><br />-Si se especifica **archivos de consulta de transacción fsutil** , la información de archivo solo se mostrará para la transacción especificada.<br />-Si se especifica **fsutil Transaction Query All** , se mostrará toda la información de la transacción. |
|  recuperación  |                                                                                                                                Revierte una transacción especificada al principio.                                                                                                                                 |

### <a name="remarks"></a>Comentarios

-   NTFS transaccional se presentó en Windows Server 2008.

### <a name="BKMK_examples"></a>Example
Para mostrar la información de transacción para el archivo c:\Test.txt, escriba:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transaccional](https://go.microsoft.com/fwlink/?LinkID=165402)


