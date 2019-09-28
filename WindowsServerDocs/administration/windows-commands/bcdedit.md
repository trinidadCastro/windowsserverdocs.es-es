---
title: bcdedit
description: 'Temas de comandos de Windows para **bcdedit** : cree nuevos almacenes, modifique almacenes existentes y agregue parámetros de menú de arranque.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9448f4461a089a93382ef8cd9e804b382fca27e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382249"
---
# <a name="bcdedit"></a>bcdedit



Los archivos datos de la configuración de arranque (BCD) (BCD) proporcionan un almacén que se usa para describir las aplicaciones de arranque y la configuración de la aplicación de arranque. Los objetos y elementos del almacén reemplazan boot. ini de forma eficaz.

BCDEdit es una herramienta de línea de comandos para administrar almacenes BCD. Se puede usar para una variedad de propósitos, como la creación de nuevos almacenes, la modificación de almacenes existentes, la adición de parámetros de menú de arranque, etc. BCDEdit sirve esencialmente el mismo propósito que Bootcfg. exe en versiones anteriores de Windows, pero con dos mejoras importantes:
-   Expone una mayor variedad de parámetros de arranque que Bootcfg. exe.
-   Ha mejorado la compatibilidad con scripting.

> [!NOTE]
> Se requieren privilegios administrativos para usar BCDEdit para modificar BCD.

BCDEdit es la herramienta principal para editar la configuración de arranque de Windows Vista y versiones posteriores de Windows. Se incluye con la distribución de Windows Vista en la carpeta%WINDIR%\System32.

BCDEdit está limitado a los tipos de datos estándar y está diseñado principalmente para realizar cambios comunes en BCD. Para operaciones más complejas o tipos de datos no estándar, considere la posibilidad de usar la interfaz de programación de aplicaciones (API) de BCD Instrumental de administración de Windows (WMI) para crear herramientas personalizadas más eficaces y flexibles.

## <a name="syntax"></a>Sintaxis

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parámetros

### <a name="general-bcdedit-command-line-option"></a>Opción de línea de comandos general de BCDEdit

|Opción|Descripción|
|------|-----------|
|/?|Muestra una lista de comandos de BCDEdit. Al ejecutar este comando sin un argumento, se muestra un resumen de los comandos disponibles. Para mostrar ayuda detallada para un comando determinado, ejecute **bcdedit/?** \<command >, donde <command> es el nombre del comando en el que busca más información. Por ejemplo, **bcdedit/? createstore** muestra ayuda detallada para el comando createstore.|

### <a name="parameters-that-operate-on-a-store"></a>Parámetros que operan en un almacén

|Opción|Descripción|
|------|-----------|
|/createstore|Crea un nuevo almacén de datos de configuración de arranque vacío. El almacén creado no es un almacén del sistema.|
|/Export|Exporta el contenido del almacén del sistema a un archivo. Este archivo se puede usar más adelante para restaurar el estado del almacén del sistema. Este comando solo es válido para el almacén del sistema.|
|/Import|Restaura el estado del almacén del sistema mediante el uso de un archivo de datos de copia de seguridad generado previamente mediante la opción **/Export** . Este comando elimina las entradas existentes en el almacén del sistema antes de que tenga lugar la importación. Este comando solo es válido para el almacén del sistema.|
|/Store|Esta opción se puede usar con la mayoría de los comandos de BCDedit para especificar el almacén que se va a usar. Si no se especifica esta opción, BCDEdit opera en el almacén del sistema. Ejecutar el comando **bcdedit/Store** por sí solo es equivalente a ejecutar el comando **bcdedit/enum Active** .|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parámetros que operan en entradas de un almacén

|Parámetro|Descripción|
|---------|-----------|
|/Copy|Realiza una copia de una entrada de arranque especificada en el mismo almacén del sistema.|
|/Create|Crea una nueva entrada en el almacén de datos de la configuración de arranque. Si se especifica un identificador conocido, no se pueden especificar los parámetros **/disponibilidad**, **/inherit**y **/Device** . Si no se especifica un identificador o no se conoce bien, se debe especificar una opción **/disponibilidad**, **/inherit**o **/Device** .|
|/delete|Elimina un elemento de una entrada especificada.|

### <a name="parameters-that-operate-on-entry-options"></a>Parámetros que operan en opciones de entrada

|Parámetro|Descripción|
|---------|-----------|
|/deletevalue|Elimina un elemento especificado de una entrada de arranque.|
|/Set|Establece un valor de la opción de entrada.|

### <a name="parameters-that-control-output"></a>Parámetros que controlan la salida

|Parámetro|Descripción|
|---------|-----------|
|/enum|Muestra las entradas de un almacén. La opción **/enum** es el valor predeterminado de BCEdit, por lo que la ejecución del comando **bcdedit** sin parámetros equivale a ejecutar el comando **bcdedit/enum Active** .|
|/v|modo detallado. Normalmente, los identificadores de entrada conocidos se representan mediante su forma abreviada. Si se especifica **/v** como opción de línea de comandos, se muestran todos los identificadores en su totalidad. Ejecutar el comando **bcdedit/v** solo es equivalente a ejecutar el comando **bcdedit/enum Active/v** .|

### <a name="parameters-that-control-the-boot-manager"></a>Parámetros que controlan el administrador de arranque

|Parámetro|Descripción|
|---------|-----------|
|/bootsequence|Especifica un orden de visualización único que se va a usar para el siguiente arranque. Este comando es similar a la opción **/displayorder** , excepto en que solo se usa la próxima vez que se inicia el equipo. Posteriormente, el equipo vuelve al orden de presentación original.|
|/default|Especifica la entrada predeterminada que el administrador de arranque selecciona cuando expira el tiempo de espera.|
|/displayorder|Especifica el orden de visualización que utiliza el administrador de arranque al mostrar los parámetros de arranque a un usuario.|
|/timeout|Especifica el tiempo de espera, en segundos, antes de que el administrador de arranque seleccione la entrada predeterminada.|
|/toolsdisplayorder|Especifica el orden de presentación que el administrador de arranque usará al mostrar el menú **herramientas** .|

### <a name="parameters-that-control-emergency-management-services"></a>Parámetros que controlan los servicios de administración de emergencia

|Parámetro|Descripción|
|---------|-----------|
|/bootems|Habilita o deshabilita los servicios de administración de emergencia (EMS) para la entrada especificada.|
|/EMS|Habilita o deshabilita EMS para la entrada de arranque del sistema operativo especificada.|
|/emssettings|Establece la configuración global de EMS para el equipo. **/emssettings** no habilita ni deshabilita EMS para ninguna entrada de arranque concreta.|

### <a name="parameters-that-control-debugging"></a>Parámetros que controlan la depuración

|Parámetro|Descripción|
|---------|-----------|
|/bootdebug|Habilita o deshabilita el depurador de arranque para una entrada de arranque especificada. Aunque este comando funciona para cualquier entrada de arranque, solo es eficaz para las aplicaciones de arranque.|
|/dbgsettings|Especifica o muestra la configuración global del depurador para el sistema. Este comando no habilita ni deshabilita el depurador del kernel; Use la opción **/Debug** para ese propósito. Para establecer una configuración de depurador global individual, use el > **bcdedit/set** \<dbgsettings <type> <value> .|
|/debug|Habilita o deshabilita el depurador del kernel para una entrada de arranque especificada.|

## <a name="examples"></a>Ejemplos

Para obtener ejemplos de BCDEdit, vea la [referencia de las opciones de bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).