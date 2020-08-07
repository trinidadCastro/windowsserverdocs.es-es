---
title: Vista scwcmd
description: Artículo de referencia de * * * *-
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c969931301afaab6cdf00e5a0238715fc655a79e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883082"
---
# <a name="scwcmd-view"></a>Scwcmd: ver

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Representa un archivo. XML mediante una transformación. xsl especificada. Este comando puede ser útil para mostrar los archivos. xml del Asistente para configuración de seguridad (SCW) mediante distintas vistas.

## <a name="syntax"></a>Sintaxis

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/x\<Xmlfile.xml>|Especifica el archivo. XML que se va a ver. Se debe especificar este parámetro.|
|modificado\<Xslfile.xsl>|Especifica la transformación. XSL que se va a aplicar al archivo. XML como parte del proceso de representación. Este parámetro es opcional para los archivos SCW. Xml. Cuando el comando **View** se usa para representar un archivo SCW. XML, intentará cargar automáticamente la transformación predeterminada correcta para el archivo. XML especificado. Si se especifica una transformación. xsl, se debe escribir la transformación bajo el supuesto de que el archivo. XML está en el mismo directorio que la transformación. Xsl.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para ver Policyfile.xml mediante la transformación Policyview. xsl, escriba:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)