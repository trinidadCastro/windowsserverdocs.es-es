---
title: change logon
description: Artículo de referencia del comando Change Logon, que habilita o deshabilita los inicios de sesión de las sesiones de cliente, o muestra el estado de inicio de sesión actual.
ms.topic: reference
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 55732dc5803f4ac783828293f5da839cca364b40
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629903"
---
# <a name="change-logon"></a>change logon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita o deshabilita los inicios de sesión de las sesiones de cliente o muestra el estado de inicio de sesión actual. Esta utilidad es útil para el mantenimiento del sistema. Debe ser un administrador para ejecutar este comando.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /Query | Muestra el estado de inicio de sesión actual, si está habilitado o deshabilitado. |
| /Enable | Habilita los inicios de sesión desde las sesiones de cliente, pero no desde la consola. |
| /Disable | Deshabilita los inicios de sesión posteriores de las sesiones de cliente, pero no de la consola. No afecta a los usuarios que han iniciado sesión actualmente. |
| /drain | Deshabilita los inicios de sesión de las nuevas sesiones de cliente, pero permite las reconexiones a las sesiones existentes. |
| /drainuntilrestart | Deshabilita los inicios de sesión de nuevas sesiones de cliente hasta que se reinicia el equipo, pero permite las reconexiones a las sesiones existentes. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los inicios de sesión se vuelven a habilitar al reiniciar el sistema.

- Si está conectado al servidor host de sesión de Escritorio remoto desde una sesión de cliente y, a continuación, deshabilita los inicios de sesión y cierra la sesión antes de volver a habilitar los inicios de sesión, no podrá volver a conectarse a la sesión. Para volver a habilitar los inicios de sesión desde las sesiones de cliente, inicie sesión en la consola de.

### <a name="examples"></a>Ejemplos

- Para mostrar el estado de inicio de sesión actual, escriba:

  ```
  change logon /query
  ```

- Para habilitar los inicios de sesión desde las sesiones de cliente, escriba:

  ```
  change logon /enable
  ```

- Para deshabilitar los inicios de sesión de cliente, escriba:

  ```
  change logon /disable
  ```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [cambiar comando](change.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
