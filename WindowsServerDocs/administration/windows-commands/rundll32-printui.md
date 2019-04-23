---
title: Rundll32 PrintUIEntry
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/25/2018
ms.openlocfilehash: c90641820bfa01c19ae7bf587c5467d3f9c5a01c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836846"
---
# <a name="rundll32-printuidllprintuientry"></a>Rundll32 PrintUIEntry

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Automatiza muchas tareas de configuración de impresora. printui.dll es el archivo ejecutable que contiene las funciones usadas por los cuadros de diálogo de configuración de impresora. Estas funciones también se pueden llamar desde dentro de una secuencia de comandos o un archivo por lotes de línea de comandos, o se pueden ejecutar de forma interactiva desde el símbolo del sistema. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).  
## <a name="syntax"></a>Sintaxis  
```  
rundll32 printui.dll PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
También puede usar la sintaxis alternativa siguiente, aunque los ejemplos de este tema usan la sintaxis anterior:  
```  
rundll32 printui.dll,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
## <a name="parameters"></a>Parámetros  
Hay dos tipos de parámetros: parámetros y parámetros de modificación de base. Bases parámetros especifican la función que el comando que se va a realizar. Solo uno de estos parámetros puede aparecer en una determinada línea de comandos. A continuación, puede modificar el parámetro base mediante el uso de uno o varios de los parámetros de modificación si son aplicables al parámetro base (no todos los parámetros de modificación son compatibles con todos los parámetros de bases).  
|Parámetros de la base|Descripción|  
|----------|--------|  
|/dl|elimina la impresora local.|  
|/dn|elimina una conexión de impresora de red.|  
|/dd|elimina un controlador de impresora.|  
|/e|Muestra las preferencias de impresión de una impresora determinada.|  
|/ga|Agrega una por cada conexión de impresora del equipo (la conexión está disponible para cualquier usuario en ese equipo cuando inician sesión).|  
|/ge|Muestra por las conexiones de impresora del equipo en un equipo.|  
|/gd|elimina una por cada conexión de impresora del equipo (la conexión se elimina la próxima vez que un usuario inicia sesión).|  
|/ia|Mediante el uso de un archivo .inf se instala a un controlador de impresora.|  
|/id|Instala a un controlador de impresora mediante el Asistente para controladores de impresora de agregar.|  
|/if|Mediante el uso de un archivo .inf se instala a una impresora.|  
|/ii|Instala a una impresora con el Asistente para agregar impresoras con un archivo .inf.|  
|/il|Instala a una impresora con el Asistente para agregar impresoras.|  
|/in|Se conecta a una impresora de red remota.|  
|/ip|Instala a una impresora con el Asistente para la instalación (disponible en la interfaz de usuario de administración de impresión) de la impresora de red.|  
|/k|imprime una página de prueba en una impresora.|  
|/o|Muestra la cola de una impresora.|  
|/p|Muestra las propiedades de una impresora. Cuando se usa este parámetro, también debe especificar un valor para el parámetro de modificación **/n [nombre]**.|  
|/s|Muestra las propiedades de un servidor de impresión. Si desea ver el servidor de impresión local, no es necesario usar un parámetro de modificación. Sin embargo, si desea ver un servidor de impresión remoto, debe especificar el **/c [name]** parámetro modificación.|  
|/Ss|Especifica qué tipo de información de una impresora se almacenarán. Si ninguno de los valores de **/Ss** se especifican el comportamiento predeterminado es como si todos ellos se especificaron. Utilice este parámetro base con los siguientes valores situados al final de la línea de comandos:<br /><br />-   **2**: Usar para almacenar la información contenida en la estructura de impresora s printER_INFO_2. Esta estructura contiene la información básica acerca de la impresora, como su nombre, nombre del servidor, nombre de puerto y nombre del recurso compartido.<br />-   **7**: Utilice esta opción para almacenar la información de servicio de directorio contenida en la estructura printER_INFO_7.<br />-   **c**: Usar para almacenar la información de perfil de color de una impresora.<br />-   **d**: Usar para almacenar datos específicos de la impresora como el identificador de hardware de la impresora s.<br />-   **s**: Usar para almacenar el descriptor de seguridad de impresora s.<br />-   **g**: Utilice esta opción para almacenar la información de la estructura DEVmode de impresora s global.<br />-   **m**: Usar para almacenar la configuración mínima para la impresora. Esto es equivalente a especificar **2** **d.**, y **g**.<br />-   **u**: Usar para almacenar la información de la impresora s por usuario estructura DEVmode.|  
|/Sr|Especifica qué información acerca de una impresora se restaura y cómo se controlan los conflictos en la configuración. Usar con los siguientes valores situados al final de la línea de comandos:<br /><br />-   **2**: Usar para restaurar la información contenida en la estructura de impresora s printER_INFO_2. Esta estructura contiene la información básica acerca de la impresora, como su nombre, nombre del servidor, nombre de puerto y nombre del recurso compartido.<br />-   **7**: Utilice esta opción para restaurar la información de servicio de directorio contenida en la estructura printER_INFO_7.<br />-   **c**: Usar para restaurar la información de perfil de color de una impresora.<br />-   **d**: Usar para restaurar datos específicos de la impresora, como el identificador de hardware de la impresora s.<br />-   **s**: Usar para restaurar el descriptor de seguridad de impresora s.<br />-   **g**: Usar para restaurar la información de la estructura DEVmode de impresora s global.<br />-   **m**: Usar para restaurar la configuración mínima para la impresora. Esto es equivalente a especificar **2**, **d.**, y **g**.<br />-   **u** usar para restaurar la información de la s de impresora por usuario estructura DEVmode.<br />-   **r**: si es diferente del nombre de la impresora está restaurando en el nombre de impresora que se almacena en el archivo, use el nombre de la impresora actual. Esto no se puede especificar con **f**. Si no **r** ni **f** especificado y los nombres no coinciden, se produce un error en la restauración de la configuración.<br />-   **f**: si el nombre de impresora que se almacena en el archivo es diferente del nombre de la impresora está restaurando en, utilice el nombre de la impresora en el archivo. Esto no se puede especificar con **r**. Si no **f** ni **r** especificado y los nombres no coinciden, se produce un error en la restauración de la configuración.<br />-   **p**: si el nombre del puerto en el archivo que se va a restaurar desde no coincide con el nombre de puerto actual de la impresora que se está restaurando a, se usa el nombre de puerto de impresora s actual.<br />-   **h**: si no se pudo compartir la impresora que se está restaurando a utilizando el nombre del recurso compartido de recursos en el archivo de configuración guardada, a continuación, intente compartir la impresora con el nombre actual del recurso compartido o un nuevo nombre de recurso compartido generada si no **H**ni **h** se especifica y no se puede compartir la impresora que se está restaurando a con el nombre de recurso compartido de guardado y, después, se produce un error de restauración.<br />-   **h**: si no se puede compartir la impresora que se está restaurando a con el nombre de recurso compartido de guardado, compartir la impresora. Si no **H** ni **h** se especifica y no se puede compartir la impresora que se está restaurando a con el nombre de recurso compartido de guardado y, después, se produce un error de restauración.<br />-   **i**: si el controlador en el archivo de configuración guardada no coincide con el controlador de la impresora que se está restaurando a y, después, se produce un error en la restauración.|  
|/Xg|Recupera la configuración de una impresora.|  
|/Xs|Establece la configuración de una impresora.|  
|/y|Establece la impresora está instalada como impresora predeterminada.|  
|/?|Muestra la Ayuda del producto para el comando y sus parámetros asociados.|  
|@[file]|Especifica un archivo de argumento de línea de comandos y se inserta directamente el texto en ese archivo en la línea de comandos.|  
|Parámetros de modificación|Descripción|  
|--------------|--------|  
|/a [archivo]|Especifica el nombre del archivo binario.|  
|/b[name]|Especifica el nombre de la base de impresora.|  
|/c[name]|Especifica el nombre del equipo si es la acción que se realizará en un equipo remoto.|  
|/f[file]|Especifica la ruta de acceso de convención de nomenclatura Universal (UNC) y el nombre del nombre del archivo .inf o el nombre de archivo de salida, dependiendo de la tarea que se va a realizar. Use **/F [archivo]** para especificar un archivo .inf dependientes.|  
|/F [archivo]|Especifica la ruta de acceso UNC y el nombre de un archivo .inf que especifica el archivo .inf con **/f [archivo]** depende.|  
|/h [arquitectura]|Especifica la arquitectura de controladores. Utilice uno de los siguientes: **x86**, **x64**, o **Itanium**.|  
|/j[provider]|Especifica el nombre del proveedor de impresión.|  
|/l[path]|Especifica la ruta de acceso UNC donde se encuentran los archivos de controlador de impresora que está usando.|  
|/m[model]|Especifica el nombre del modelo de controlador. (Este valor puede especificarse en el archivo .inf).|  
|/n[name]|Especifica el nombre de la impresora.|  
|/q|Ejecuta el comando con ninguna notificación al usuario.|  
|/r[port]|Especifica el nombre del puerto.|  
|/u|Especifica que se usará el controlador de impresora existente si ya está instalado.|  
|/t[#]|Especifica la página de índice de base cero para que comience.|  
|/v[version]|Especifica la versión del controlador. Si no especifica un valor para también **/K**, debe especificar uno de los siguientes valores: **tipo 2 - modo de núcleo** o **tipo 3 - modo de usuario**.|  
|/w|pide al usuario un controlador si el controlador no se encuentra en el archivo .inf que se especifica mediante **/f**.|  
|/Y|Especifica que no se deben generar automáticamente los nombres de la impresora.|  
|/z|Especifica que se comparten automáticamente la impresora está instalada.|  
|/K|cambia el significado del parámetro **/h [arquitectura]** para aceptar **2** en lugar de **x86**, **3** en lugar de **x64**, o **4** en lugar de **Itanium**. También cambia el valor del parámetro **/v [versión]** para aceptar **2** en lugar de **tipo 2 - modo de núcleo** y **3** en lugar de **tipo 3 - modo de usuario**.|  
|/Z|Recursos compartidos de la impresora que se va a instalar. Usar solo con el **/if** parámetro.|  
|/Mw[message]|Muestra un mensaje de advertencia al usuario antes de confirmar los cambios especificados en la línea de comandos.|  
|/Mq[message]|Muestra un mensaje de confirmación al usuario antes de confirmar los cambios especificados en la línea de comandos.|  
|/W[flags]|Especifica los parámetros o las opciones para la impresora de red, el Asistente para controladores de impresora de agregar y el Asistente para agregar impresoras de Asistente para la instalación.<br /><br />**r**: Permite a los asistentes para reiniciarse desde la última página.|  
|/G [flags]|Especifica los parámetros globales y las opciones que desea usar.<br /><br />**w**: Suprime las advertencias de controlador de configuración para el usuario.|  
## <a name="remarks"></a>Comentarios  
-   El **PrintUIEntry** palabra clave distingue mayúsculas de minúsculas y debe escribir la sintaxis de este comando con las mayúsculas y minúsculas que se muestra en los ejemplos de este tema.  
-   Consulte [ejemplos](#BKMK_Examples) en este documento para conocer la sintaxis para algunas tareas comunes. ¿Para obtener más ejemplos, en un símbolo del sistema, escriba: **rundll32 PrintUIEntry /?**  
## <a name="BKMK_Examples"></a>Ejemplos  
Para agregar una nueva impresora remota printer1, para un equipo, el CLIENTE1, que es visible para la cuenta de usuario donde se ejecuta este comando, escriba:  
```  
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1  
```  
Para agregar una impresora por medio de la impresora Agregar asistente y el uso de un archivo .inf, InfFile.inf, ubicado en la unidad c: en Infpath, tipo:  
```  
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf  
```  
Para eliminar una impresora existente, printer1 en un equipo, en Client1, escriba:  
```  
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1  
```  
Para agregar una conexión de impresora del equipo, printer2 para todos los usuarios de un tipo de equipo, Client2, (la conexión aplicará cuando un usuario inicia sesión) por:  
```  
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2  
```  
Para eliminar una conexión de impresora del equipo, printer2 para todos los usuarios de un tipo de equipo, Client2, (se eliminará la conexión cuando un usuario inicia sesión) por:  
```  
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2  
```  
Para ver las propiedades del servidor de impresión, Servidorimpresión1, escriba:  
```  
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1  
```  
Para ver las propiedades de una impresora, printer3, escriba:  
```  
rundll32 printui.dll PrintUIEntry /p /n\\printer3  
```  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [rundll32](rundll32.md)  
-   [Referencia de comandos de impresión](print-command-reference.md)  
