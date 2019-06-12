---
title: bitsadmin addfilewithranges
description: Tema de los comandos de Windows para **addfilewithranges bitsadmin** -agrega un archivo para el trabajo especificado. Descargas de BITS de los intervalos especificados desde el archivo remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5e1e4f8af9117928f9ab044d29e65f57aa5a119
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811280"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Agrega un archivo para el trabajo especificado. Descargas de BITS de los intervalos especificados desde el archivo remoto. Este modificador es válido sólo para los trabajos de descarga.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|RemoteURL|*RemoteURL* es la dirección URL del archivo en el servidor.|
|LocalName|*LocalName* es el nombre del archivo en el equipo local. *LocalName* debe contener una ruta de acceso absoluta al archivo.|
|RangeList|*RangeList* es una lista delimitada por comas de pares de desplazamiento: longitud. Utilice dos puntos para separar el valor de desplazamiento desde el valor de longitud. Por ejemplo, un valor de `0:100,2000:100,5000:eof` indica los BITS para transferir 100 bytes desde el desplazamiento 0, 100 bytes del desplazamiento de 2000 y los bytes restantes del desplazamiento 5000 hasta el final del archivo.|

## <a name="more-information"></a>Más información

-   El token **eof** es un valor de longitud válida dentro de los pares de desplazamiento y longitud de la  *\<RangeList >* . Indica el servicio para leer hasta el final del archivo especificado.
-   Tenga en cuenta que AddFileWithRanges se producirá un error con código de error 0x8020002c cuando se especifica un intervalo de longitud cero, junto con otro intervalo con el mismo desplazamiento, como: C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0, 100:5

    Mensaje de error: No se puede agregar archivos al trabajo - 0x8020002c. La lista de intervalos de bytes contiene intervalos solapados, que no son compatibles.

    Solución alternativa: no especifique el intervalo de longitud cero en primer lugar. Por ejemplo: bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5, 100:0.

## <a name="examples"></a>Ejemplos

El ejemplo siguiente se indica a BITS para transferir 100 bytes desde el desplazamiento 0, 100 bytes del desplazamiento 2000 y los bytes restantes del desplazamiento 5000 hasta el final del archivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)