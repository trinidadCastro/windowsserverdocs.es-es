---
title: crear
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70a8f53d9cb90fc36a76b11de2f93da71874617a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881276"
---
# <a name="create"></a>crear



inicia el proceso de creación de la copia de instantáneas, usando la configuración actual de contexto y la opción. Requiere al menos un volumen en el conjunto de copia sombra.

## <a name="syntax"></a>Sintaxis

```
create
```

## <a name="remarks"></a>Comentarios

-   Debe agregar al menos un volumen con la **agregar volumen** comando antes de poder usar el **crear** comando.
-   Puede usar el **comenzar la copia de seguridad** comando para especificar una copia de seguridad completa, en lugar de una copia de seguridad.
-   Después de ejecutar el **crear** de comandos, puede usar el **exec** comando para ejecutar una secuencia de comandos de duplicación para copia de seguridad de la instantánea.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)