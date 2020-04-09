---
title: crear
description: Comando comandos de Windows para crear, que inicia el proceso de creación de instantáneas, con la configuración de contexto y opciones actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d29285517ca678a15828079c95663fc4d501eaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846838"
---
# <a name="create"></a>crear

Inicia el proceso de creación de instantáneas con la configuración de contexto y opciones actual. Requiere al menos un volumen en el conjunto de instantáneas.

## <a name="syntax"></a>Sintaxis

```
create
```

## <a name="remarks"></a>Comentarios

-   Debe agregar al menos un volumen con el comando **Add Volume** para poder usar el comando **Create** .
-   Puede usar el comando **Begin backup** para especificar una copia de seguridad completa, en lugar de una copia de seguridad de copia.
-   Después de ejecutar el comando **Create** , puede usar el comando **exec** para ejecutar un script de duplicación para la copia de seguridad de la instantánea.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)