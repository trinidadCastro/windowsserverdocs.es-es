---
title: secedit analyze
description: Artículo de referencia para el comando secedit Analyze, que permite analizar la configuración actual de los sistemas con respecto a la configuración de línea de base almacenada en una base de datos.
ms.topic: reference
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 783f9d1042b860adefc49f58f38f66e0571468e3
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388561"
---
# <a name="secedit-analyze"></a>secedit/ANALYZE

Permite analizar la configuración actual de los sistemas con respecto a la configuración de línea de base almacenada en una base de datos.

## <a name="syntax"></a>Sintaxis

```
secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /dB | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la base de datos que contiene la configuración almacenada en la que se realiza el análisis. Si el nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, `/cfg <configuration file name>` también se debe especificar la opción. |
| /cfg | Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis. Esta opción solo es válida cuando se usa con el `/db <database file name>` parámetro. Si este parámetro no se especifica también, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos. |
| /overwrite | Especifica si la plantilla de seguridad del parámetro **/cfg** debe sobrescribir cualquier plantilla o plantilla compuesta almacenada en la base de datos, en lugar de anexar los resultados a la plantilla almacenada. Esta opción solo es válida cuando `/cfg <configuration file name>` se usa también el parámetro. Si este parámetro no se especifica también, la plantilla del parámetro **/cfg** se anexa a la plantilla almacenada. |
| /log | Especifica la ruta de acceso y el nombre del archivo de registro que se va a utilizar en el proceso. Si no especifica una ubicación de archivo, se utiliza el archivo de registro predeterminado `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` . |
| /quiet | Suprime la salida de la pantalla. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC). |

## <a name="examples"></a>Ejemplos

Para realizar el análisis de los parámetros de seguridad en la base de datos de seguridad, *SecDbContoso. sdb*y dirigir la salida al archivo *SecAnalysisContosoFY11*, incluidos los mensajes para comprobar que el comando se ejecutó correctamente, escriba:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

Para incorporar los cambios requeridos por el proceso de análisis en el archivo *SecContoso. inf* y, a continuación, dirigir la salida al archivo existente, *SecAnalysisContosoFY11*, sin preguntar, escriba:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [secedit/configure](secedit-configure.md)

- [secedit/Export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [secedit/Validate](secedit-validate.md)