---
title: importación
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07fdd03c73c454e92218a4c6983eac7f29b50883
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842178"
---
# <a name="import"></a>importación



importa una instantánea transportable de un archivo de metadatos cargado en el sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
import
```

## <a name="remarks"></a>Comentarios

-   Las instantáneas transportables no se almacenan en el sistema inmediatamente. Sus detalles se almacenan en un archivo XML de documento de componentes de copia de seguridad, que DiskShadow solicita y guarda automáticamente en un archivo de metadatos. cab en el directorio de trabajo. Puede cambiar la ruta de acceso y el nombre de este archivo mediante el comando **set Metadata** .
-   Antes de poder usar la **importación**, debe cargar un archivo de metadatos de DiskShadow con el comando **cargar metadatos** .

## <a name="examples"></a><a name=BKMK_examples></a>Example

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