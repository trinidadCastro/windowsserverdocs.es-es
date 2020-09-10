---
title: change port
description: Artículo de referencia del comando Change Port, que enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a38f4f4885ac13c40a7e2a340bf94623bcbdd77d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629896"
---
# <a name="change-port"></a>change port

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|-----------------|----------------------------------------|
| <portX>=<portY> | Asigna COM `<*portX*>` a `<*portY*>` |
| /d. <portX> | Elimina la asignación para COM `<*portX*>` |
| /Query | Muestra las asignaciones de puerto actuales. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- La mayoría de las aplicaciones de MS-DOS solo admiten puertos serie de COM1 a COM4. El comando **cambiar puerto** asigna un puerto serie a un número de puerto diferente, lo que permite que las aplicaciones que no admiten puertos com de gran número tengan acceso al puerto serie. La reasignación solo funciona para la sesión actual y no se conserva si cierra sesión en una sesión y vuelve a iniciarla.

- Use **cambiar puerto** sin parámetros para mostrar los puertos com disponibles y sus asignaciones actuales.

## <a name="examples"></a>Ejemplos

- Para asignar COM12 a COM1 para su uso por parte de una aplicación basada en MS-dos, escriba:

  ```
  change port com12=com1
  ```

- Para mostrar las asignaciones de puertos actuales, escriba:

  ```
  change port /query
  ```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [cambiar comando](change.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
