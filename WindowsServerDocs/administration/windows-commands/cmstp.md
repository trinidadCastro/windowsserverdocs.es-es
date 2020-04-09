---
title: cmstp
description: Temas de comandos de Windows para cmstp, que instala o quita un perfil de servicio de administrador de conexiones.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8516cbce34fb8129aa623b75f5c295a7b8c28331
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847508"
---
# <a name="cmstp"></a>cmstp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o quita un perfil de servicio de administrador de conexiones. Si se usa sin parámetros opcionales, **cmstp** instala un perfil de servicio con la configuración predeterminada adecuada para el sistema operativo y los permisos del usuario. 

## <a name="syntax"></a>Sintaxis
Sintaxis 1:
```
ServiceProfileFileName .exe /q:a /c:cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]
```
Sintaxis 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|< NombreArchivoPerfilServicio >. exe|Especifica, por nombre, el paquete de instalación que contiene el perfil que desea instalar.<p>Se requiere para la sintaxis 1, pero no es válida para la sintaxis 2.|
|/q:a|Especifica que el perfil debe instalarse sin preguntar al usuario. El mensaje de comprobación que indica que la instalación se ha realizado correctamente seguirá apareciendo.<p>Se requiere para la sintaxis 1, pero no es válida para la sintaxis 2.|
|[Unidad:] [rutaDeAcceso] <ServiceProfileFileName>. inf|Obligatorio. Especifica, por nombre, el archivo de configuración que determina cómo se debe instalar el perfil.<p>El parámetro [unidad:] [rutaDeAcceso] no es válido para la sintaxis 1.|
|/NF|Especifica que no se deben instalar los archivos de compatibilidad.|
|/ni|Especifica que no se debe crear un icono de escritorio. Este parámetro solo es válido para equipos que ejecutan Windows 95, Windows 98, Windows NT 4,0 o Windows Millennium Edition.|
|/NS|Especifica que no se debe crear un acceso directo de escritorio. Este parámetro solo es válido para equipos que ejecutan un miembro de la familia de Windows Server 2003, Windows 2000 o Windows XP.|
|/s|Especifica que el perfil de servicio debe instalarse o desinstalarse en modo silencioso (sin solicitar la respuesta del usuario ni mostrar el mensaje de comprobación).|
|/su|Especifica que el perfil de servicio debe instalarse para un solo usuario en lugar de para todos los usuarios. Este parámetro solo es válido para equipos que ejecutan Windows Server 2003, Windows 2000 o Windows XP.|
|/au|Especifica que el perfil de servicio debe instalarse para todos los usuarios. Este parámetro solo es válido para equipos que ejecutan Windows Server 2003, Windows 2000 o Windows XP.|
|/u|Especifica que debe desinstalarse el perfil de servicio.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
**/s** es el único parámetro que puede usar en combinación con **/u**.
La sintaxis 1 es la sintaxis típica utilizada en una aplicación de instalación personalizada. Para usar esta sintaxis, debe ejecutar **cmstp** desde el directorio que contiene el archivo <ServiceProfileFileName>. exe.

## <a name="examples"></a><a name=BKMK_Examples></a>Example
Para instalar el perfil de servicio ficción sin ningún archivo de compatibilidad, escriba:
```
fiction.exe /c:cmstp.exe fiction.inf /nf
```
Para instalar de forma silenciosa el perfil de servicio ficción para un solo usuario, escriba:
```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```
Para desinstalar de forma silenciosa el perfil del servicio ficción, escriba:
```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```
## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
