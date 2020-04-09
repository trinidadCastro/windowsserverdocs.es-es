---
title: bitsadmin getproxyusage
description: Tema de comandos de Windows para **bitsadmin getproxyusage**, que recupera la configuración de uso del proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01c9bb9a1d413fa847482f652e18eed30ad76109
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850518"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

Recupera la configuración de uso del proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="remarks"></a>Comentarios

Los valores de uso del proxy incluyen:

- **Configuración** predeterminada: Use los valores predeterminados de Internet Explorer del propietario.

- **No_Proxy** : no use un servidor proxy.

- **Invalidar** : usa una lista de proxy explícita.

- **Detección** automática: detecta automáticamente la configuración de proxy.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el uso de proxy para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)