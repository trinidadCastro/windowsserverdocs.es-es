---
title: ™
description: Artículo de referencia para los comandos reg, que realizan operaciones en la información y los valores de subclaves del registro en entradas del registro.
ms.topic: reference
ms.assetid: c97496b2-d1ff-4887-b5d2-6e1524be465a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a39fc22c2fe845d8cbbb64cf751455316bdc8747
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639493"
---
# <a name="reg"></a>™

Realiza operaciones en la información y los valores de subclaves del registro en entradas del registro.

Algunas operaciones permiten ver o configurar las entradas del registro en equipos locales o remotos, mientras que otras permiten configurar solo equipos locales. El uso de **reg** para configurar el registro de equipos remotos limita los parámetros que se pueden usar en algunas operaciones. Compruebe la sintaxis y los parámetros de cada operación para comprobar que se pueden usar en equipos remotos.

> [!CAUTION]
> No edite el registro directamente a menos que no tenga ninguna alternativa. El editor del registro omite las medidas de seguridad estándar, lo que permite valores que pueden degradar el rendimiento, dañar el sistema o incluso requerir que se vuelva a instalar Windows. Puede modificar la mayoría de la configuración del registro de forma segura mediante los programas del panel de control o Microsoft Management Console (MMC). Si debe modificar el registro directamente, haga una copia de seguridad en primer lugar.

## <a name="syntax"></a>Sintaxis

```
reg add
reg compare
reg copy
reg delete
reg export
reg import
reg load
reg query
reg restore
reg save
reg unload
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [reg add](reg-add.md) | Agrega una nueva subclave o entrada al registro. |
| [reg compare](reg-compare.md) | Compara las subclaves o entradas del registro especificadas. |
| [reg copy](reg-copy.md) | Copia una entrada del registro en una ubicación especificada en el equipo local o remoto. |
| [reg delete](reg-delete.md) | Elimina una subclave o entradas del registro. |
| [reg export](reg-export.md) | Copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores. |
| [reg import](reg-import.md) | Copia el contenido de un archivo que contiene las subclaves del registro exportadas, las entradas y los valores en el registro del equipo local. |
| [reg load](reg-load.md) | Escribe las subclaves y entradas guardadas en una subclave diferente del registro. |
| [reg query](reg-query.md) | Devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada del registro. |
| [reg restore](reg-restore.md) | Vuelve a escribir las subclaves y entradas guardadas en el registro. |
| [reg save](reg-save.md) | Guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado. |
| [reg unload](reg-unload.md) | Quita una sección del registro que se cargó con la operación de **carga de reg** . |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
