---
title: reg load
description: Artículo de referencia para el comando reg Load, que escribe las subclaves y entradas guardadas en una subclave diferente del registro.
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc44a6d992312f67abb29a91da848cc17787d507
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884089"
---
# <a name="reg-load"></a>reg load

Escribe las subclaves y entradas guardadas en una subclave diferente del registro. Este comando está pensado para su uso con archivos temporales que se usan para solucionar problemas o editar entradas del registro.

## <a name="syntax"></a>Sintaxis

```
reg load <keyname> <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave que se va a cargar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas.  |
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo que se va a cargar. Este archivo debe crearse de antemano mediante el comando **reg Save** y debe tener la extensión. HIV. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **carga del registro** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para cargar el archivo denominado TempHive. HIV en la clave HKLM\TempHive, escriba:

```
reg load HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG Save (comando)](reg-save.md)
