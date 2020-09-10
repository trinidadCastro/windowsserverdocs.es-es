---
title: bitsadmin setsecurityflags
description: Artículo de referencia del comando bitsadmin setsecurityflags, que establece marcas de seguridad para HTTP para determinar si los BITS deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a usar cuando un servidor redirija la solicitud HTTP.
ms.topic: reference
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 771ff8ba34f82e942877158d351ed8ff760bda9d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630636"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

Establece marcas de seguridad para que HTTP determine si los BITS deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a utilizar cuando un servidor redirija la solicitud HTTP. El valor es un entero sin signo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| valor | Puede incluir una o varias de las siguientes marcas de notificación, entre las que se incluyen:<ul><li>Establezca el bit menos significativo para habilitar la comprobación de CRL.</li><li>Establezca el segundo bit de la derecha para omitir los nombres comunes incorrectos en el certificado de servidor.</li><li>Establezca el tercer bit de la derecha para omitir las fechas incorrectas en el certificado de servidor.</li><li>Establezca el cuarto bit de la derecha para omitir las entidades de certificación incorrectas en el certificado de servidor.</li><li>Establezca el quinto bit de la derecha para omitir el uso incorrecto del certificado de servidor.</li><li>Establezca el 9 por los undécimo bits de la derecha para implementar la Directiva de redireccionamiento especificada, entre las que se incluyen:<ul><li>**0, 0,0.** Las redirecciones se permiten automáticamente.</li><li>**0, 0, 1.** El nombre remoto de la interfaz **IBackgroundCopyFile** se actualiza si se produce una redirección.</li><li>**0, 1, 0.** BITS produce un error en el trabajo si se produce una redirección.</li></ul></li><li>Establezca el 12 bit de la derecha para permitir el redireccionamiento de HTTPS a HTTP.</li></ul> |

## <a name="examples"></a>Ejemplos

Para establecer las marcas de seguridad para habilitar una comprobación de CRL para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
