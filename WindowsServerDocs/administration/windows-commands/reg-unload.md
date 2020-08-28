---
title: reg unload
description: Artículo de referencia para el comando reg Unload, que quita una sección del registro cargada mediante la operación reg Load.
ms.topic: reference
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4f0ba24fd2d6dead3ff4f224584b56adbfd8425
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024999"
---
# <a name="reg-unload"></a>reg unload

Quita una sección del registro que se cargó con la operación de **carga de reg** .

## <a name="syntax"></a>Sintaxis

```
reg unload <keyname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **descarga de reg** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

## <a name="examples"></a>Ejemplos

Para descargar el subárbol TempHive en el archivo HKLM, escriba:

```
reg unload HKLM\TempHive
```

> [!CAUTION]
> No edite el registro directamente a menos que no tenga ninguna alternativa. El editor del registro omite las medidas de seguridad estándar, lo que permite valores que pueden degradar el rendimiento, dañar el sistema o incluso requerir que se vuelva a instalar Windows. Puede modificar la mayoría de la configuración del registro de forma segura mediante los programas del panel de control o Microsoft Management Console (MMC). Si debe modificar el registro directamente, haga una copia de seguridad en primer lugar.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG Load (comando)](reg-load.md)
