---
title: bitsadmin addfilewithranges
description: Windows Commands topic for **bitsadmin addfilewithranges**, que agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b448550473691bbbaf573f99f00609e14e0f42
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850958"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto. Este modificador solo es válido para trabajos de descarga.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |
| RemoteURL | Dirección URL del archivo en el servidor. |
| LocalName | Nombre del archivo en el equipo local. Debe contener una ruta de acceso absoluta al archivo. |
| RangeList | Lista delimitada por comas de pares de desplazamiento: longitud. Use un signo de dos puntos para separar el valor de desplazamiento del valor de longitud. Por ejemplo, un valor de `0:100,2000:100,5000:eof` indica a BITS que transfiera 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo. |

## <a name="remarks"></a>Comentarios

- El **EOF** del token es un valor de longitud válido dentro de los pares de longitud y desplazamiento en el *\<RangeList >* . Indica al servicio que lea hasta el final del archivo especificado.

- AddFileWithRanges producirá un error con el código 0x8020002c, cuando se especifica un intervalo de longitud cero junto con otro intervalo con el mismo desplazamiento, como:

    `C:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Mensaje de error:** No se puede Agregar el archivo a Job-0x8020002c. La lista de intervalos de bytes contiene algunos intervalos superpuestos, que no se admiten.

    **Solución alternativa:** No especifique primero el intervalo de longitud cero. Por ejemplo, use: 

    `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0.`

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se indica a BITS que transfiera 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)