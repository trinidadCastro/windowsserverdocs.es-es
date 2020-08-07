---
title: wbadmin start systemstatebackup
description: Artículo de referencia de Wbadmin Start systemstatebackup, que crea una copia de seguridad del estado del sistema del equipo local y la almacena en la ubicación especificada.
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386f5053dd547c4b5285a2b9a09cea76238dcf5b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879657"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup



Crea una copia de seguridad del estado del sistema del equipo local y la almacena en la ubicación especificada.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza copias de seguridad ni recupera subárboles de usuario del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

Para realizar una copia de seguridad del estado del sistema con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                                                                                                                      Descripción                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Especifica la ubicación en la que desea almacenar la copia de seguridad. La ubicación de almacenamiento requiere una letra de unidad o un volumen basado en GUID con el formato: \\ \\ ? \Volume{*GUID*}.</br>Una copia de seguridad de estado del sistema en una carpeta de red compartida no se admite en un equipo que ejecute Windows Server 2008. Si el servidor ejecuta Windows Server 2008 R2 o una versión posterior, puede usar el comando **-backupTarget: \\ \\ servername\sharedFolder \\ ** para almacenar copias de seguridad del estado del sistema. |
|    -quiet     |                                                                                                                                                                                                   Ejecuta el subcomando sin preguntar al usuario.                                                                                                                                                                                                    |

## <a name="remarks"></a>Observaciones

Para obtener información acerca de cómo guardar una copia de seguridad de estado del sistema en un volumen que, a su vez, contiene archivos de estado del sistema, consulte el artículo 944530 en Microsoft Knowledge base ( [https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439) ).

## <a name="examples"></a>Ejemplos

Para crear una copia de seguridad del estado del sistema y almacenarla en el volumen f, escriba:
```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBBackup](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10))
