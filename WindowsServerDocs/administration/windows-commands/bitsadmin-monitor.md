---
title: bitsadmin monitor
description: Tema de comandos de Windows para el **monitor de bitsadmin**, que supervisa los trabajos de la cola de transferencia que son propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bda268afd5fda24bba2afb04b32bac9cda9a05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850218"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

Supervisa los trabajos de la cola de transferencia que son propiedad del usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| /allusers | Opcional. Supervisa los trabajos de todos los usuarios. Debe tener privilegios de administrador para usar este parámetro. |
| demorará | Opcional. Actualiza los datos en un intervalo especificado por `<seconds>`. El intervalo de actualización predeterminado es de cinco segundos. Para detener la actualización, presione CTRL + C. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el siguiente ejemplo se supervisa la cola de transferencia para los trabajos que pertenecen al usuario actual y se actualiza la información cada 60 segundos.

```
C:\>bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)