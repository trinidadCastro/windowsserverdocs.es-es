---
title: tsprof
description: Tema de referencia para tsprof, que copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5a4980455eb2901db949a06f0c6dfec9ecf5793
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721232"
---
# <a name="tsprof"></a>tsprof

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.
La Servicios de Escritorio remoto información de configuración de usuario se muestra en las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.

**tsprof** también puede establecer la ruta de acceso del perfil para un usuario.



> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

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
|/Domain:\<nombreDeDominio>|Especifica el nombre del dominio en el que se aplica la operación.|
|/local|Aplica la operación solo a cuentas de usuario locales.|
|/Profile:\<ruta de acceso>|Especifica la ruta de acceso del perfil tal y como se muestra en el Servicios de Escritorio remoto extensiones de usuarios y grupos locales y usuarios y equipos de Active Directory.|
|\<Nombre de usuario>|Especifica el nombre del usuario para el que desea actualizar o consultar la ruta de acceso del perfil del servidor.|
|/copy|Copia la información de configuración \<de usuario de *SourceUser*> a \< *DestinationUser*> y actualiza la información \<de la ruta de acceso del perfil para *DestinationUser*> a \< *Profilepath*>. Tanto \< *SourceUser*> como \<> *DestinationUser* deben ser locales o deben estar en el dominio \< *domainname*>.|
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
- [Referencia de comandos de servicios de escritorio remoto de la clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[(Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
