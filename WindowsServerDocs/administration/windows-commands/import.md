---
title: importar DiskShadow
description: Tema de referencia del comando de importación, que importa una instantánea transportable de un archivo de metadatos cargado en el sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4eaf4afcbe2485a893a3e5335c595b3d9b256b5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818515"
---
# <a name="import-diskshadow"></a>importar (DiskShadow)

Importa una instantánea transportable de un archivo de metadatos cargado en el sistema.

> AÚN Para poder usar este comando, debe usar el [comando cargar metadatos](load-metadata.md) para cargar un archivo de metadatos de DiskShadow.

## <a name="syntax"></a>Sintaxis

```
import
```

#### <a name="remarks"></a>Observaciones

- Las instantáneas transportables no se almacenan en el sistema inmediatamente. Sus detalles se almacenan en un archivo XML de documento de componentes de copia de seguridad, que DiskShadow solicita y guarda automáticamente en un archivo de metadatos. cab en el directorio de trabajo. Use el [comando establecer metadatos](set-metadata.md) para cambiar la ruta de acceso y el nombre de este archivo XML.

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

- [DiskShadow (comando)](diskshadow.md)