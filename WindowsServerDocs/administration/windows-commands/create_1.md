---
title: create
description: Tema de referencia de Create, que inicia el proceso de creación de instantáneas, utilizando la configuración de contexto y opciones actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfddbebd5744d8cd222d67e46690ce8b5d2e0fde
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716837"
---
# <a name="create"></a>create

Inicia el proceso de creación de instantáneas con la configuración de contexto y opciones actual. Requiere al menos un volumen en el conjunto de instantáneas.

## <a name="syntax"></a>Sintaxis

```
create
```

## <a name="remarks"></a>Observaciones

-   Debe agregar al menos un volumen con el comando **Add Volume** para poder usar el comando **Create** .
-   Puede usar el comando **Begin backup** para especificar una copia de seguridad completa, en lugar de una copia de seguridad de copia.
-   Después de ejecutar el comando **Create** , puede usar el comando **exec** para ejecutar un script de duplicación para la copia de seguridad de la instantánea.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)