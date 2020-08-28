---
title: tsprof
description: Artículo de referencia para tsprof, que copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
ms.topic: reference
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f676b1d11586d413e544d451043da242861083e1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023399"
---
# <a name="tsprof"></a>tsprof

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
La Servicios de Escritorio remoto información de configuración de usuario se muestra en las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.

**tsprof** también puede establecer la ruta de acceso del perfil para un usuario.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/update|Actualiza la información de la ruta de acceso del perfil para <*nombre de usuario*> en dominio <*DomainName*> para <*Profilepath*>.|
|/Domain\<DomainName>|Especifica el nombre del dominio en el que se aplica la operación.|
|/local|Aplica la operación solo a cuentas de usuario locales.|
|/Profile\<path>|Especifica la ruta de acceso del perfil tal y como se muestra en el Servicios de Escritorio remoto extensiones de usuarios y grupos locales y usuarios y equipos de Active Directory.|
|\<UserName>|Especifica el nombre del usuario para el que desea actualizar o consultar la ruta de acceso del perfil del servidor.|
|/copy|Copia la información de configuración de usuario de \<*SourceUser*> a \<*DestinationUser*> y actualiza la información de la ruta de acceso del perfil para \<*DestinationUser*> a \<*Profilepath*> . \<*SourceUser*>Y \<*DestinationUser*> deben ser locales o deben estar en el dominio \<*DomainName*> .|
|\<Src_usr>|Especifica el nombre del usuario del que desea copiar la información de configuración de usuario.|
|\<Dest_usr>|Especifica el nombre del usuario al que desea copiar la información de configuración de usuario.|
|/q|Muestra la ruta de acceso del perfil actual del usuario para el que desea consultar la ruta de acceso del perfil del servidor.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   El comando **tsprof** solo está disponible cuando se ha instalado el servicio de rol Terminal Server en un equipo que ejecuta windows Server 2008 o el servicio de rol host de sesión de escritorio remoto en un equipo que ejecuta windows Server 2008 R2.

## <a name="examples"></a>Ejemplos
-   Para copiar la información de configuración de usuario de LocalUser1 a LocalUser2, escriba:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Para establecer la ruta de acceso del perfil de Servicios de Escritorio remoto para LocalUser1 en un directorio denominado c:\Profiles, escriba:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Referencia de comandos de servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
