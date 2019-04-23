---
title: Metadatos del conjunto
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82d2cd9ba447a0ea261f91dc01c11e45dfc0aa9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835996"
---
# <a name="set-metadata"></a>Metadatos del conjunto



Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas utilizado para transferir instantáneas de un equipo a otro. Si se utiliza sin parámetros, **establecer metadatos** muestra la Ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica la ubicación para crear el archivo de metadatos.|
|\<MetaData.cab>|Especifica el nombre del archivo cab para almacenar metadatos de creación de instantáneas.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)