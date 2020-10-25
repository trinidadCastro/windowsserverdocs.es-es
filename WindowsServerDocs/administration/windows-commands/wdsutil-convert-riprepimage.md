---
title: Convert-riprepimage
description: Artículo de referencia para el comando Convert-riprepimage, que convierte una imagen existente de preparación de la instalación remota (RIPrep) al formato de imagen de Windows (. wim).
ms.topic: reference
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4062aa747bd02956a498c16c9e8aa15e96d8170d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524930"
---
# <a name="convert-riprepimage"></a>Convert-riprepimage

Convierte una imagen existente de preparación de la instalación remota (RIPrep) en el formato de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Convert-RIPrepImage /FilePath:<Filepath and name> /DestinationImage /FilePath:<Filepath and name> [/Name:<Name>] [/Description:<Description>] [/InPlace] [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| FilePath`<Filepath and name>` | Especifica la ruta de acceso completa y el nombre del archivo. sif correspondiente a la imagen RIPrep. Este archivo se denomina normalmente Riprep. sif y se encuentra en la subcarpeta **\Templates** de la carpeta que contiene la imagen Riprep. |
| /DestinationImage | Especifica la configuración de la imagen de destino.  Utiliza las siguientes opciones:<ul><li>`/FilePath:<Filepath and name>` : Establece la ruta de acceso completa del archivo para el nuevo archivo. Por ejemplo: **C:\Temp\convert.Wim**</li><li>[ `/Name:<Name>` ]: Establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usa el nombre para mostrar de la imagen de origen.</li><li>[ `/Description:<Description>` ]: Establece la descripción de la imagen.</li><li>[/InPlace]: especifica que la conversión debe realizarse en la imagen original RIPrep y no en una copia de la imagen original, que es el comportamiento predeterminado.</li><li>[ `/Overwrite:{Yes | No | Append}` -Establece si esta imagen debe sobrescribir o anexar los archivos existentes.</li></ul> |

## <a name="examples"></a>Ejemplos

Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim, escriba:

```
wdsutil /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English \Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage /FilePath:C:\Temp\RIPREP.wim
```

Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim con el nombre y la descripción especificados y sobrescribirla con el nuevo archivo si ya existe un archivo, escriba:

```
wdsutil /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server \RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif /DestinationImage /FilePath:\\Server\Share\RIPREP.wim /Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP /Overwrite:Append
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
