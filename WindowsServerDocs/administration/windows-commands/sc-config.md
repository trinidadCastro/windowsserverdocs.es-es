---
title: Sc.exe configuración
description: Obtenga información acerca de cómo cambiar las configuraciones de servicio mediante la utilidad sc.exe
ms.topic: reference
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 06/05/2018
ms.openlocfilehash: 55432910455896434a1857d17016519bedb51caf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637133"
---
# <a name="scexe-config"></a>Sc.exe configuración

Modifica el valor de las entradas de un servicio en el registro y en la base de datos del administrador de control de servicios.

## <a name="syntax"></a>Sintaxis

```
sc.exe [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, mi \\ \\ Server). Para ejecutar SC.exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|Type = {Own \| share \| kernel \| files \| de \| \| tipo interactúad Type = {Own \| share}} | Especifica el tipo de servicio.</br>**propietario** : especifica un servicio que se ejecuta en su propio proceso. No comparte un archivo ejecutable con otros servicios. Este es el valor predeterminado.</br>**compartir** : especifica un servicio que se ejecuta como un proceso compartido. Comparte un archivo ejecutable con otros servicios.</br>**kernel** : especifica un controlador.</br>**files** : especifica un controlador del sistema de archivos.</br>**rec** : especifica un controlador reconocido por el sistema de archivos que identifica los sistemas de archivos usados en el equipo.</br>**adaptar** : especifica un controlador de adaptador que identifica los dispositivos de hardware, como teclados, ratones y unidades de disco.</br>**interactuar** : especifica un servicio que puede interactuar con el escritorio y recibir la entrada de los usuarios. Los servicios interactivos deben ejecutarse con la cuenta LocalSystem. Este tipo debe usarse junto con **Type = Own** o **Type = Shared** (por ejemplo, **Type = interactúe** **Type = Own**). El uso de **Type = interactúe** por sí mismo generará un error.|
|Start = { \| \| repeticiones automáticas del sistema \| de arranque \| deshabilitada \| -auto diferida}|Especifica el tipo de inicio para el servicio.</br>**arranque** : especifica un controlador de dispositivo que carga el cargador de arranque.</br>**sistema** : especifica un controlador de dispositivo que se inicia durante la inicialización del kernel.</br>**auto** -especifica un servicio que se inicia automáticamente cada vez que el equipo se reinicia y se ejecuta incluso si no hay un inicio de sesión en el equipo.</br>**Demand** : especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si no se especifica **Start =** .</br>**Disabled** : especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a otro valor.</br>**Delay-auto** -especifica un servicio que se inicia automáticamente un breve período de tiempo después de que se inicien otros servicios automáticos.|
|error = {normal \| grave \| crítico \| ignore}|Especifica la gravedad del error si el servicio no se puede iniciar en el momento del arranque.</br>**normal** : especifica que el error se ha registrado y se muestra un cuadro de mensaje que informa al usuario de que se ha producido un error al iniciar un servicio. El inicio continuará. Ésta es la configuración predeterminada.</br>**grave** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Esto podría dar lugar a que el equipo se reinicie, pero es posible que el servicio no se pueda ejecutar.</br>**Critical** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Si se produce un error en la última configuración válida conocida, también se produce un error de inicio y el proceso de arranque se detiene con un error de detención.</br>**omitir** : especifica que el error se ha registrado y el inicio continúa. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.|
|Ruta de ruta = \<BinaryPathName>|Especifica una ruta de acceso al archivo binario del servicio.|
|Grupo = \<LoadOrderGroup>|Especifica el nombre del grupo del que es miembro este servicio. La lista de grupos se almacena en el registro, en la subclave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . El valor predeterminado es null.|
|etiqueta = {sí \| no}|Especifica si se debe obtener o no un TagID de la llamada a CreateService. Las etiquetas solo se usan para los controladores de inicio de arranque y de inicio del sistema.|
|depende de \<dependencies>|Especifica los nombres de los servicios o grupos que deben iniciarse antes que este servicio. Los nombres se separan mediante barras diagonales (/).|
|obj = { \<AccountName> \| \<ObjectName> }|Especifica el nombre de una cuenta en la que se ejecutará un servicio o especifica el nombre del objeto del controlador de Windows en el que se ejecutará el controlador. La configuración predeterminada es **LocalSystem**.|
|DisplayName = \<DisplayName>|Especifica un nombre descriptivo para identificar el servicio en los programas de la interfaz de usuario. Por ejemplo, el nombre de la subclave de un servicio determinado es **wuauserv**, que tiene un nombre para mostrar más descriptivo de actualizaciones automáticas.|
|contraseña = \<Password>|Especifica una contraseña. Esto es necesario si se usa una cuenta distinta de la cuenta LocalSystem.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para cada opción de línea de comandos (parámetro), el signo igual es parte del nombre de la opción.
-   Se requiere un espacio entre una opción y su valor (por ejemplo, **Type = Own**. Si se omite el espacio, se producirá un error en la operación.

## <a name="examples"></a>Ejemplos

Para especificar una ruta de acceso binaria para el servicio NEWSERVICE, escriba:
```
sc.exe config NewService binpath= ntsd -d c:\windows\system32\NewServ.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
