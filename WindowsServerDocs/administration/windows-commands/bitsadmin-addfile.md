---
title: bitsadmin addfile
description: Artículo de referencia del comando bitsadmin AddFile, que agrega un archivo al trabajo especificado.
ms.topic: reference
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 74e77f2af220a057ed0ad87da797e9ae88c7aef5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632747"
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
