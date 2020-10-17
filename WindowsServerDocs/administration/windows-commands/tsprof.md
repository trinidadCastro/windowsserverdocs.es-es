---
title: tsprof
description: Artículo de referencia para el comando tsprof, que copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
ms.topic: reference
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e18f736a31af3cc649d4921197710f713e7df6af
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156340"
---
# <a name="tsprof"></a>tsprof

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro. La Servicios de Escritorio remoto información de configuración de usuario aparece en las extensiones de Servicios de Escritorio remoto para usuarios y grupos locales y usuarios y equipos de Active Directory.

> [!NOTE]
> También puede usar el [comando tsprof](tsprof.md) para establecer la ruta de acceso del perfil para un usuario.
>
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
tsprof /update {/domain:<Domainname> | /local} /profile:<path> <username>
tsprof /copy {/domain:<Domainname> | /local} [/profile:<path>] <src_user> <dest_user>
tsprof /q {/domain:<Domainname> | /local} <username>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /update | Actualiza la información de la ruta de acceso del perfil de `<username>` en el dominio `<domainname>` a `<profilepath>` . |
| /Domain`<Domainname>` | Especifica el nombre del dominio en el que se aplica la operación. |
| /local | Aplica la operación solo a cuentas de usuario locales. |
| /Profile`<path>` | Especifica la ruta de acceso del perfil tal y como se muestra en el Servicios de Escritorio remoto extensiones de usuarios y grupos locales y usuarios y equipos de Active Directory. |
| `<username>` | Especifica el nombre del usuario para el que desea actualizar o consultar la ruta de acceso del perfil del servidor. |
| /copy | Copia la información de configuración de usuario de `<src_user>` a `<dest_user>` y actualiza la información de la ruta de acceso del perfil para `<dest_user>` a `<profilepath>` . `<src_user>`Y `<dest_user>` deben ser locales o deben estar en el dominio `<domainname>` . |
| `<src_user>` | Especifica el nombre del usuario del que desea copiar la información de configuración de usuario. También se conoce como el usuario de origen. |
| `<dest_user>` | Especifica el nombre del usuario al que desea copiar la información de configuración de usuario. También se conoce como usuario de destino. |
| /q | Muestra la ruta de acceso del perfil actual del usuario para el que desea consultar la ruta de acceso del perfil del servidor. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para copiar la información de configuración de usuario de *LocalUser1* a *LocalUser2*, escriba:

```
tsprof /copy /local LocalUser1 LocalUser2
```

Para establecer la ruta de acceso del perfil de Servicios de Escritorio remoto para *LocalUser1* en un directorio denominado *c:\Profiles*, escriba:

```
tsprof /update /local /profile:c:\profiles LocalUser1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
