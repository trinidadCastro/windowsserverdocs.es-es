---
title: jetpack
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec29a1fd48fdba72f07fe5d00de7730d93434105
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724797"
---
# <a name="jetpack"></a>jetpack

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

compacta una base de datos del servicio de nombres Internet de Windows (WINS) o del Protocolo de configuración dinámica de host (DHCP). Microsoft recomienda compactar la base de datos WINS cada vez que se aproxime a 30 MB. 

## <a name="syntax"></a>Sintaxis
```
jetpack.EXE <database name> <temp database name>
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<database name>|Especifica el archivo de base de datos original.|
|<temp database name>|Especifica el archivo de base de datos temporal.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos
Para compactar la base de datos WINS:
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Para compactar la base de datos DHCP:
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
En los ejemplos anteriores, **tmp. mdb** es una base de datos temporal utilizada por jetpack. exe. **WINS. mdb** es la base de datos WINS. **DHCP. mdb** es la base de datos DHCP.
Jetpack. exe compacta la base de datos WINS o DHCP haciendo lo siguiente:
1.  Copia la información de base de datos en un archivo de base de datos temporal denominado **tmp. mdb**.
2.  elimina el archivo de base de datos original, **WINS. mdb** o **DHCP. mdb**.
3.  cambia el nombre de los archivos temporales de la base de datos al nombre de archivo original.

> [!NOTE]
> Durante el proceso de compactación, jetpack. exe crea un archivo temporal con el nombre especificado por el parámetro de *nombre de base de datos temporal* . El archivo temporal se quita cuando se completa el proceso de compactación. Asegúrese de que no hay ningún archivo ya existente en la carpeta WINS o DHCP con el mismo nombre que el especificado en el parámetro *nombre de la base de datos temporal* .

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
