---
title: Creación de SC
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8ea8f1c33472b7ac95ec0282a50d902a9d7cf84d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384375"
---
# <a name="sc-create"></a>Creación de SC



Crea una subclave y entradas para un servicio en el registro y en la base de datos del administrador de control de servicios.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0ServerName >|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, \\ @ no__t-1myserver). Para ejecutar SC. exe localmente, omita este parámetro.|
|@no__t 0ServiceName >|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|Type = {Own \| share \| kernel \| files \| REC \| tipo de interacción = {propietario \| recurso compartido}}|Especifica el tipo de servicio. La configuración predeterminada es **Type = Own**.</br>**propietario** : especifica que el servicio se ejecuta en su propio proceso. No comparte un archivo ejecutable con otros servicios. Esta es la configuración predeterminada.</br>**share** : especifica que el servicio se ejecuta como un proceso compartido. Comparte un archivo ejecutable con otros servicios.</br>**kernel** : especifica un controlador.</br>**files** : especifica un controlador del sistema de archivos.</br>**rec** : especifica un controlador reconocido del sistema de archivos (identifica los sistemas de archivos usados en el equipo).</br>**interactuar** : especifica que el servicio puede interactuar con el escritorio y recibir la entrada de los usuarios. Los servicios interactivos deben ejecutarse con la cuenta LocalSystem. Este tipo se debe usar junto con **Type = Own** o **Type = Shared**. El uso de **Type = interactúe** por sí mismo generará un error de "parámetro no válido".|
|Start = {boot \| System \| auto \| Demand \| Disabled}|Especifica el tipo de inicio para el servicio. La configuración predeterminada es **Start = Demand**.</br>**arranque** : especifica un controlador de dispositivo que carga el cargador de arranque.</br>**sistema** : especifica un controlador de dispositivo que se inicia durante la inicialización del kernel.</br>**auto** -especifica un servicio que se inicia automáticamente cada vez que se reinicia el equipo. Tenga en cuenta que el servicio se ejecuta incluso si no hay uno que inicie sesión en el equipo.</br>**Demand** : especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si no se especifica **Start =** .</br>**Disabled** : especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a otro valor.|
|error = {normal \| grave \| crítico \| ignore}|Especifica la gravedad del error si se produce un error en el servicio cuando se inicia el equipo. La configuración predeterminada es **error = normal**.</br>**normal** : especifica que el error se registra. Aparece un cuadro de mensaje que informa al usuario de que se ha producido un error al iniciar un servicio. El inicio continuará. Esta es la configuración predeterminada.</br>**grave** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Esto podría dar lugar a que el equipo se reinicie, pero es posible que el servicio no se pueda ejecutar.</br>**Critical** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Si se produce un error en la última configuración válida conocida, también se produce un error de inicio y el proceso de arranque se detiene con un error de detención.</br>**omitir** : especifica que el error se ha registrado y el inicio continúa. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.|
|Ruta de la ruta = \<BinaryPathName >|Especifica una ruta de acceso al archivo binario del servicio. No hay ningún valor predeterminado para la **ruta de ruta =** y se debe proporcionar esta cadena.|
|Grupo = \<LoadOrderGroup >|Especifica el nombre del grupo del que es miembro este servicio. La lista de grupos se almacena en el registro en la subclave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . El valor predeterminado es NULL.|
|etiqueta = {Yes \| no}|Especifica si se va a obtener un TagID a partir de la llamada a CreateService. Las etiquetas solo se usan para los controladores de inicio de arranque y de inicio del sistema.|
|depend = \<dependencies >|Especifica los nombres de los servicios o grupos que deben iniciarse antes de que se inicie este servicio. Los nombres se separan mediante barras diagonales (/).|
|obj = {\<AccountName > \| \<ObjectName >}|Especifica el nombre de una cuenta en la que se ejecutará un servicio o especifica el nombre del objeto del controlador de Windows en el que se ejecutará el controlador.|
|DisplayName = \<DisplayName >|Especifica un nombre descriptivo que pueden usar los programas de la interfaz de usuario para identificar el servicio.|
|Password = \<Password >|Especifica una contraseña. Esto es necesario si se utiliza una cuenta que no sea LocalSystem.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cada opción de línea de comandos, el signo igual es parte del nombre de la opción.
-   Se requiere un espacio entre una opción y su valor (por ejemplo, **Type = Own**. Si se omite el espacio, se producirá un error en la operación.

## <a name="BKMK_examples"></a>Example

En los siguientes ejemplos se muestra cómo se puede usar el comando **SC Create** :
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
