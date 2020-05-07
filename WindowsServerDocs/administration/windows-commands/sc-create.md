---
title: Creación de SC. exe
description: Obtenga información acerca de cómo registrar nuevos servicios con Windows Service Manager mediante la utilidad SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e1a581273def291502bf01e3fc9acf0c296707b
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850106"
---
# <a name="scexe-create"></a>Creación de SC. exe

Crea una subclave y entradas para un servicio en el registro y en la base de datos del administrador de control de servicios.

## <a name="syntax"></a>Sintaxis

```
sc.exe [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDeServidor>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por \\ \\ejemplo, mi Server). Para ejecutar SC. exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|Type = {Own \| share \| kernel \| files \| REC \| interactúan Type = \| {Own share}}|Especifica el tipo de servicio. La configuración predeterminada es **Type = Own**.</br>**propietario** : especifica que el servicio se ejecuta en su propio proceso. No comparte un archivo ejecutable con otros servicios. Esta es la configuración predeterminada.</br>**share** : especifica que el servicio se ejecuta como un proceso compartido. Comparte un archivo ejecutable con otros servicios.</br>**kernel** : especifica un controlador.</br>**files** : especifica un controlador del sistema de archivos.</br>**rec** : especifica un controlador reconocido del sistema de archivos (identifica los sistemas de archivos usados en el equipo).</br>**interactuar** : especifica que el servicio puede interactuar con el escritorio y recibir la entrada de los usuarios. Los servicios interactivos deben ejecutarse con la cuenta LocalSystem. Este tipo se debe usar junto con **Type = Own** o **Type = Shared**. El uso de **Type = interactúe** por sí mismo generará un error de parámetro no válido.|
|Start \| = {repeticiones \| \| \| automáticas del sistema \| de arranque deshabilitada-auto diferida}|Especifica el tipo de inicio para el servicio. La configuración predeterminada es **Start = Demand**.</br>**arranque** : especifica un controlador de dispositivo que carga el cargador de arranque.</br>**sistema** : especifica un controlador de dispositivo que se inicia durante la inicialización del kernel.</br>**auto** -especifica un servicio que se inicia automáticamente cada vez que se reinicia el equipo. Tenga en cuenta que el servicio se ejecuta incluso si no hay uno que inicie sesión en el equipo.</br>**Demand** : especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si no se especifica **Start =** .</br>**Disabled** : especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a otro valor.</br>**Delay-auto** -especifica un servicio que se inicia automáticamente un breve período de tiempo después de que se inicien otros servicios automáticos.|
|error = {normal \| grave \| crítico \| ignore}|Especifica la gravedad del error si se produce un error en el servicio cuando se inicia el equipo. La configuración predeterminada es **error = normal**.</br>**normal** : especifica que el error se registra. Aparece un cuadro de mensaje que informa al usuario de que se ha producido un error al iniciar un servicio. El inicio continuará. Esta es la configuración predeterminada.</br>**grave** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Esto podría dar lugar a que el equipo se reinicie, pero es posible que el servicio no se pueda ejecutar.</br>**Critical** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Si se produce un error en la última configuración válida conocida, también se produce un error de inicio y el proceso de arranque se detiene con un error de detención.</br>**omitir** : especifica que el error se ha registrado y el inicio continúa. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.|
|Ruta de \<ruta = BinaryPathName>|Especifica una ruta de acceso al archivo binario del servicio. No hay ningún valor predeterminado para la **ruta de ruta =** y se debe proporcionar esta cadena.|
|Grupo = \<LoadOrderGroup>|Especifica el nombre del grupo del que es miembro este servicio. La lista de grupos se almacena en el registro en la subclave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . El valor predeterminado es null.|
|etiqueta = {sí \| no}|Especifica si se va a obtener un TagID a partir de la llamada a CreateService. Las etiquetas solo se usan para los controladores de inicio de arranque y de inicio del sistema.|
|depend = \<dependencias>|Especifica los nombres de los servicios o grupos que deben iniciarse antes de que se inicie este servicio. Los nombres se separan mediante barras diagonales (/).|
|obj = {\<AccountName> \| \<objectname>}|Especifica el nombre de una cuenta en la que se ejecutará un servicio o especifica el nombre del objeto del controlador de Windows en el que se ejecutará el controlador.|
|DisplayName = \<DisplayName>|Especifica un nombre descriptivo que pueden usar los programas de la interfaz de usuario para identificar el servicio.|
|Password = \<contraseña>|Especifica una contraseña. Esto es necesario si se utiliza una cuenta que no sea LocalSystem.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para cada opción de línea de comandos, el signo igual es parte del nombre de la opción.
-   Se requiere un espacio entre una opción y su valor (por ejemplo, **Type = Own**. Si se omite el espacio, se producirá un error en la operación.

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo se puede usar el comando **SC. exe Create** :
```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
