---
title: bitsadmin addfile
description: Tema de comandos de Windows para **bitsadmin AddFile**, que agrega un archivo al trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 330e79eb2ba5a824cea54094f64ceb6f9cfd66b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850968"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Agrega un archivo al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |
| RemoteURL | Dirección URL del archivo en el servidor. |
| LocalName | Nombre del archivo en el equipo local. *Localname* debe contener una ruta de acceso absoluta al archivo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Agregue un archivo al trabajo. Repita esta llamada para cada archivo que desee agregar. Si varios trabajos usan *myDownloadJob* como su nombre, debe reemplazar *MYDOWNLOADJOB* por el GUID del trabajo para identificar el trabajo de forma única.

```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

## <a name="additional-references"></a>Referencias adicionales

- &copy; [de la clave de sintaxis de línea de comandos](command-line-syntax-key.md)