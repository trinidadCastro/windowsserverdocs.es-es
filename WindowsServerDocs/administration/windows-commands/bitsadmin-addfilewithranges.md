---
title: bitsadmin addfilewithranges
description: 'Temas de comandos de Windows para **bitsadmin addfilewithranges** : agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382022"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto. Este modificador solo es válido para trabajos de descarga.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|RemoteURL|*RemoteURL* es la dirección URL del archivo en el servidor.|
|LocalName|*Localname* es el nombre del archivo en el equipo local. *Localname* debe contener una ruta de acceso absoluta al archivo.|
|RangeList|*RangeList* es una lista delimitada por comas de pares de desplazamiento: longitud. Use un signo de dos puntos para separar el valor de desplazamiento del valor de longitud. Por ejemplo, un valor de `0:100,2000:100,5000:eof` indica a BITS que transfiera 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo.|

## <a name="more-information"></a>Más información

-   El **EOF** del token es un valor de longitud válido dentro de los pares de longitud y desplazamiento en el *> \<RangeList*. Indica al servicio que lea hasta el final del archivo especificado.
-   Tenga en cuenta que AddFileWithRanges producirá un error con el código 0x8020002c cuando se especifica un intervalo de longitud cero junto con otro intervalo con el mismo desplazamiento, como: C:\bits > bitsadmin/addfilewithranges J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0100:5

    Mensaje de error: No se puede Agregar el archivo a Job-0x8020002c. La lista de intervalos de bytes contiene algunos intervalos superpuestos, que no se admiten.

    Solución alternativa: no especifique primero el intervalo de longitud cero. Por ejemplo: bitsadmin/addfilewithranges J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5100:0.

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se indica a BITS que transfiera 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)