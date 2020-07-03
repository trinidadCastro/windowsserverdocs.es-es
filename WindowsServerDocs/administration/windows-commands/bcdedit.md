---
title: bcdedit
description: Artículo de referencia para el comando bcdedit, que crea nuevos almacenes, modifica almacenes existentes y agrega parámetros de menú de arranque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: 2e49ed45875b79dfc4d8bbbdad8a1221000bf2b5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923520"
---
# <a name="bcdedit"></a>bcdedit

Los archivos de datos de la configuración de arranque (BCD) proporcionan un almacén que se usa para describir aplicaciones de arranque y configuraciones de aplicaciones de arranque. Los objetos y elementos del almacén reemplazan Boot.ini.

BCDEdit es una herramienta de la línea de comandos que sirve para administrar almacenes BCD. Se puede usar para una variedad de propósitos, como la creación de nuevos almacenes, la modificación de almacenes existentes, la adición de parámetros de menú de arranque, etc. Esencialmente, BCDEdit tiene el mismo objetivo que Bootcfg.exe en versiones anteriores de Windows, con dos mejoras destacables:

- Expone una gama más amplia de parámetros de arranque que Bootcfg.exe.

- Ha mejorado la compatibilidad con scripting.

> [!NOTE]
> Se requieren privilegios administrativos para usar BCDEditor con el fin de modificar BCD.

BCDEdit es la principal herramienta para editar la configuración de arranque de Windows Vista y versiones posteriores de Windows. Se incluye con la distribución de Windows Vista en la carpeta %WINDIR%\System32.

BCDEdit se limita a los tipos de datos estándar y se ha diseñado principalmente para realizar cambios comunes en BCD. Para realizar operaciones más complejas o trabajar con tipos de datos no estándar, use la interfaz de programación de aplicaciones (API) de Instrumental de administración de Windows (WMI) de BCD para crear herramientas personalizadas más versátiles y flexibles.

## <a name="syntax"></a>Sintaxis

```
bcdedit /command [<argument1>] [<argument2>] ...
```

### <a name="parameters"></a>Parámetros

### <a name="general-bcdedit-command-line-options"></a>Opciones generales de la línea de comandos de BCDEdit

| Opción | Descripción |
| ------ | ----------- |
| /? | Muestra una lista de comandos de BCDEdit. Si ejecuta este comando sin argumentos, se muestra un resumen de los comandos disponibles. Para mostrar ayuda detallada para un comando determinado, ejecute **bcdedit/?** `<command>`, donde `<command>` es el nombre del comando en el que busca más información. Por ejemplo, **bcdedit/? createstore** muestra ayuda detallada para el comando createstore. |

#### <a name="parameters-that-operate-on-a-store"></a>Parámetros que operan en un almacén

| Opción | Descripción |
| ------ | ----------- |
| /createstore | Crea un nuevo almacén de datos de la configuración de arranque (BCD) vacío. El almacén creado no es un almacén del sistema. |
| /export | Exporta el contenido del almacén del sistema a un archivo. Este archivo se puede usar más adelante para restaurar el estado del almacén del sistema. Este comando sólo es válido para el almacén del sistema. |
| /import | Restaura el estado del almacén del sistema mediante el uso de un archivo de datos de copia de seguridad generado previamente mediante la opción **/Export** . Este comando elimina las entradas existentes en el almacén del sistema antes de realizar la importación. Este comando sólo es válido para el almacén del sistema. |
| /store | Esta opción se puede usar con la mayoría de los comandos de BCDEdit para especificar el almacén que se va a usar. Si no se especifica esta opción, BCDEdit opera en el almacén del sistema. Ejecutar el comando **bcdedit/Store** por sí solo es equivalente a ejecutar el comando **bcdedit/enum Active** . |

#### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parámetros que operan en entradas de un almacén

| Parámetro | Descripción |
| ------ | ----------- |
| /copy | Hace una copia de una entrada de arranque especificada en el mismo almacén del sistema. |
| /create | Crea una nueva entrada en el almacén de datos de la configuración de arranque (BCD). Si se especifica un identificador conocido, no se pueden especificar los parámetros **/disponibilidad**, **/inherit**y **/Device** . Si no se especifica un identificador o no se conoce bien, se debe especificar una opción **/disponibilidad**, **/inherit**o **/Device** . |
| /delete | Elimina un elemento de una entrada especificada. |

#### <a name="parameters-that-operate-on-entry-options"></a>Parámetros que operan en opciones de entrada

| Parámetro | Descripción |
| ------ | ----------- |
| /deletevalue | Elimina un elemento especificado de una entrada de arranque. |
| /set | Establece el valor de una opción de entrada. |

#### <a name="parameters-that-control-output"></a>Parámetros que controlan la salida

| Parámetro | Descripción |
| ------ | ----------- |
| /enum | Enumera las entradas de un almacén. La opción **/enum** es el valor predeterminado de BCEdit, por lo que la ejecución del comando **bcdedit** sin parámetros equivale a ejecutar el comando **bcdedit/enum Active** . |
| /v | Modo detallado. Normalmente, los identificadores de entrada conocidos se representan en formato abreviado. Si se especifica **/v** como opción de línea de comandos, se muestran todos los identificadores en su totalidad. Ejecutar el comando **bcdedit/v** solo es equivalente a ejecutar el comando **bcdedit/enum Active/v** . |

#### <a name="parameters-that-control-the-boot-manager"></a>Parámetros que controlan el administrador de arranque

| Parámetro | Descripción |
| ------ | ----------- |
| /bootsequence | Especifica un orden de presentación único que se usará en el siguiente arranque. Este comando es similar a la opción **/displayorder** , excepto en que solo se usa la próxima vez que se inicia el equipo. Después, el equipo vuelve al orden de presentación original. |
| /default | Especifica la entrada predeterminada que el administrador de arranque selecciona cuando se agota el tiempo de espera. |
| /displayorder | Especifica el orden de visualización que utiliza el administrador de arranque al mostrar los parámetros de arranque a un usuario. |
| /timeout | Especifica el tiempo de espera, en segundos, antes de que el administrador de arranque seleccione la entrada predeterminada. |
| /toolsdisplayorder | Especifica el orden de presentación que el administrador de arranque usará al mostrar el menú **herramientas** . |

#### <a name="parameters-that-control-emergency-management-services"></a>Parámetros que controlan los servicios de administración de emergencia

| Parámetro | Descripción |
| ------ | ----------- |
| /bootems | Habilita o deshabilita Servicios de administración de emergencia (EMS) para la entrada especificada. |
| /ems | Habilita o deshabilita EMS para la entrada de arranque de sistema operativo especificada. |
| /emssettings | Establece la configuración global de EMS para el equipo. **/emssettings** no habilita ni deshabilita EMS para ninguna entrada de arranque concreta. |

#### <a name="parameters-that-control-debugging"></a>Parámetros que controlan la depuración

| Parámetro | Descripción |
| ------ | ----------- |
| /bootdebug | Habilita o deshabilita el depurador de arranque para una entrada de arranque especificada. Aunque este comando funciona con cualquier entrada de arranque, sólo es efectivo para aplicaciones de arranque. |
| /dbgsettings | Especifica o muestra la configuración global del depurador para el sistema. Este comando no enablepose. Para establecer una configuración de depurador global individual, use el comando **bcdedit/Set** `<dbgsettings> <type> <value>` . |
| /debug | Habilita o deshabilita el depurador de kernel para una entrada de arranque especificada. |

## <a name="additional-references"></a>Referencias adicionales

Para obtener ejemplos de cómo usar BCDEdit, consulte el artículo de [referencia de las opciones de bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference) .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
