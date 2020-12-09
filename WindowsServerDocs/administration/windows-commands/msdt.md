---
title: msdt
description: Artículo de referencia del comando MSDT, que invoca un paquete de solución de problemas en la línea de comandos o como parte de un script automatizado y habilita opciones adicionales sin intervención del usuario.
ms.topic: reference
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0270039fcbc9f99ff2569635ddd75918baed783e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865414"
---
# <a name="msdt"></a>msdt

Invoca un paquete de solución de problemas en la línea de comandos o como parte de un script automatizado y habilita opciones adicionales sin intervención del usuario.

## <a name="syntax"></a>Sintaxis

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /ID `<packagename>` | Especifica el paquete de diagnóstico que se va a ejecutar. Para obtener una lista de los paquetes disponibles, consulte los [paquetes de solución de problemas disponibles](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs). |
| /path `<directory|.diagpkg file|.diagcfg file>` | Especifica la ruta de acceso completa a un paquete de diagnóstico. Si especifica un directorio, el directorio debe contener un paquete de diagnóstico. No se puede usar el parámetro **/path** junto con los parámetros * */ID * *, **/DCI** o **/Cab** |                                                                                   |
| /dci `<passkey>` | Rellena previamente el campo clave de paso. Este parámetro solo se usa cuando un proveedor de soporte técnico ha proporcionado una clave de paso. |
| /DT `<directory>` | Muestra el historial de solución de problemas en el directorio especificado. Los resultados de diagnóstico se almacenan en los directorios **%LOCALAPPDATA%\Diagnostics** o **%LOCALAPPDATA%\ElevatedDiagnostics** del usuario. |
| /af `<answerfile>` | Especifica un archivo de respuesta en formato XML que contiene respuestas a una o más interacciones de diagnóstico. |
| /modal `<ownerHWND>` | Convierte el módulo de solución de problemas en modal en una ventana designada por el identificador de la ventana de consola primaria (HWND), en formato decimal. Este parámetro lo suelen usar las aplicaciones que inician un paquete de solución de problemas. Para obtener más información sobre cómo obtener los identificadores de la ventana de la consola, vea [Cómo obtener un identificador de ventana de consola (HWND)](https://support.microsoft.com/help/124103/how-to-obtain-a-console-window-handle-hwnd). |
| /moreoptions `<true|false>` | Habilita (true) o suprime (false) la pantalla de solución de problemas final que pregunta si el usuario desea explorar opciones adicionales. Este parámetro se utiliza normalmente cuando el paquete de solución de problemas lo inicia un solucionador de problemas que no forma parte del sistema operativo. |
| /param Returns `<parameters>` | Especifica un conjunto de respuestas de interacción en la línea de comandos, similar a un archivo de respuesta. Este parámetro no se usa normalmente en el contexto de los paquetes de solución de problemas creados con el diseñador de TSP. Para obtener más información sobre el desarrollo de parámetros personalizados, consulte [plataforma de solución de problemas de Windows](/previous-versions/windows/desktop/wintt/windows-troubleshooting-toolkit-portal). |
| /advanced | Expande el vínculo avanzado en la página de bienvenida de forma predeterminada cuando se inicia el paquete de solución de problemas. |
| /custom | Solicita al usuario que confirme cada posible solución antes de aplicarla. |

### <a name="return-codes"></a>Códigos de retorno

Los paquetes de solución de problemas comprenden un conjunto de causas raíz, cada una de las cuales describe un problema técnico específico. Después de completar las tareas del paquete de solución de problemas, cada causa raíz devuelve un estado de Fixed, no Fixed, detectado (pero no fixable) o no encontrado. Además de los resultados específicos que se indican en la interfaz de usuario del solucionador de problemas, el motor de solución de problemas devuelve un código en los resultados que describen, en términos generales, si el solucionador corrigió o no el problema original. Los códigos son:

| Código | Descripción |
| ---- | ----------- |
| -1 | **Interrupción:** El solucionador de problemas se cerró antes de que se completaran las tareas de solución de problemas. |
| 0 | Problema **corregido:** El solucionador de problemas identificó y corrigió al menos una causa raíz, y ninguna causa raíz permanece en un estado no fijo. |
| 1 | **Presente, pero sin corregir:** El solucionador de problemas identificó una o más causas raíz que permanecen en un estado no fijo. Este código se devuelve incluso si se corrigió otra causa raíz. |
| 2 | **No encontrado:** El solucionador de problemas no identificó ninguna causa raíz. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Paquetes de solución de problemas disponibles](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)

- [Referencia de PowerShell para TroubleshootingPack](/powershell/module/troubleshootingpack/)
