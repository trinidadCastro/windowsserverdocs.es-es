---
title: fsutil transaction
description: Tema de referencia del comando fsutil Transaction, que administra las transacciones NTFS.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fc81934c5838fd81722b27a7b7e57b14709ed26a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437050"
---
# <a name="fsutil-transaction"></a>fsutil transaction

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra las transacciones NTFS.

## <a name="syntax"></a>Sintaxis

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <filename>
fsutil transaction [list]
fsutil transaction [query] [{files | all}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| confirmación | Marca el final de una transacción especificada implícita o explícita correcta. |
| `<GUID>` | Especifica el valor de GUID que representa una transacción. |
| FileInfo  | Muestra información de transacción para el archivo especificado. |
| `<filename>` | Especifica la ruta de acceso completa y el nombre de archivo. |
| list | Muestra una lista de las transacciones que se están ejecutando actualmente. |
| Query | Muestra información de la transacción especificada.<ul><li>Si `fsutil transaction query files` se especifica, la información de archivo solo se muestra para la transacción especificada.</li><li>Si `fsutil transaction query all` se especifica, se mostrará toda la información de la transacción.</li></ul> |
| revertir | Revierte una transacción especificada al principio. |

### <a name="examples"></a>Ejemplos

Para mostrar la información de transacción para el archivo *c:\Test.txt*, escriba:

```
fsutil transaction fileinfo c:\test.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS de transacciones](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730726(v=ws.10))
