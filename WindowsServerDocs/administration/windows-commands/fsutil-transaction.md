---
title: fsutil transaction
description: Artículo de referencia para el comando fsutil Transaction, que administra las transacciones NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f70281af6ecf652cc1dba95ec09b07529f71752e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889797"
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

Para mostrar la información de transacción del *c:\test.txt*de archivos, escriba:

```
fsutil transaction fileinfo c:\test.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS de transacciones](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v=ws.10))
