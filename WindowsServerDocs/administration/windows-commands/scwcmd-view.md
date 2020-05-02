---
title: Vista scwcmd
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa35cc46af36bca17cc042c658f7613572823bc9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722112"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Representa un archivo. XML mediante una transformación. xsl especificada. Este comando puede ser útil para mostrar los archivos. xml del Asistente para configuración de seguridad (SCW) mediante distintas vistas.

## <a name="syntax"></a>Sintaxis

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/x:\<Xmlfile. XML>|Especifica el archivo. XML que se va a ver. Se debe especificar este parámetro.|
|/s:\<xslfile. xsl>|Especifica la transformación. XSL que se va a aplicar al archivo. XML como parte del proceso de representación. Este parámetro es opcional para los archivos SCW. Xml. Cuando el comando **View** se usa para representar un archivo SCW. XML, intentará cargar automáticamente la transformación predeterminada correcta para el archivo. XML especificado. Si se especifica una transformación. xsl, se debe escribir la transformación bajo el supuesto de que el archivo. XML está en el mismo directorio que la transformación. Xsl.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd. exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para ver el archivo policyFile. XML mediante la transformación Policyview. xsl, escriba:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)