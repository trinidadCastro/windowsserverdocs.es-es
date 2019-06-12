---
title: Con el subcomando add AllDriverPackages
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f934d8c65da939fb60c564b375699f411b7c9ac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440833"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>Con el subcomando add AllDriverPackages



Agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Parámetros

|          Parámetro           |                                                              Descripción                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  / FolderPath:\<ruta de acceso de carpeta >  |                      Especifica la ruta de acceso completa a la carpeta que contiene los archivos .inf para los paquetes de controladores.                      |
|   [/ Server:\<nombre del servidor >]   | Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local. |
|     [/ Arquitectura: {x86      |                                                                 ia64                                                                  |
| [/ DriverGroup:\<nombre de grupo >] |                             Especifica el nombre del grupo de controladores a la que se deben agregar los paquetes.                             |

## <a name="BKMK_examples"></a>Ejemplos

Para agregar paquetes de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)