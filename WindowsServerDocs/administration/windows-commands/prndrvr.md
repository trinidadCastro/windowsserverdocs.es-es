---
title: prndrvr
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05e03a4b0b5d686c8fbd1646a775c7f0e5c95706
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372120"
---
# <a name="prndrvr"></a>prndrvr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use el comando **prndrvr** para agregar, eliminar y enumerar controladores de impresora.

## <a name="syntax"></a>Sintaxis
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|-a|Instala un controlador.|
|-d|elimina un controlador.|
|-l|enumera todos los controladores de impresora instalados en el servidor especificado por el parámetro **-s** . Si no especifica un servidor, Windows enumera los controladores de impresora instalados en el equipo local.|
|-x|elimina todos los controladores de impresora y controladores de impresora adicionales no utilizados por una impresora lógica en el servidor especificado por el parámetro **-s** . Si no especifica un servidor para quitar de la lista, Windows eliminará todos los controladores de impresora no usados en el equipo local.|
|-m \<DrivermodelName @ no__t-1|Especifica (por nombre) el controlador que desea instalar. Los controladores a menudo se denominan para el modelo de impresora que admiten. Consulte la documentación de la impresora para obtener más información.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Especifica la versión del controlador que desea instalar. Vea la descripción del parámetro **-e**para obtener información sobre qué versiones están disponibles para cada entorno. Si no especifica una versión, se instalará la versión del controlador adecuado para la versión de Windows que se ejecuta en el equipo en el que está instalando el controlador.<br /><br />-la versión **0** admite Windows 95, Windows 98 y Windows Millennium Edition.<br />-la versión **1** admite Windows NT 3,51.<br />-la versión **2** admite Windows NT 4,0.<br />-la versión **3** es compatible con los sistemas operativos Windows Vista, Windows XP, Windows 2000 y windows Server 2003. Tenga en cuenta que esta es la única versión del controlador de impresora compatible con Windows Vista.|
|-e \<Environment >|Especifica el entorno para el controlador que desea instalar. Si no especifica un entorno, se usa el entorno del equipo en el que va a instalar el controlador. Los parámetros de entorno admitidos son los siguientes:<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows ia64"**|
|-s \<ServerName >|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|
|-u \<UserName >-w \<Password >|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione.|
|-h \<path >|Especifica la ruta de acceso al archivo del controlador. Si no especifica una ruta de acceso, se usa la ruta de acceso a la ubicación en la que se instaló Windows.|
|-i \<Filename.inf>|Especifica la ruta de acceso completa y el nombre de archivo del controlador que desea instalar. Si no especifica un nombre de archivo, el script usa uno de los archivos Printer. inf de la bandeja de entrada en el subdirectorio inf del directorio de Windows.<br /><br />Si no se especifica la ruta de acceso del controlador, el script busca archivos de controlador en el archivo driver. cab.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
- El comando **prndrvr** es un script de Visual Basic ubicado en el directorio%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prndrvr o cambie los directorios a la carpeta correspondiente.

  Por ejemplo:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).
- La opción-x elimina todos los controladores de impresora adicionales (controladores instalados para su uso en clientes que ejecutan versiones alternativas de Windows), incluso si el controlador principal está en uso. Si el componente de fax está instalado, esta opción también elimina los controladores de fax. Si no está en uso, se elimina el controlador de fax principal (es decir, si no hay ninguna cola que lo use). Si se elimina el controlador de fax principal, la única manera de volver a habilitar fax es reinstalar el componente de fax.
- Si se usa sin parámetros, **prndrvr** muestra la ayuda de la línea de comandos para el comando **prndrvr** .

## <a name="BKMK_examples"></a>Example

Para enumerar todos los controladores del servidor \\ \ Servidorimpresión1, escriba:
```
cscript Prndrvr -l -s
```

Para agregar un controlador de impresora Windows x64 de la versión 3 para el modelo de impresora "modelo de impresora láser 1" con el archivo de información del controlador C:\temp\Laserprinter1.inf para un controlador almacenado en el tipo de carpeta C:\temp:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para eliminar un controlador de impresora de Windows NT x86 de la versión 3 para "modelo de impresora láser 1", escriba:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comando de impresión](print-command-reference.md)
