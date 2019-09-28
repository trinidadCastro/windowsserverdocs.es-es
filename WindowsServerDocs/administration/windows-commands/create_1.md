---
title: crear
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2245efb6c3bce8aecf8edf730694804ffbdc3d80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378775"
---
# <a name="create"></a>crear



Inicia el proceso de creación de instantáneas con la configuración de contexto y opciones actual. Requiere al menos un volumen en el conjunto de instantáneas.

## <a name="syntax"></a>Sintaxis

```
create
```

## <a name="remarks"></a>Comentarios

-   Debe agregar al menos un volumen con el comando **Add Volume** para poder usar el comando **Create** .
-   Puede usar el comando **Begin backup** para especificar una copia de seguridad completa, en lugar de una copia de seguridad de copia.
-   Después de ejecutar el comando **Create** , puede usar el comando **exec** para ejecutar un script de duplicación para la copia de seguridad de la instantánea.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)