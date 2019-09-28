---
title: 'secedit: generaterollback'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ce4bd83e6eda24c10f65bd9d450a204906ff7fd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384223"
---
# <a name="seceditgeneraterollback"></a>secedit: generaterollback



Permite generar una plantilla de reversión para una plantilla de configuración especificada. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|bases|Obligatorio.</br>Especifica la ruta de acceso y el nombre de un archivo de base de datos que contiene la configuración almacenada en la que se realizará el análisis.</br>Si nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) `/cfg \<configuration file name>` asociada, también se debe especificar la opción de línea de comandos.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis.</br>Esta opción/cfg solo es válida cuando se usa con `/db \<database file name>` el parámetro. Si no se especifica, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos.|
|rbk|Obligatorio.</br>Especifica una plantilla de seguridad en la que se escribe la información de reversión. Las plantillas de seguridad se crean mediante el complemento plantillas de seguridad. Los archivos de reversión se pueden crear con este comando.|
|Inicia|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|Actividad|Opcional.</br>Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Si no se proporciona la ruta de acceso del archivo de registro, se usa el archivode registro \*predeterminado, (SystemRoot \Users cuentadeusuario<em>\*\Mis Documents\Security\Logs DatabaseName</em>. log).

A partir de Windows Server 2008 `Secedit /refreshpolicy` , se ha reemplazado por. `gpupdate` Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

La ejecución correcta de este comando indicará que la tarea se ha completado correctamente. y registran solo las discrepancias entre la plantilla de seguridad y la configuración de la Directiva de seguridad indicadas. Muestra estas discrepancias en el archivo scesrv. log.

Si se especifica una plantilla de reversión existente, este comando la sobrescribirá. Puede crear una nueva plantilla de reversión con este comando. No se necesitan parámetros adicionales para ninguna de las condiciones.

## <a name="BKMK_Examples"></a>Example

Después de crear la plantilla de seguridad con el complemento configuración y análisis de seguridad, SecTmplContoso. inf, cree el archivo de configuración de reversión para guardar la configuración original. Escriba la acción en el archivo de registro FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)