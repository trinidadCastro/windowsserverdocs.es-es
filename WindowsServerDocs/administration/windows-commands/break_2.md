---
title: break
description: Artículo de referencia para el comando break, que desasocia un volumen de instantáneas de VSS y hace que sea accesible como volumen normal.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f3974f183215a42920f7406a62ab335eb101f56
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924899"
---
# <a name="break"></a>break

Desasocia un volumen de instantáneas de VSS y hace que sea accesible como volumen normal. A continuación, se puede tener acceso al volumen mediante una letra de unidad (si se asigna) o un nombre de volumen. Si se usa sin parámetros, **break** muestra la ayuda en el símbolo del sistema.

> [!NOTE]
> Este comando solo es relevante para las instantáneas de hardware después de la importación.
>
> Los volúmenes expuestos, como las instantáneas de las que se originan, son de solo lectura de forma predeterminada. El acceso al volumen se realiza directamente en el proveedor de hardware sin registrar el volumen que ha sido una instantánea.

## <a name="syntax"></a>Sintaxis

```
break [writable] <setid>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| edita | Habilita el acceso de lectura y escritura en el volumen. |
| \<setid> | Especifica el identificador del conjunto de instantáneas. El alias del identificador de instantánea, que se almacena como una variable de entorno mediante el comando **cargar metadatos** , se puede usar en el parámetro *SetID* . |

## <a name="examples"></a>Ejemplos

Para crear una instantánea con el nombre de alias Alias1 accesible como volumen grabable en el sistema operativo:

```
break writable %Alias1%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)