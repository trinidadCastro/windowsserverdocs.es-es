---
title: tsprof
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836076"
---
# <a name="tsprof"></a>tsprof

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia la información de configuración de usuario de servicios de escritorio remoto de un usuario a otro.
La información de configuración de usuario de servicios de escritorio remoto se muestra en las extensiones de servicios de escritorio remoto a los usuarios locales y grupos y active directory a los usuarios y equipos.

**tsprof** también se puede establecer la ruta de acceso de perfil para un usuario.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/update|Información de ruta de acceso para generar perfiles de las actualizaciones <*UserName*> en el dominio <*DomainName*> a <*rutaAccesoPerfil*>.|
|/ Domain:\<DomainName >|Especifica el nombre del dominio en el que se aplica la operación.|
|/local|Se aplica la operación únicamente a cuentas de usuario local.|
|/profile:\<path>|Especifica la ruta de acceso de perfil tal como se muestra en las extensiones de servicios de escritorio remoto en usuarios locales y grupos y active directory a los usuarios y equipos.|
|\<UserName>|Especifica el nombre del usuario para los que desea actualizar o consultar la ruta de acceso del perfil de servidor.|
|/Copy|Copia la información de configuración de usuario de \< *SourceUser*> a \< *DestinationUser*> y actualiza la información de ruta de acceso de perfil de \<  *DestinationUser*> a \< *rutaAccesoPerfil*>. Ambos \< *SourceUser*> y \< *DestinationUser*> debe ser local o debe estar en el dominio \< *DomainName*> .|
|\<Src_usr>|Especifica el nombre del usuario del que desea copiar la información de configuración de usuario.|
|\<Dest_usr>|Especifica el nombre del usuario a los que van a copiar la información de configuración de usuario.|
|/q|Muestra la ruta de acceso de perfil actual del usuario para el que desee consultar la ruta de acceso del perfil de servidor.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   El **tsprof** comando solo está disponible cuando se ha instalado el servicio de rol Terminal Server en un equipo que ejecuta el servicio de rol Host de sesión de escritorio remoto o de Windows Server 2008 en un equipo que ejecuta Windows Server 2008 R2.

## <a name="BKMK_examples"></a>Ejemplos
-   Para copiar información de configuración de usuario de LocalUser1 a LocalUser2, escriba:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Para establecer la ruta de acceso del perfil de servicios de escritorio remoto para LocalUser1 en un directorio llamado "c:\profiles", escriba:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
