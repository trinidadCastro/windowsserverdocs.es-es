---
title: Agregar volumen
description: Tema de los comandos de Windows para **agregar volumen** -agrega volúmenes a la instantánea, que es el conjunto de volúmenes que se tomen instantáneas copiados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8960ffafdf49d4512e1df2dfcc046bdfbe56e224
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819476"
---
# <a name="add-volume"></a>Agregar volumen



Agrega los volúmenes en el conjunto de copia de instantáneas, que es el conjunto de volúmenes de instantáneas. Este comando es necesario para crear instantáneas. Si se utiliza sin parámetros, **agregar volumen** muestra la Ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Volume>|Especifica un volumen que se agrega al conjunto de copia sombra. Al menos un volumen es necesario para la creación de instantáneas.|
|[proveedor \<ProviderID >]|Especifica el identificador del proveedor de un proveedor registrado se usa para crear la instantánea. Si **proveedor** no se especifica, se utiliza el proveedor predeterminado.|

## <a name="remarks"></a>Comentarios

-   Los volúmenes se agregan una vez.
-   Cada vez que se ha agregado un volumen, se comprueba para asegurarse de que VSS es compatible con la creación de instantáneas de dicho volumen. Esta comprobación principal puede invalidar, sin embargo, su uso posterior de la **establecer el contexto de** comando.
-   Cuando se crea una instantánea, una variable de entorno vincula el alias para el identificador de la sombra, por lo que el alias, a continuación, se puede utilizar para generar scripts.

## <a name="BKMK_examples"></a>Ejemplos

Para ver la lista actual de los proveedores registrados en el `DISKSHADOW>` , escriba:
```
list providers
```
La salida siguiente muestra un proveedor único, que se usará de forma predeterminada:
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```
Para agregar la unidad C para el conjunto de copia de instantáneas y asignar un alias denominado System1, escriba:
```
add volume c: alias System1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)