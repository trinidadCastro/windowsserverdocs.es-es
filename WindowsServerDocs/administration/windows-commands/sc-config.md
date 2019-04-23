---
title: Sc config
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 361a8407aae3b5e823b58cd71b97b043159146e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837226"
---
# <a name="sc-config"></a>Sc config



Modifica el valor de una entrada del servicio en el registro y en la base de datos de administrador de Control de servicio.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe tener el formato de convención de nomenclatura Universal (UNC) (por ejemplo, \\ \\myserver). Para ejecutar SC.exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre del servicio devolviendo por la **getkeyname** operación.|
|tipo = {propio\| compartir \| kernel \| filesys \| rec \| adaptar \| interactuar tipo = {propio \| compartir}} | Especifica el tipo de servicio.</br>**propio** -especifica un servicio que se ejecuta en su propio proceso. No se comparte un archivo ejecutable con otros servicios. Este es el valor predeterminado.</br>**compartir** -especifica un servicio que se ejecuta como un proceso compartido. Un archivo ejecutable comparte con otros servicios.</br>**kernel** -especifica un controlador.</br>**filesys** -especifica un controlador de sistema de archivos.</br>**Rec** -especifica un controlador reconocidas por el sistema de archivos que identifica los sistemas de archivos que se usa en el equipo.</br>**adaptar** : especifica un controlador de adaptador que identifica los dispositivos de hardware, como teclados, mouse, y unidades de disco.</br>**interactuar** -especifica un servicio que puede interactuar con el escritorio, recibir datos de entrada a los usuarios. Servicios interactivos se deben ejecutar bajo la cuenta LocalSystem. Este tipo debe usarse junto con **tipo = propio** o **tipo = compartido** (por ejemplo, **tipo = interactuar** **tipo = propio**). Uso de **tipo = interactuar** por sí mismo, se generará un error.|
|inicio = {arranque \| sistema \| automática \| demanda \| deshabilitado \| automático retrasado}|Especifica el tipo de inicio para el servicio.</br>**arranque** -especifica un controlador de dispositivo que es cargado por el cargador de arranque.</br>**sistema** -especifica un controlador de dispositivo que se inicia durante la inicialización del núcleo.</br>**Auto** : especifica un servicio que automáticamente se inicia cada vez que el equipo se reinicia y se ejecuta incluso si nadie inicia sesión en el equipo.</br>**demanda** -especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si **iniciar =** no se especifica.</br>**deshabilitado** -especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a algún otro valor.</br>**retraso** -especifica un servicio que se inicia automáticamente un breve período después de haberse iniciados otros servicios automáticamente.|
|error = {normal \| graves \| críticas \| omitir}|Especifica la gravedad del error si el servicio no se inicia en tiempo de arranque.</br>**normal** : Especifica que el error se registra y se muestra un cuadro de mensaje, informa al usuario que se ha podido iniciar un servicio. Inicio continuará. Esta es la configuración predeterminada.</br>**grave** : Especifica que se registra el error (si es posible). El equipo intenta reiniciar con la configuración válida conocida de la última. Esto podría resultar en el equipo se pueda reiniciar, pero puede no se puede ejecutar el servicio.</br>**crítica** : Especifica que se registra el error (si es posible). El equipo intenta reiniciar con la configuración válida conocida de la última. Si se produce un error en la configuración válida conocida último, también se produce un error en Inicio y el proceso de arranque se detiene con un error de detención.</br>**omitir** : Especifica que se registra el error y continúa el inicio. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.|
|binpath= \<BinaryPathName>|Especifica una ruta de acceso al archivo binario del servicio.|
|group= \<LoadOrderGroup>|Especifica el nombre del grupo del que este servicio es un miembro. La lista de grupos se almacena en el registro, en el **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** subclave. El valor predeterminado es null.|
|etiqueta = {Sí \| no}|Especifica si se deben obtener un TagID de la llamada de CreateService. Las etiquetas se usan únicamente para los controladores de inicio y de inicio del sistema.|
|depend= \<dependencies>|Especifica los nombres de servicios o grupos que deben iniciarse antes de este servicio. Los nombres están separados por barras diagonales (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Especifica el nombre de una cuenta en el que se ejecutará un servicio, o especifica un nombre del objeto de controlador de Windows en el que se ejecutará el controlador. El valor predeterminado es **LocalSystem**.|
|displayname= \<DisplayName>|Especifica un nombre descriptivo para identificar el servicio en los programas de la interfaz de usuario. Por ejemplo, el nombre de la subclave de un servicio determinado es **wuauserv**, que tiene un nombre más descriptivo para mostrar de las actualizaciones automáticas.|
|password= \<Password>|Especifica una contraseña. Esto es necesario si se usa una cuenta distinta de la cuenta LocalSystem.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cada opción de línea de comandos (parámetro), el signo de igual forma parte de nombre de la opción.
-   Se requiere un espacio entre una opción y su valor (por ejemplo, **tipo = propio**. Si se omite el espacio, se producirá un error en la operación.

## <a name="BKMK_examples"></a>Ejemplos

Para especificar una ruta de acceso binaria para el servicio NUEVOSERVICIO, escriba:
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)