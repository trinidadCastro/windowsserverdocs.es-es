---
title: nslookup set
description: Tema de referencia del comando Nslookup Set, que cambia las opciones de configuración que afectan a cómo se comportan las búsquedas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1fe5b36d-e93e-468b-abca-43b0204b32d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579334b3b6b0cd5e9373876144f46fa21d57c745
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721188"
---
# <a name="nslookup-set"></a>nslookup set

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia los valores de configuración que afectan a cómo funcionan las búsquedas.

## <a name="syntax"></a>Sintaxis

```
set all [class | d2 | debug | domain | port | querytype | recurse | retry | root | search | srchlist | timeout | type | vc] [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [nslookup set all](nslookup-set-all.md) | Muestra todas las opciones de configuración actuales. |
| [nslookup set class](nslookup-set-class.md) | Cambia la clase de consulta, que especifica el grupo de protocolos de la información. |
| [nslookup set d2](nslookup-set-d2.md) | Activa o desactiva el modo de depuración detallado. |
| [nslookup set debug](nslookup-set-debug.md) | Desactiva por completo el modo de depuración. |
| [nslookup set domain](nslookup-set-domain.md) | Cambia el nombre de dominio del sistema de nombres de dominio (DNS) predeterminado por el nombre especificado. |
| [nslookup set port](nslookup-set-port.md) | Cambia el puerto del servidor de nombres del sistema de nombres de dominio (DNS) TCP/UDP predeterminado al valor especificado.
| [nslookup set querytype](nslookup-set-querytype.md) | Cambia el tipo de registro de recursos de la consulta. |
| [nslookup set recurse](nslookup-set-recurse.md) | Indica al servidor de nombres del sistema de nombres de dominio (DNS) que consulte a otros servidores si no encuentra ninguna información. |
| [nslookup set retry](nslookup-set-retry.md) | Establece el número de reintentos. |
| [nslookup set root](nslookup-set-root.md) | Cambia el nombre del servidor raíz que se usa para las consultas. |
| [nslookup set search](nslookup-set-search.md) | Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Cambia el nombre de dominio y la lista de búsqueda predeterminados del sistema de nombres de dominio (DNS). |
| [nslookup set timeout](nslookup-set-timeout.md) | Cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda. |
| [nslookup set type](nslookup-set-type.md) | Cambia el tipo de registro de recursos de la consulta. |
| [nslookup set vc](nslookup-set-vc.md) | Especifica si se debe usar un circuito virtual al enviar solicitudes al servidor. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
