---
title: bitsadmin getnotifyflags
description: Artículo de referencia para el comando bitsadmin getnotifyflags, que recupera las marcas de notificación para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea97c039f372a2211b1e2a6c640c4499a38dfe4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926924"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Recupera las marcas de notificación para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="remarks"></a>Comentarios

El trabajo puede contener una o varias de las siguientes marcas de notificación:

| Marcar | Descripción |
| ----- | ----- |
| 0x001 | Genera un evento cuando se han transferido todos los archivos del trabajo. |
| 0x002 | Genera un evento cuando se produce un error. |
| 0x004 | Deshabilite las notificaciones. |
| 0x008 | Genera un evento cuando se modifica el trabajo o se realiza el progreso de la transferencia. |

## <a name="examples"></a>Ejemplos

Para recuperar las marcas de notificación del trabajo denominado *myDownloadJob*:

```
bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
