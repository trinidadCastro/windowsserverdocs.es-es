---
title: bitsadmin create
description: Artículo de referencia del comando bitsadmin Create, que crea un trabajo de transferencia con el nombre para mostrar especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3e0f8fb7d8396a238cabcbeb375f61cbe526c3f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928349"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un trabajo de transferencia con el nombre para mostrar especificado.

> [!NOTE]
> Los **/Upload**   tipos de parámetro/upload y **/upload-reply** no son compatibles con bits 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| type | Hay tres tipos de trabajos:<ul><li>**Descargar.** Transfiere datos de un servidor a un archivo local.</li><li>**Subir.** Transfiere datos de un archivo local a un servidor.</li><li>**/Upload-Reply.** Transfiere datos de un archivo local a un servidor y recibe un archivo de respuesta del servidor.</li></ul>El valor predeterminado de este parámetro es **/download** si no se especifica. |
| displayname | Nombre para mostrar asignado al trabajo recién creado. |

## <a name="examples"></a>Ejemplos

Para crear un trabajo de descarga denominado *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de reanudación de bitsadmin](bitsadmin-resume.md)

- [bitsadmin (comando)](bitsadmin.md)
