---
title: scwcmd view
description: Artículo de referencia para el comando scwcmd View, que representa un archivo. XML mediante una transformación. xsl especificada.
ms.topic: reference
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e411bf06015684e7dedb94d109f5e4294f46af84
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388935"
---
# <a name="scwcmd-view"></a>scwcmd view

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Representa un archivo. XML mediante una transformación. xsl especificada. Este comando puede ser útil para mostrar los archivos. xml del Asistente para configuración de seguridad (SCW) mediante distintas vistas.

## <a name="syntax"></a>Sintaxis

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /x`<Xmlfile.xml>` | Especifica el archivo. XML que se va a ver. Se debe especificar este parámetro. |
| modificado`<Xslfile.xsl>` | Especifica la transformación. XSL que se va a aplicar al archivo. XML como parte del proceso de representación. Este parámetro es opcional para los archivos SCW. Xml. Cuando el comando **View** se usa para representar un archivo SCW. XML, intentará cargar automáticamente la transformación predeterminada correcta para el archivo. XML especificado. Si se especifica una transformación. xsl, se debe escribir la transformación bajo el supuesto de que el archivo. XML está en el mismo directorio que la transformación. Xsl. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="example"></a>Ejemplo

Para ver *Policyfile.xml* mediante la transformación *Policyview. xsl* , escriba:

```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd ANALYZE](scwcmd-analyze.md)

- [comando scwcmd configure](scwcmd-configure.md)

- [comando scwcmd Register](scwcmd-register.md)

- [comando de restauración de scwcmd](scwcmd-rollback.md)

- [scwcmd transform (comando)](scwcmd-transform.md)