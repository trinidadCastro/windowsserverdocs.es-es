---
title: Creación de SC
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7931ddc91b91d5fce01335f4b090d0305790f65c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826506"
---
# <a name="sc-create"></a>Creación de SC



Crea una subclave y entradas para un servicio en el registro y en la base de datos de administrador de Control de servicio.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe tener el formato de convención de nomenclatura Universal (UNC) (por ejemplo, \\ \\myserver). Para ejecutar SC.exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre del servicio devolviendo por la **getkeyname** operación.|
|tipo = {propio \| compartir \| kernel \| filesys \| rec \| interactuar tipo = {propio \| compartir}}|Especifica el tipo de servicio. El valor predeterminado es **tipo = propio**.</br>**propio** : Especifica que el servicio se ejecuta en su propio proceso. No se comparte un archivo ejecutable con otros servicios. Esta es la configuración predeterminada.</br>**compartir** : Especifica que el servicio se ejecuta como un proceso compartido. Un archivo ejecutable comparte con otros servicios.</br>**kernel** -especifica un controlador.</br>**filesys** -especifica un controlador de sistema de archivos.</br>**Rec** -especifica un controlador reconocido el sistema de archivos (identifica los sistemas de archivos que se usa en el equipo).</br>**interactuar** : Especifica que el servicio puede interactuar con el escritorio, recibir datos de entrada a los usuarios. Servicios interactivos se deben ejecutar bajo la cuenta LocalSystem. Este tipo debe usarse junto con **tipo = propio** o **tipo = compartido**. Uso de **tipo = interactuar** por sí mismo, se generará un error de "parámetro no válido".|
|inicio = {arranque \| sistema \| automática \| demanda \| deshabilitado}|Especifica el tipo de inicio para el servicio. El valor predeterminado es **iniciar = a petición**.</br>**arranque** -especifica un controlador de dispositivo que es cargado por el cargador de arranque.</br>**sistema** -especifica un controlador de dispositivo que se inicia durante la inicialización del núcleo.</br>**Auto** -especifica un servicio que se inicia automáticamente cada vez que se reinicie el equipo. Tenga en cuenta que el servicio se ejecuta incluso si nadie inicia sesión en el equipo.</br>**demanda** -especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si **iniciar =** no se especifica.</br>**deshabilitado** -especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a algún otro valor.|
|error = {normal \| graves \| críticas \| omitir}|Especifica la gravedad del error si se produce un error en el servicio cuando se inicia el equipo. El valor predeterminado es **error = normal**.</br>**normal** : Especifica que se registra el error. Se muestra un cuadro de mensaje, informa al usuario que se ha podido iniciar un servicio. Inicio continuará. Esta es la configuración predeterminada.</br>**grave** : Especifica que se registra el error (si es posible). El equipo intenta reiniciar con la configuración válida conocida de la última. Esto podría resultar en el equipo se pueda reiniciar, pero puede no se puede ejecutar el servicio.</br>**crítica** : Especifica que se registra el error (si es posible). El equipo intenta reiniciar con la configuración válida conocida de la última. Si se produce un error en la configuración válida conocida último, también se produce un error en Inicio y el proceso de arranque se detiene con un error de detención.</br>**omitir** : Especifica que se registra el error y continúa el inicio. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.|
|binpath= \<BinaryPathName>|Especifica una ruta de acceso al archivo binario del servicio. No hay ningún valor predeterminado para **binpath =**, y se debe proporcionar esta cadena.|
|group= \<LoadOrderGroup>|Especifica el nombre del grupo del que este servicio es un miembro. La lista de grupos se almacena en el registro en el **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** subclave. El valor predeterminado es null.|
|etiqueta = {Sí \| no}|Especifica si un elemento TagID se va a obtenerse de la llamada de CreateService. Las etiquetas se usan únicamente para los controladores de inicio y de inicio del sistema.|
|depend= \<dependencies>|Especifica los nombres de servicios o grupos que deben iniciarse antes de este servicio. Los nombres están separados por barras diagonales (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Especifica el nombre de una cuenta en el que se ejecutará un servicio, o especifica un nombre del objeto de controlador de Windows en el que se ejecutará el controlador.|
|displayname= \<DisplayName>|Especifica un nombre descriptivo que se puede utilizar con programas de la interfaz de usuario para identificar el servicio.|
|password= \<Password>|Especifica una contraseña. Esto es necesario si se usa una cuenta distinta de LocalSystem.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cada opción de línea de comandos, el signo de igual forma parte de nombre de la opción.
-   Se requiere un espacio entre una opción y su valor (por ejemplo, **tipo = propio**. Si se omite el espacio de la operación dará error.

## <a name="BKMK_examples"></a>Ejemplos

Los ejemplos siguientes muestran cómo puede usar el **crear sc** comando:
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
