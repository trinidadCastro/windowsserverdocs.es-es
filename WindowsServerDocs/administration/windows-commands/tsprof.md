---
title: tsprof
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 77d0752f74d2f6031f83f805273650747d24cfee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392317"
---
# <a name="tsprof"></a>tsprof

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
La Servicios de Escritorio remoto información de configuración de usuario se muestra en las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.

**tsprof** también puede establecer la ruta de acceso del perfil para un usuario.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Update|Actualiza la información de la ruta de acceso del perfil para <*nombre de usuario*> en dominio <*DomainName*> para <*Profilepath*>.|
|/Domain: @no__t 0DomainName >|Especifica el nombre del dominio en el que se aplica la operación.|
|/local|Aplica la operación solo a cuentas de usuario locales.|
|/Profile: @no__t 0path >|Especifica la ruta de acceso del perfil tal y como se muestra en el Servicios de Escritorio remoto extensiones de usuarios y grupos locales y usuarios y equipos de Active Directory.|
|@no__t 0UserName >|Especifica el nombre del usuario para el que desea actualizar o consultar la ruta de acceso del perfil del servidor.|
|/Copy|Copia la información de configuración de usuario de \<*SourceUser*> a \<*DestinationUser*> y actualiza la información de la ruta de acceso del perfil de \<*DestinationUser*> a \<*Profilepath*>. @No__t-0*SourceUser*> y \< >*DestinationUser*deben ser locales o deben estar en el dominio \<*nombreDominio*>.|
|@no__t 0Src_usr >|Especifica el nombre del usuario del que desea copiar la información de configuración de usuario.|
|@no__t 0Dest_usr >|Especifica el nombre del usuario al que desea copiar la información de configuración de usuario.|
|/q|Muestra la ruta de acceso del perfil actual del usuario para el que desea consultar la ruta de acceso del perfil del servidor.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   El comando **tsprof** solo está disponible cuando se ha instalado el servicio de rol Terminal Server en un equipo que ejecuta windows Server 2008 o el servicio de rol host de sesión de escritorio remoto en un equipo que ejecuta windows Server 2008 R2.

## <a name="BKMK_examples"></a>Example
-   Para copiar la información de configuración de usuario de LocalUser1 a LocalUser2, escriba:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Para establecer la ruta de acceso del perfil de Servicios de Escritorio remoto para LocalUser1 en un directorio denominado "c:\Profiles", escriba:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[servicios de escritorio remoto &#40;referencia de comandos Terminal Services&#41; ](remote-desktop-services-terminal-services-command-reference.md)
