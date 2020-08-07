---
title: bitsadmin getreplyfilename
description: Artículo de referencia del comando bitsadmin getreplyfilename, que obtiene la ruta de acceso del archivo que contiene la respuesta de carga del servidor para el trabajo.
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 213df5c5dcef57db8f1cdc2b26c90dbdc124007c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893890"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtiene la ruta de acceso del archivo que contiene la respuesta de carga del servidor para el trabajo.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el nombre de archivo de la respuesta de carga para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
