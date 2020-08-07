---
title: Manage-BDE keypackage
description: Artículo de referencia para el comando Manage-BDE keypackage, que genera un paquete de claves para una unidad.
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 969b9fc85959d137ec8b6bfc6b377f48e02e5157
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886878"
---
# <a name="manage-bde-keypackage"></a>Manage-BDE keypackage

Genera un paquete de claves para una unidad. El paquete de claves se puede usar junto con la herramienta de reparación para reparar unidades dañadas.

## <a name="syntax"></a>Sintaxis

```
manage-bde -keypackage [<drive>] [-ID <keyprotectoryID>] [-path <pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -ID | Crea un paquete de claves mediante el protector de clave con el identificador especificado por este valor de identificador. **Sugerencia:** Use el comando **Manage-BDE-protectors-Get** , junto con la letra de unidad para la que desea crear un paquete de claves, para obtener una lista de los GUID disponibles que se usarán como valor de identificador. |
| -path | Especifica la ubicación donde se guardará el paquete de claves creado. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para crear un paquete de claves para la unidad C, en función del protector de clave identificado por el GUID, y guardar el paquete de claves en F:\Folder, escriba:

```
manage-bde -keypackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
