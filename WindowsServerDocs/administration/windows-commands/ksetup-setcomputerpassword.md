---
title: ksetup setcomputerpassword
description: Tema de referencia del comando ksetup setcomputerpassword, que establece la contraseña para el equipo local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ec410cd85c13cb3a925c3fc65b8c9f86fba606a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817435"
---
# <a name="ksetup-setcomputerpassword"></a>ksetup setcomputerpassword

Establece la contraseña del equipo local. Este comando afecta solo a la cuenta de equipo y requiere un reinicio para que el cambio de contraseña surta efecto.

> [!IMPORTANT]
> La contraseña de la cuenta de equipo no se muestra en el registro o como salida del comando **ksetup** .

## <a name="syntax"></a>Sintaxis

```
ksetup /setcomputerpassword <password>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<password>` | Especifica la contraseña proporcionada para establecer la cuenta de equipo en el equipo local. La contraseña solo se puede establecer mediante una cuenta con privilegios de administrador y la contraseña debe tener entre 1 y 156 caracteres alfanuméricos o especiales. |

### <a name="examples"></a>Ejemplos

Para cambiar la contraseña de la cuenta de equipo en el equipo local de *IPops897* a *IPop $897!*, escriba:

```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)
