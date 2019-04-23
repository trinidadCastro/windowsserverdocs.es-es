---
title: Vista scwcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef1dd72903108edd6c5fb450c536a9325fcf546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889556"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Representa un archivo .xml mediante una transformación XSL especificado. Este comando puede ser útil para mostrar los archivos .xml de Asistente de configuración de seguridad (SCW) mediante distintas vistas.

## <a name="syntax"></a>Sintaxis

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/x:\<Xmlfile.xml>|Especifica el archivo .xml a verse. Este parámetro debe especificarse.|
|/s:\<Xslfile.xsl>|Especifica la transformación XSL que se aplicará al archivo .xml como parte del proceso de representación. Este parámetro es opcional para los archivos .xml SCW. Cuando el **vista** comando se usa para representar un archivo .xml SCW, automáticamente intenta cargar la transformación predeterminada correcta para el archivo .xml especificado. Si se especifica una transformación XSL, la transformación debe escribirse con la asunción de que se encuentra el archivo .xml en el mismo directorio que la transformación XSL.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para ver Policyfile.xml mediante el uso de la transformación Policyview.xsl, escriba:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)