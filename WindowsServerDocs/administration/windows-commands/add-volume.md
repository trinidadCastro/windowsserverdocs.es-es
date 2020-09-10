---
title: add volume
description: Artículo de referencia del comando Add Volume, que agrega volúmenes al conjunto de instantáneas, que es el conjunto de volúmenes de los que se va a realizar la instantánea.
ms.topic: reference
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2a1dc6f00ebc497e9276d1f3b5a74a1f1a10dbfc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633539"
---
# <a name="add-volume"></a>add volume

Agrega volúmenes al conjunto de instantáneas, que es el conjunto de volúmenes de los que se va a realizar la instantánea. Cuando se crea una instantánea, una variable de entorno vincula el alias con el identificador de la instantánea, de modo que el alias se puede usar para el scripting.

Los volúmenes se agregan de uno en uno. Cada vez que se agrega un volumen, se comprueba para asegurarse de que VSS admite la creación de instantáneas para ese volumen. Esta comprobación se puede invalidar mediante el uso posterior del comando **set context** .

Este comando es necesario para crear instantáneas. Si se usa sin parámetros, **agregar volumen** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
add volume <volume> [provider <providerid>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` | Especifica el volumen que se va a agregar al conjunto de instantáneas. Se requiere al menos un volumen para la creación de la instantánea. |
| `[provider \<providerid>]` | Especifica el identificador de proveedor de un proveedor registrado que se va a usar para crear la instantánea. Si no se especifica **Provider** , se utilizará el proveedor predeterminado. |

## <a name="examples"></a>Ejemplos

Para ver la lista actual de proveedores registrados, `diskshadow>` Escriba lo siguiente en el símbolo del sistema:

```
list providers
```

La siguiente salida muestra un solo proveedor, que se usará de forma predeterminada:

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

Para agregar la unidad C: al conjunto de instantáneas y asignar un alias denominado *System1*, escriba:

```
add volume c: alias System1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [establecer contexto (comando)](set-context.md)
