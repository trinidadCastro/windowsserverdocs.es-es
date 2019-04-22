---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: transacción de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 93c981d077dbb027400a1eb2e2c662f72c14cc44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825216"
---
# <a name="fsutil-transaction"></a>transacción de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Administra las transacciones de NTFS.

Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxis

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|Confirmación|Marca el final de una transacción correcta de especificado implícita o explícita.|
|<GUID>|Especifica el valor GUID que representa una transacción.|
|fileinfo|Muestra información de transacción para el archivo especificado.|
|<Filename>|Especifica la ruta de acceso completa y nombre de archivo.|
|list|Muestra una lista de las transacciones actualmente en ejecución.|
|query|Muestra información de la transacción especificada.<br /><br />-If **archivos de consulta de transacción de fsutil** se especifica, se mostrará la información de archivo solo para la transacción especificada.<br />-If **consultar transacciones fsutil todos** se especifica, se mostrará toda la información de la transacción.|
|Reversión|Revierte una transacción especificada al principio.|

### <a name="remarks"></a>Comentarios

-   NTFS transaccional se introdujo en Windows Server 2008.

### <a name="BKMK_examples"></a>Ejemplos
Para mostrar información de transacción para el archivo c:\test.txt, escriba:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transaccional](https://go.microsoft.com/fwlink/?LinkID=165402)


