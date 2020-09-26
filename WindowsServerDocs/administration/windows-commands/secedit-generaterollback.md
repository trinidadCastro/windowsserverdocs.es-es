---
title: secedit generaterollback
description: Artículo de referencia para el comando secedit generaterollback, que le permite generar una plantilla de reversión para una plantilla de configuración especificada.
ms.topic: reference
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ae2e368ef387ea84095fcbcc51ad1e622225a2cc
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388821"
---
# <a name="secedit-generaterollback"></a>secedit/generaterollback

Permite generar una plantilla de reversión para una plantilla de configuración especificada. Si existe una plantilla de reversión existente, si vuelve a ejecutar este comando, se sobrescribirá la información existente.

Al ejecutar correctamente este comando, se registran las discrepancias entre la plantilla de seguridad especificada de la configuración de la Directiva de seguridad en el archivo scesrv. log.

## <a name="syntax"></a>Sintaxis

```
secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /dB | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la base de datos que contiene la configuración almacenada en la que se realiza el análisis. Si el nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, `/cfg <configuration file name>` también se debe especificar la opción. |
| /cfg | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis. Esta opción solo es válida cuando se usa con el `/db <database file name>` parámetro. Si este parámetro no se especifica también, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos. |
| /rbk | Obligatorio. Especifica una plantilla de seguridad en la que se escribe la información de reversión. Las plantillas de seguridad se crean mediante el complemento plantillas de seguridad. Los archivos de reversión se pueden crear con este comando. |
| /log | Especifica la ruta de acceso y el nombre del archivo de registro que se va a utilizar en el proceso. Si no especifica una ubicación de archivo, se utiliza el archivo de registro predeterminado `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` . |
| /quiet | Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC). |

## <a name="examples"></a>Ejemplos

Para crear el archivo de configuración de reversión, para el archivo *SecTmplContoso. inf* creado anteriormente, al guardar la configuración original y, a continuación, escribir la acción en el archivo de registro *SecAnalysisContosoFY11* , escriba:

```
secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [secedit/ANALYZE](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/Export](secedit-export.md)

- [secedit/Import](secedit-import.md)

- [secedit/Validate](secedit-validate.md)