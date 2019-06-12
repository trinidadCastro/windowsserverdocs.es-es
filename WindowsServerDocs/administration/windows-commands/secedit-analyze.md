---
title: 'secedit: analizar'
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9122c5c0fa8c42b0ccfc77ceb3f2d337b44ee5dc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441561"
---
# <a name="seceditanalyze"></a>secedit: analizar



Permite analizar la configuración actual de los sistemas frente a configuración de línea de base que se almacena en una base de datos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|db|Obligatorio.</br>Especifica la ruta de acceso y el nombre de una base de datos que contiene la configuración almacenada en la que se realizará el análisis.</br>Si el nombre de archivo especifica una base de datos que no se ha realizado una plantilla de seguridad (tal como está representada por el archivo de configuración) asociada con él, el `/cfg \<configuration file name>` también debe especificarse la opción de línea de comandos.|
|cfg|Opcional.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se importarán en la base de datos para el análisis.</br>Esta opción /cfg solo es válida cuando se usa con el `/db \<database file name>` parámetro. Si no se especifica, el análisis se realiza contra cualquier configuración que ya está almacenada en la base de datos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad en el parámetro /cfg debe sobrescribir cualquier otra plantilla o plantilla compuesta que se almacena en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos sólo es válida cuando el `/cfg \<configuration file name>` también se usa el parámetro. Si no se especifica, la plantilla en el parámetro /cfg se anexa a la plantilla almacenada.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro que se usará en el proceso.|
|No interactivo|Opcional.</br>Suprime la salida de la pantalla. Aún puede ver los resultados de análisis mediante el uso de la configuración de seguridad y análisis de complemento Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Los resultados del análisis se almacenan en un área distinta de la base de datos y pueden verse en la configuración de seguridad y el complemento de análisis a la consola MMC.

Si la ruta de acceso del archivo de registro no se proporciona, el archivo de registro predeterminado (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. se usa el registro).

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Ejemplos

Realizar el análisis de los parámetros de seguridad en la base de datos de seguridad, SecDbContoso.sdb, que ha creado mediante la configuración de seguridad y análisis. Dirigir la salida al archivo SecAnalysisContosoFY11 pedir confirmación para que pueda comprobar que el comando se ejecutó correctamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supongamos que el análisis reveló algunas deficiencias, por lo que se modificó la plantilla de seguridad, SecContoso.inf. Ejecute el comando nuevo para incorporar los cambios, dirigir la salida al archivo SecAnalysisContosoFY11 existente sin pedir confirmación.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)