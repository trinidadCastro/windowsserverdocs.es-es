---
title: bitsadmin addfilewithranges
description: Artículo de referencia para el comando bitsadmin addfilewithranges, que agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto.
ms.topic: reference
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f9031ebd6ffe2e1480e59e5e357a33b9895766
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027853"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Agrega un archivo al trabajo especificado. BITS descarga los intervalos especificados del archivo remoto. Este modificador solo es válido para trabajos de descarga.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfilewithranges <job> <remoteURL> <localname> <rangelist>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| remoteURL | Dirección URL del archivo en el servidor. |
| localname | Nombre del archivo en el equipo local. Debe contener una ruta de acceso absoluta al archivo. |
| rangelist | Lista delimitada por comas de pares de desplazamiento: longitud. Use un signo de dos puntos para separar el valor de desplazamiento del valor de longitud. Por ejemplo, un valor de `0:100,2000:100,5000:eof` indica a bits que transfiera 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo. |

## <a name="remarks"></a>Observaciones

- El **EOF** del token es un valor de longitud válido dentro de los pares de longitud y desplazamiento en `<rangelist>` . Indica al servicio que lea hasta el final del archivo especificado.

- El `addfilewithranges` comando producirá un error con el código 0x8020002c, si se especifica un intervalo de longitud cero junto con otro intervalo con el mismo desplazamiento, por ejemplo:

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Mensaje de error:** No se puede Agregar el archivo a Job-0x8020002c. La lista de intervalos de bytes contiene algunos intervalos superpuestos, que no se admiten.

    **Solución alternativa:** No especifique primero el intervalo de longitud cero. Por ejemplo, use `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`.

## <a name="examples"></a>Ejemplos

Para transferir 100 bytes desde el desplazamiento 0, 100 bytes desde el desplazamiento 2000 y los bytes restantes desde el desplazamiento 5000 hasta el final del archivo:

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
