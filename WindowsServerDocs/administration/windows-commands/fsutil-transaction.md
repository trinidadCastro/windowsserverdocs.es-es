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
ms.openlocfilehash: aa6692dbb8af1ec832650971c6723c060fc2cd56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844018"
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

#### <a name="parameters"></a>Parámetros

| Parámetro  |                                                                                                                                                     Descripción                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   promete   |                                                                                                                      Marca el final de una transacción especificada implícita o explícita correcta.                                                                                                                      |
|   <GUID>   |                                                                                                                               Especifica el valor de GUID que representa una transacción.                                                                                                                               |
|  FileInfo  |                                                                                                                              Muestra información de transacción para el archivo especificado.                                                                                                                               |
| <Filename> |                                                                                                                                         Especifica la ruta de acceso completa y el nombre de archivo.                                                                                                                                          |
|    lista    |                                                                                                                                 Muestra una lista de las transacciones que se están ejecutando actualmente.                                                                                                                                  |
|   consulta    | Muestra información de la transacción especificada.<p>-Si se especifica **archivos de consulta de transacción fsutil** , la información de archivo solo se mostrará para la transacción especificada.<br />-Si se especifica **fsutil Transaction Query All** , se mostrará toda la información de la transacción. |
|  revertir  |                                                                                                                                Revierte una transacción especificada al principio.                                                                                                                                 |

### <a name="remarks"></a>Comentarios

-   NTFS transaccional se presentó en Windows Server 2008.

### <a name="examples"></a><a name="BKMK_examples"></a>Example
Para mostrar la información de transacción para el archivo c:\Test.txt, escriba:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS transaccional](https://go.microsoft.com/fwlink/?LinkID=165402)


