---
title: bitsadmin setnotifyflags
description: Artículo de referencia para el comando bitsadmin setnotifyflags, que establece las marcas de notificación de eventos para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4054788bb8c14e4bd9be38296f5c0f933de9462
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927619"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Establece las marcas de notificación de eventos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setnotifyflags <job> <notifyflags>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| notifyflags | Puede incluir una o varias de las siguientes marcas de notificación, entre las que se incluyen:<ul><li>**1.** genera un evento cuando se han transferido todos los archivos del trabajo.</li><li>**2.** genera un evento cuando se produce un error.</li><li>**3.** genera un evento cuando se han completado las transferencias de todos los archivos o cuando se produce un error.</li><li>**4.** deshabilita las notificaciones.</li></ul> |

## <a name="examples"></a>Ejemplos

Para establecer las marcas de notificación de para generar un evento cuando se produce un error, para un trabajo denominado *myDownloadJob*:

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
