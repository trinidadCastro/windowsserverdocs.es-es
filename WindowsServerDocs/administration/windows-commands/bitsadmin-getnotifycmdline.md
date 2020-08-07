---
title: bitsadmin getnotifycmdline
description: Artículo de referencia para el comando bitsadmin getnotifycmdline, que recupera el comando de línea de comandos que se ejecuta cuando el trabajo finaliza la transferencia de datos.
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5726147701e3ef09e40b3edf900a2e4d417f95cc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894105"
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
