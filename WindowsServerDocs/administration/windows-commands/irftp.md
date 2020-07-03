---
title: irftp
description: Artículo de referencia del comando irftp, que envía archivos a través de un vínculo de infrarrojos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92bb484650245555597121c8b6f6378d3c09209c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924344"
---
# <a name="irftp"></a>irftp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía archivos a través de un vínculo de infrarrojos.

> [!IMPORTANT]
> Asegúrese de que los dispositivos diseñados para comunicarse a través de un vínculo de infrarrojos tienen la funcionalidad de infrarrojos habilitada y funcionan correctamente. Asegúrese también de que se establece un vínculo de infrarrojos entre los dispositivos.

## <a name="syntax"></a>Sintaxis

```
irftp [<drive>:\] [[<path>] <filename>] [/h][/s]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>:\` | Especifica la unidad que contiene los archivos que desea enviar a través de un vínculo de infrarrojos. |
| `[path]<filename>` | Especifica la ubicación y el nombre del archivo o conjunto de archivos que desea enviar a través de un vínculo de infrarrojos. Si especifica un conjunto de archivos, debe especificar la ruta de acceso completa para cada archivo. |
| /h | Especifica el modo oculto. Cuando se utiliza el modo oculto, los archivos se envían sin mostrar el cuadro de diálogo vínculo inalámbrico. |
| /s | Abre el cuadro de diálogo **vínculo inalámbrico** , de modo que puede seleccionar el archivo o el conjunto de archivos que desea enviar sin utilizar la línea de comandos para especificar la unidad, la ruta de acceso y los nombres de archivo. El cuadro de diálogo **vínculo inalámbrico** también se abre si usa este comando sin ningún parámetro. |

### <a name="examples"></a>Ejemplos

Para enviar *c:\example.txt* sobre el vínculo de infrarrojos, escriba:

```
irftp c:\example.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
