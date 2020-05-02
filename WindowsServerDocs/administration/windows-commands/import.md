---
title: importación
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72cbd6195de64a6a0a7f2c258e19b2d5eb1378b1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724853"
---
# <a name="import"></a>importación



Importa una instantánea transportable de un archivo de metadatos cargado en el sistema.



## <a name="syntax"></a>Sintaxis

```
import
```

## <a name="remarks"></a>Observaciones

-   Las instantáneas transportables no se almacenan en el sistema inmediatamente. Sus detalles se almacenan en un archivo XML de documento de componentes de copia de seguridad, que DiskShadow solicita y guarda automáticamente en un archivo de metadatos. cab en el directorio de trabajo. Puede cambiar la ruta de acceso y el nombre de este archivo mediante el comando **set Metadata** .
-   Antes de poder usar la **importación**, debe cargar un archivo de metadatos de DiskShadow con el comando **cargar metadatos** .

## <a name="examples"></a>Ejemplos

El siguiente es un script de DiskShadow de ejemplo que muestra el uso del comando de **importación** :
```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)