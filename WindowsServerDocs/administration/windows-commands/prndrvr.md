---
title: prndrvr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436212"
---
# <a name="prndrvr"></a>prndrvr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use la **prndrvr** comando para agregar, eliminar y enumerar los controladores de impresora.

## <a name="syntax"></a>Sintaxis
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|-a|Instala a un controlador.|
|-d|elimina un controlador.|
|-l|Enumera todos los controladores de impresora instalados en el servidor especificado por el **-s** parámetro. Si no especifica un servidor, Windows muestran los controladores de impresora instalados en el equipo local.|
|-x|Elimina todos los controladores de impresora y controladores de impresora adicionales no esté en uso por una impresora lógica en el servidor especificado por el **-s** parámetro. Si no especifica un servidor para quitar de la lista, Windows eliminan todos los controladores de impresora no utilizados en el equipo local.|
|-m \<DrivermodelName\>|Especifica el controlador que desea instalar (por nombre). A menudo se denominan controladores para el modelo de impresora compatible. Consulte la documentación de la impresora para obtener más información.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Especifica la versión del controlador que desea instalar. Vea la descripción de la **-e**parámetro para información sobre qué versiones están disponibles para cada entorno. Si no especifica una versión, se instala la versión del controlador adecuado para la versión de Windows que se ejecutan en el equipo donde está instalando el controlador.<br /><br />-versión **0** es compatible con Windows 95, Windows 98 y Windows Millennium edition.<br />-versión **1** es compatible con Windows NT 3.51.<br />-versión **2** es compatible con Windows NT 4.0.<br />-versión **3** es compatible con Windows Vista, Windows XP, Windows 2000 y los sistemas operativos Windows Server 2003. Tenga en cuenta que esta es la única versión de controlador de impresora que es compatible con Windows Vista.|
|-e \<entorno >|Especifica el entorno para el controlador que desea instalar. Si no especifica un entorno, se utiliza el entorno del equipo donde está instalando el controlador. Los parámetros de entorno admitidos son:<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s \<ServerName >|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|
|-u \<UserName > -w \<contraseña >|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando.|
|-h \<ruta de acceso >|Especifica la ruta de acceso al archivo del controlador. Si no especifica una ruta de acceso, se utiliza la ruta de acceso a la ubicación donde se instaló Windows.|
|-i \<archivo.inf >|Especifica la ruta de acceso y el nombre completo para el controlador que desea instalar. Si no especifica un nombre de archivo, el script utiliza uno de los archivos de bandeja de entrada impresora .inf en el subdirectorio inf del directorio de Windows.<br /><br />Si no se especifica la ruta de acceso de controlador, el script busca archivos de controlador en el archivo driver.cab.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
- El **prndrvr** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo prndrvr o cambie los directorios a la carpeta correspondiente.

  Por ejemplo:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).
- El x - opción elimina todos los controladores de impresora adicionales (controladores instalados para su uso en los clientes de las versiones alternativas de funcionamiento de Windows), incluso si el controlador principal está en uso. Si está instalado el componente de fax, esta opción también elimina los controladores de fax. Se elimina el controlador de fax principal si no está en uso (es decir, si no hay ninguna cola de mensajes con él). Si se elimina el controlador de fax principal, es la única forma de volver a habilitar el fax volver a instalar el componente de fax.
- Se utiliza sin parámetros, **prndrvr** muestra la Ayuda de línea de comandos para el **prndrvr** comando.

## <a name="BKMK_examples"></a>Ejemplos

Para enumerar todos los controladores en el \\\printServer1 server, escriba:
```
cscript Prndrvr -l -s
```

Para agregar un controlador de impresora de Windows x64 versión 3 para el modelo de "modelo de impresora láser 1" de la impresora mediante el archivo de información de controlador C:\temp\Laserprinter1.inf para un controlador que se almacenan en la carpeta C:\temp, escriba:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para eliminar un controlador de impresora de Windows NT x86 versión 3 "modelo de impresora láser 1", escriba:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
