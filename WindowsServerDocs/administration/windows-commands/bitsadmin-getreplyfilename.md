---
title: bitsadmin getreplyfilename
description: Artículo de referencia del comando bitsadmin getreplyfilename, que obtiene la ruta de acceso del archivo que contiene la respuesta de carga del servidor para el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fce592b52b9efe9d3d67c893dd6b2441446c0938
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926737"
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
