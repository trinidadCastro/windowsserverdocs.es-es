---
title: nslookup set class
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1ae3a5336815a5273aafa976b1dcad8b60fac9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723657"
---
# <a name="nslookup-set-class"></a>nslookup set class



Cambia la clase de consulta. La clase especifica el grupo de protocolos de la información.

## <a name="syntax"></a>Sintaxis

```
set class=<Class>
```

### <a name="parameters"></a>Parámetros

| Parámetro |                                                                                                                                    Descripción                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Class>  | La clase predeterminada está en. A continuación se enumeran los valores válidos para este comando.</br>-IN: especifica la clase de Internet.</br>-CAOS: especifica la clase caos.</br>-HESIOD: especifica la clase MIT Athena Hesiod.</br>-ANY: especifica cualquiera de los caracteres comodín enumerados anteriormente. |
|   {ayuda   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)