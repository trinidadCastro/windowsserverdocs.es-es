---
title: jetpack
description: Artículo de referencia del comando jetpack, que compacta una base de datos del servicio de nombres Internet de Windows (WINS) o del Protocolo de configuración dinámica de host (DHCP).
ms.topic: reference
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf2664c142a169982acc0d11c34a53aea193d030
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035423"
---
# <a name="jetpack"></a>jetpack

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Compacta una base de datos del servicio de nombres Internet de Windows (WINS) o del Protocolo de configuración dinámica de host (DHCP). Se recomienda compactar la base de datos WINS cada vez que se aproxime a 30 MB.

Jetpack.exe compacta la base de datos de la siguiente manera:

1. Copiar la información de base de datos en un archivo de base de datos temporal.

2. Eliminar el archivo de base de datos original, ya sea WINS o DHCP.

3. Cambia el nombre de los archivos temporales de la base de datos al nombre de archivo original.

## <a name="syntax"></a>Sintaxis

```
jetpack.exe <database_name> <temp_database_name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| `<database_name>` | Especifica el nombre del archivo de base de datos original. |
| `<temp_database_name>` | Especifica el nombre del archivo de base de datos temporal que va a crear jetpack.exe.<p>Nota: este archivo temporal se quita cuando se completa el proceso de compactación. Para que este comando funcione correctamente, debe asegurarse de que el nombre del archivo temporal es único y de que ya existe un archivo con ese nombre. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para compactar la base de datos WINS, donde **tmp. mdb** es una base de datos temporal y **WINS. mdb** es la base de datos WINS, escriba:

```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack Wins.mdb Tmp.mdb
NET start WINS
```

Para compactar la base de datos DHCP, donde **tmp. mdb** es una base de datos temporal y **DHCP. mdb** es la base de datos DHCP, escriba:

```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERVER
jetpack Dhcp.mdb Tmp.mdb
NET start DHCPSERVER
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
