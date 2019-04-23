---
title: cmstp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ee5c5189b4eab21994def221dd757b0061ef22d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836386"
---
# <a name="cmstp"></a>cmstp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o quita un perfil de servicio de administrador de conexiones. Si se utiliza sin parámetros opcionales, **cmstp** instala un perfil de servicio con la configuración predeterminada adecuada para el sistema operativo y los permisos del usuario. 
## <a name="syntax"></a>Sintaxis
Sintaxis 1:
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
Sintaxis 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|< ServiceProfileFileName >.exe|Especifica el nombre, el paquete de instalación que contiene el perfil que desea instalar.<br /><br />Necesaria para la sintaxis 1, pero no es válido para la sintaxis 2.|
|/q:a|Especifica que el perfil debe instalarse sin preguntar al usuario. Seguirá apareciendo el mensaje de comprobación que se ha realizado correctamente la instalación.<br /><br />Necesaria para la sintaxis 1, pero no es válido para la sintaxis 2.|
|[Unidad:] [ruta] <ServiceProfileFileName>.inf|Obligatorio. Especifica el nombre, el archivo de configuración que determina cómo se debe instalar el perfil.<br /><br />El [unidad:] [ruta] parámetro no es válido para la sintaxis 1.|
|/nf|Especifica que no se deberían instalar los archivos de compatibilidad.|
|/ni|Especifica que no se debe crear un icono del escritorio. Este parámetro solo es válido para los equipos que ejecutan Windows 95, Windows 98, Windows NT 4.0 o Windows Millennium edition.|
|/ns|Especifica que no se debe crear un acceso directo del escritorio. Este parámetro solo es válido para los equipos que ejecutan a un miembro de la familia Windows Server 2003, Windows 2000 o Windows XP.|
|/s|Especifica que el perfil de servicio se debe instalar o desinstalar en modo silencioso (sin pedir respuesta del usuario ni mostrar mensajes de confirmación).|
|/su|Especifica que se debe instalar el perfil de servicio para un único usuario en lugar de todos los usuarios. Este parámetro solo es válido para los equipos que ejecutan un Windows Server 2003, Windows 2000 o Windows XP.|
|/au|Especifica que se debe instalar el perfil de servicio para todos los usuarios. Este parámetro solo es válido para los equipos que ejecutan Windows Server 2003, Windows 2000 o Windows XP.|
|/u|Especifica que se debe desinstalar el perfil de servicio.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
**/s** es el único parámetro que puede usar en combinación con **/u**.
La sintaxis 1 es la típica utilizado en una aplicación de instalación personalizada. Para utilizar esta sintaxis, se debe ejecutar **cmstp** desde el directorio que contiene el <ServiceProfileFileName>archivo .exe.
## <a name="BKMK_Examples"></a>Ejemplos
Para instalar el perfil de servicio ficción sin los archivos de compatibilidad, escriba:
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
Para instalar el perfil de servicio ficción para un único usuario de forma silenciosa, escriba:
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
Para desinstalar el perfil de servicio Ficción de forma silenciosa, escriba:
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
