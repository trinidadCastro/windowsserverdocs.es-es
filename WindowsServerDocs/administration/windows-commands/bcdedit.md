---
title: bcdedit
description: 'Tema de los comandos de Windows para **bcdedit** : crear nuevos almacenes, modificar almacenes existentes y agregar parámetros de menú de arranque.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872446"
---
# <a name="bcdedit"></a>bcdedit



Archivos de arranque de datos de configuración (BCD) proporcionan un almacén que se usa para describir aplicaciones de arranque y la configuración de la aplicación de arranque. Los objetos y elementos del almacén reemplazan Boot.ini.

BCDEdit es una herramienta de línea de comandos para administrar almacenes BCD. Se puede usar para una variedad de propósitos, incluida la creación de nuevos almacenes, modificar almacenes existentes, agregar parámetros de menú de arranque y así sucesivamente. BCDEdit actúa esencialmente el mismo objetivo que Bootcfg.exe en versiones anteriores de Windows, pero con dos mejoras principales:
-   Expone una gama más amplia de los parámetros de arranque que Bootcfg.exe.
-   Ha mejorado la compatibilidad con scripting.

> [!NOTE]
> Se requieren privilegios administrativos para usar BCDEdit para modificar el BCD.

BCDEdit es la principal herramienta para editar la configuración de arranque de Windows Vista y versiones posteriores de Windows. Se incluye con la distribución de Windows Vista en la carpeta %WINDIR%\System32.

BCDEdit se limita a los tipos de datos estándar y está diseñado principalmente para realizar cambios comunes en BCD. Para operaciones más complejas o tipos de datos no estándar, considere el uso de la interfaz de programación de aplicaciones (API) de Windows Management Instrumentation (WMI) de BCD para crear herramientas personalizadas más eficaces y flexibles.

## <a name="syntax"></a>Sintaxis

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parámetros

### <a name="general-bcdedit-command-line-option"></a>Opción de línea de comandos de general BCDEdit

|Opción|Descripción|
|------|-----------|
|/?|Muestra una lista de los comandos de BCDEdit. Ejecutar este comando sin un argumento, muestra un resumen de los comandos disponibles. ¿Para mostrar ayuda detallada para un determinado comando, ejecute **bcdedit /?** \<comando >, donde <command> es el nombre del comando que está buscando para obtener más información acerca de. Por ejemplo, **bcdedit /? createstore** muestra ayuda detallada acerca del comando Createstore.|

### <a name="parameters-that-operate-on-a-store"></a>Parámetros que operan en un Store

|Opción|Descripción|
|------|-----------|
|/createstore|Crea un nuevo almacén de datos de configuración de arranque vacío. El almacén creado no es un almacén del sistema.|
|/export|Exporta el contenido del sistema de almacena en un archivo. Este archivo se puede usar posteriormente para restaurar el estado del almacén del sistema. Este comando es válido sólo para el almacén del sistema.|
|/Import|Restaura el estado del almacén del sistema mediante el uso de un archivo de datos de copia de seguridad generado anteriormente con el **/export** opción. Este comando elimina las entradas existentes en el almacén del sistema antes de la importación. Este comando es válido sólo para el almacén del sistema.|
|/store|Esta opción puede utilizarse con la mayoría de los comandos de BCDedit para especificar el almacén que se usará. Si no se especifica esta opción, BCDEdit opera en el almacén del sistema. Ejecuta el **bcdedit /store** es equivalente a ejecutar comando por sí mismo el **bcdedit /enum active** comando.|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parámetros que operan en las entradas de un Store

|Parámetro|Descripción|
|---------|-----------|
|/Copy|Realiza una copia de una entrada de arranque especificada en el mismo almacén del sistema.|
|/create|Crea una nueva entrada en el almacén de datos de configuración de arranque. Si se especifica un identificador conocido, el **aplicación/**, **/ heredan**, y **/Device** no se puede especificar parámetros. Si un identificador no se especificó o no conocida, un **aplicación/**, **/ heredan**, o **/Device** opción debe especificarse.|
|/delete|Elimina un elemento de una entrada especificada.|

### <a name="parameters-that-operate-on-entry-options"></a>Parámetros que operan en las opciones de entrada

|Parámetro|Descripción|
|---------|-----------|
|/deletevalue|Elimina un elemento especificado de una entrada de arranque.|
|/set|Establece un valor de opción de entrada.|

### <a name="parameters-that-control-output"></a>Parámetros que controlan la salida

|Parámetro|Descripción|
|---------|-----------|
|/enum|Enumera las entradas de un almacén. El **/enum** opción es el valor predeterminado para BCEdit, por lo que ejecutar el **bcdedit** comando sin parámetros es equivalente a ejecutar el **bcdedit /enum active** comando.|
|/v|Modo detallado. Normalmente, los identificadores de entrada conocidos se representan mediante formato abreviado. Especificar **/v** como una opción de línea de comandos muestra todos los identificadores en su totalidad. Ejecuta el **bcdedit /v** es equivalente a ejecutar comando por sí mismo el **bcdedit /enum active /v** comando.|

### <a name="parameters-that-control-the-boot-manager"></a>Parámetros que controlan el Administrador de arranque

|Parámetro|Descripción|
|---------|-----------|
|/bootsequence|Especifica un orden de presentación único que se usará para el siguiente arranque. Este comando es similar a la **/displayorder** opción, salvo que se usa solo la próxima vez que el equipo se inicia. Después, el equipo vuelve al orden de presentación original.|
|/ default|Especifica la entrada predeterminada que el Administrador de arranque selecciona cuando expira el tiempo de espera.|
|/displayorder|Especifica el orden de presentación que utiliza el Administrador de arranque al mostrar los parámetros de arranque a un usuario.|
|/timeout|Especifica el tiempo de espera en segundos, antes de que el Administrador de arranque seleccione la entrada predeterminada.|
|/toolsdisplayorder|Especifica el orden de presentación para el Administrador de arranque que se usará al mostrar la **herramientas** menú.|

### <a name="parameters-that-control-emergency-management-services"></a>Parámetros que controlan los servicios de administración de emergencia

|Parámetro|Descripción|
|---------|-----------|
|/bootems|Habilita o deshabilita los servicios de administración de emergencia (EMS) para la entrada especificada.|
|/ems|Habilita o deshabilita EMS para la entrada de arranque del sistema operativo especificado.|
|/emssettings|Establece la configuración global de EMS para el equipo. **/emssettings** no habilita ni deshabilita EMS para ninguna entrada de arranque en particular.|

### <a name="parameters-that-control-debugging"></a>Parámetros que controlan la depuración

|Parámetro|Descripción|
|---------|-----------|
|/bootdebug|Habilita o deshabilita al depurador de arranque para una entrada de arranque especificada. Aunque este comando funciona para cualquier entrada de arranque, es eficaz solo para aplicaciones de arranque.|
|/dbgsettings|Especifica o muestra la configuración global del depurador para el sistema. Este comando no habilita ni deshabilita al depurador de kernel; Utilice la **/debug** opción para ese propósito. Para establecer un configuración de depurador global individual, utilice el **bcdedit /set** \<dbgsettings > <type> <value> .|
|/debug|Habilita o deshabilita al depurador de kernel para una entrada de arranque especificada.|

## <a name="examples"></a>Ejemplos

Para obtener ejemplos de BCDEdit, vea el [BCDEdit opciones referencia](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).