---
title: Wbadmin start systemstatebackup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d98ba295b2a76baf98e85a01a02677d57922877d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440263"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



Crea una copia de seguridad del estado del sistema del equipo local y lo almacena en la ubicación especificada.

> [!NOTE]
> Copia de seguridad de Windows Server no copia de seguridad o recuperar subárboles del registro de usuario (HKEY_CURRENT_USER) como parte de la copia de seguridad o recuperación del estado del sistema.

Para realizar una copia de seguridad del estado del sistema con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                                                                                                                      Descripción                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Especifica la ubicación donde desea almacenar la copia de seguridad. La ubicación de almacenamiento requiere una letra de unidad o un volumen basado en el GUID del formato: \\ \\? \Volume {*GUID*}.</br>Una copia de seguridad del estado del sistema en una carpeta compartida de red no se admite en un equipo que ejecuta Windows Server 2008. Si el servidor se está ejecutando Windows Server 2008 R2 o posterior puede usar el comando **- backuptarget:\\\\servername\sharedFolder\\**  para almacenar copias de seguridad de estado del sistema. |
|    -quiet     |                                                                                                                                                                                                   Se ejecuta el subcomando sin solicitudes para el usuario.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentarios

Para obtener información acerca de cómo guardar una copia de seguridad del estado del sistema a un volumen que, a su vez, contiene los archivos de estado del sistema, consulte el artículo 944530 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)).

## <a name="BKMK_examples"></a>Ejemplos

Para crear una copia de seguridad del estado del sistema y lo almacena en el volumen f, escriba:
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx) cmdlet