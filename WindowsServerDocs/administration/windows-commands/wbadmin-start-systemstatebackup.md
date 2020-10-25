---
title: wbadmin start systemstatebackup
description: Artículo de referencia para el comando Wbadmin Start systemstatebackup, que crea una copia de seguridad del estado del sistema del equipo local y la almacena en la ubicación especificada.
ms.topic: reference
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: afdfb3f4a52ae0f5897517f8d59069bea820bc4a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524730"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup

Crea una copia de seguridad del estado del sistema del equipo local y la almacena en la ubicación especificada.

Para realizar una copia de seguridad del estado del sistema mediante este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza una copia de seguridad ni recupera subárboles de usuario del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

## <a name="syntax"></a>Sintaxis

```
wbadmin start systemstatebackup -backupTarget:<VolumeName> [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -backupTarget | Especifica la ubicación en la que desea almacenar la copia de seguridad. La ubicación de almacenamiento requiere una letra de unidad o un volumen basado en GUID con el formato: `\\?\Volume{*GUID*}` . Use el comando `-backuptarget:\\servername\sharedfolder\` para almacenar copias de seguridad del estado del sistema. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para crear una copia de seguridad del estado del sistema y almacenarla en el volumen f, escriba:

```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBBackup](/powershell/module/windowserverbackup/Start-WBBackup)
