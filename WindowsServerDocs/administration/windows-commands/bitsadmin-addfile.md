---
title: bitsadmin addfile
description: Artículo de referencia del comando bitsadmin AddFile, que agrega un archivo al trabajo especificado.
ms.topic: reference
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63df6c33bae3022c91633d4507c1fe9709cdd65e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031383"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Agrega un archivo al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfile <job> <remoteURL> <localname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| remoteURL | Dirección URL del archivo en el servidor. |
| localname | Nombre del archivo en el equipo local. *Localname* debe contener una ruta de acceso absoluta al archivo. |

## <a name="examples"></a>Ejemplos

Para agregar un archivo al trabajo:

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

Repita esta llamada para cada archivo que se va a agregar. Si varios trabajos usan *myDownloadJob* como su nombre, debe reemplazar *MYDOWNLOADJOB* por el GUID del trabajo para identificar el trabajo de forma única.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
