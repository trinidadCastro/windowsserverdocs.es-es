---
title: bitsadmin getnotifycmdline
description: Artículo de referencia para el comando bitsadmin getnotifycmdline, que recupera el comando de línea de comandos que se ejecuta cuando el trabajo finaliza la transferencia de datos.
ms.topic: reference
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85ed3dc301aed9d79619a1bbc6e9dc835b2102a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033483"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Recupera el comando de línea de comandos que se ejecutará después de que el trabajo especificado termine de transferir los datos.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el comando de línea de comandos que usa el servicio cuando se completa el trabajo denominado *myDownloadJob* .

```
bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
