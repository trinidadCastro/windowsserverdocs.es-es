---
title: sc.exe crear
description: Artículo de referencia para el comando sc.exe Create, que crea una subclave y entradas para un servicio en el registro y en la base de datos del administrador de control de servicios.
ms.topic: reference
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2be59f0d91abdf91984985c536e45abb3cdc0d3f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388514"
---
# <a name="scexe-create"></a>sc.exe crear

Crea una subclave y entradas para un servicio en el registro y en la base de datos del administrador de control de servicios.

## <a name="syntax"></a>Sintaxis

```
sc.exe [<servername>] create [<servicename>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <binarypathname>] [group= <loadordergroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<accountname> | <objectname>}] [displayname= <displayname>] [password= <password>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
| `<servername>` | Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, mi \\ Server). Para ejecutar SC.exe localmente, no utilice este parámetro. |
| `<servicename>` | Especifica el nombre de servicio devuelto por la operación **getkeyname** . |
| `type= {own | share | kernel | filesys | rec | interact type= {own | share}}` | Especifica el tipo de servicio. Entre estas opciones se incluyen:<ul><li>**propietario** : especifica un servicio que se ejecuta en su propio proceso. No comparte un archivo ejecutable con otros servicios. Este es el valor predeterminado.</li><li>**compartir** : especifica un servicio que se ejecuta como un proceso compartido. Comparte un archivo ejecutable con otros servicios.</li><li>**kernel** : especifica un controlador.</li><li>**files** : especifica un controlador del sistema de archivos.</li><li>**rec** : especifica un controlador reconocido por el sistema de archivos que identifica los sistemas de archivos usados en el equipo.</li><li>**interactuar** : especifica un servicio que puede interactuar con el escritorio y recibir la entrada de los usuarios. Los servicios interactivos deben ejecutarse con la cuenta LocalSystem. Este tipo debe usarse junto con **Type = Own** o **Type = Shared** (por ejemplo, **Type = interactúe** **Type = Own**). El uso de **Type = interactúe** por sí mismo generará un error.</li></ul> |
| `start= {boot | system | auto | demand | disabled | delayed-auto}` | Especifica el tipo de inicio para el servicio. Entre estas opciones se incluyen:<ul><li>**arranque** : especifica un controlador de dispositivo que carga el cargador de arranque.</li><li>**sistema** : especifica un controlador de dispositivo que se inicia durante la inicialización del kernel.</li><li>**auto** -especifica un servicio que se inicia automáticamente cada vez que el equipo se reinicia y se ejecuta incluso si no hay un inicio de sesión en el equipo.</li><li>**Demand** : especifica un servicio que se debe iniciar manualmente. Este es el valor predeterminado si no se especifica **Start =** .</li><li>**Disabled** : especifica un servicio que no se puede iniciar. Para iniciar un servicio deshabilitado, cambie el tipo de inicio a otro valor.</li><li>**Delay-auto** -especifica un servicio que se inicia automáticamente un breve período de tiempo después de que se inicien otros servicios automáticos.</li></ul> |
| `error= {normal | severe | critical | ignore}` | Especifica la gravedad del error si el servicio no se inicia cuando se inicia el equipo. Entre estas opciones se incluyen:<ul><li>**normal** : especifica que el error se ha registrado y se muestra un cuadro de mensaje que informa al usuario de que se ha producido un error al iniciar un servicio. El inicio continuará. Esta es la configuración predeterminada.</li><li>**grave** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Esto podría dar lugar a que el equipo se reinicie, pero es posible que el servicio no se pueda ejecutar.</li><li>**Critical** : especifica que el error se registra (si es posible). El equipo intentará reiniciarse con la última configuración válida conocida. Si se produce un error en la última configuración válida conocida, también se produce un error de inicio y el proceso de arranque se detiene con un error de detención.</li><li>**omitir** : especifica que el error se ha registrado y el inicio continúa. No se proporciona ninguna notificación al usuario más allá de registrar el error en el registro de eventos.</li></ul> |
| `binpath= <binarypathname>` | Especifica una ruta de acceso al archivo binario del servicio. No hay ningún valor predeterminado para la **ruta de ruta =** y se debe proporcionar esta cadena. |
| `group= <loadordergroup>` | Especifica el nombre del grupo del que es miembro este servicio. La lista de grupos se almacena en el registro, en la subclave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . El valor predeterminado es null. |
| `tag= {yes | no}` | Especifica si se debe obtener o no un TagID de la llamada a CreateService. Las etiquetas solo se usan para los controladores de inicio de arranque y de inicio del sistema. |
| `depend= <dependencies>` | Especifica los nombres de los servicios o grupos que deben iniciarse antes que este servicio. Los nombres se separan mediante barras diagonales (/). |
| `obj= {<accountname> | <objectname>}` | Especifica el nombre de una cuenta en la que se ejecutará un servicio o especifica el nombre del objeto del controlador de Windows en el que se ejecutará el controlador. La configuración predeterminada es **LocalSystem**. |
| `displayname= <displayname>` | Especifica un nombre descriptivo para identificar el servicio en los programas de la interfaz de usuario. Por ejemplo, el nombre de la subclave de un servicio determinado es **wuauserv**, que tiene un nombre para mostrar más descriptivo de actualizaciones automáticas. |
| `password= <password>` | Especifica una contraseña. Esto es necesario si se usa una cuenta distinta de la cuenta LocalSystem. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Cada opción de línea de comandos (parámetro) debe incluir el signo igual como parte del nombre de la opción.

- Se requiere un espacio entre una opción y su valor (por ejemplo, **Type = Own**. Si se omite el espacio, se produce un error en la operación.

## <a name="examples"></a>Ejemplos

Para crear y registrar una nueva ruta de acceso binaria para el servicio *NewService* , escriba:

```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
```

```
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
