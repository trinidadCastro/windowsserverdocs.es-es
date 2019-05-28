---
title: secedit:generaterollback
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3229e6ccb07c925a900b298a8332c5e48cefefe7
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564666"
---
# <a name="seceditgeneraterollback"></a>secedit:generaterollback



Le permite generar una plantilla de reversión de una plantilla de configuración especificado. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|db|Obligatorio.</br>Especifica la ruta de acceso y el nombre de una base de datos que contiene la configuración almacenada en la que se realizará el análisis.</br>Si el nombre de archivo especifica una base de datos que no se ha realizado una plantilla de seguridad (tal como está representada por el archivo de configuración) asociada con él, el `/cfg \<configuration file name>` también debe especificarse la opción de línea de comandos.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se importarán en la base de datos para el análisis.</br>Esta opción /cfg solo es válida cuando se usa con el `/db \<database file name>` parámetro. Si no se especifica, el análisis se realiza contra cualquier configuración que ya está almacenada en la base de datos.|
|rbk|Obligatorio.</br>Especifica una plantilla de seguridad donde se escribe la información de reversión. Plantillas de seguridad se crean mediante el complemento de plantillas de seguridad. Archivos de versiones anteriores se pueden crear con este comando.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|No interactivo|Opcional.</br>Suprime la salida de pantalla y de registro. Aún puede ver los resultados de análisis mediante el uso de la configuración de seguridad y análisis de complemento Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Si la ruta de acceso del archivo de registro no se proporciona, el archivo de registro predeterminado (*systemroot*\Users \*UserAccount *\My Documents\Security\Logs\*DatabaseName*.log) se utiliza.

Con Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

La ejecución correcta de este comando indicará "la tarea se completó correctamente." y sólo las diferencias entre la plantilla de seguridad indicada y la configuración de directiva de seguridad de registros. Estas discrepancias en la scesrv.log contiene una lista.

Si se especifica una plantilla existente de reversión, este comando lo sobrescribirá. Puede crear una nueva plantilla de reversión con este comando. Parámetros adicionales no necesarios para alguna de estas condiciones.

## <a name="BKMK_Examples"></a>Ejemplos

Después de crear la plantilla de seguridad mediante la configuración de seguridad y análisis complemento, SecTmplContoso.inf, cree el archivo de configuración de reversión para guardar la configuración original. Escribe la acción en el archivo de registro FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)