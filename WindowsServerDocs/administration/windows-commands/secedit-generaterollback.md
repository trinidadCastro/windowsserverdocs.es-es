---
title: 'secedit: generaterollback'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d2a8ec7be0292fc096c072b0f4e5806e8051408
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834938"
---
# <a name="seceditgeneraterollback"></a>secedit: generaterollback



Permite generar una plantilla de reversión para una plantilla de configuración especificada. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|bases|Obligatorio.</br>Especifica la ruta de acceso y el nombre de un archivo de base de datos que contiene la configuración almacenada en la que se realizará el análisis.</br>Si nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, también se debe especificar la opción de línea de comandos `/cfg \<configuration file name>`.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis.</br>Esta opción/cfg solo es válida cuando se usa con el parámetro `/db \<database file name>`. Si no se especifica, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos.|
|rbk|Obligatorio.</br>Especifica una plantilla de seguridad en la que se escribe la información de reversión. Las plantillas de seguridad se crean mediante el complemento plantillas de seguridad. Los archivos de reversión se pueden crear con este comando.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|actividad|Opcional.</br>Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Si no se proporciona la ruta de acceso del archivo de registro, se utiliza el archivo de registro predeterminado, (*systemroot*\Users \*cuentadeusuario<em>\Mis Documents\Security\Logs\*DatabaseName</em>. log).

A partir de Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

La ejecución correcta de este comando indicará que la tarea se ha completado correctamente. y registran solo las discrepancias entre la plantilla de seguridad y la configuración de la Directiva de seguridad indicadas. Muestra estas discrepancias en el archivo scesrv. log.

Si se especifica una plantilla de reversión existente, este comando la sobrescribirá. Puede crear una nueva plantilla de reversión con este comando. No se necesitan parámetros adicionales para ninguna de las condiciones.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Después de crear la plantilla de seguridad con el complemento configuración y análisis de seguridad, SecTmplContoso. inf, cree el archivo de configuración de reversión para guardar la configuración original. Escriba la acción en el archivo de registro FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

## <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)