---
title: bitsadmin setsecurityflags
description: Comando comandos de Windows para **bitsadmin setsecurityflags**, que establece marcas de seguridad para http para determinar si los bits deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a usar cuando un servidor redirija la solicitud HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d73361bceda8c0eb24992bdee176b47bf82a878
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122724"
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
| value | Puede incluir una o varias de las siguientes marcas de notificación, entre las que se incluyen:<ul><li>Establezca el bit menos significativo para habilitar la comprobación de CRL.</li><li>Establezca el segundo bit de la derecha para omitir los nombres comunes incorrectos en el certificado de servidor.</li><li>Establezca el tercer bit de la derecha para omitir las fechas incorrectas en el certificado de servidor.</li><li>Establezca el cuarto bit de la derecha para omitir las entidades de certificación incorrectas en el certificado de servidor.</li><li>Establezca el quinto bit de la derecha para omitir el uso incorrecto del certificado de servidor.</li><li>Establezca el 9 por los undécimo bits de la derecha para implementar la Directiva de redireccionamiento especificada, entre las que se incluyen:<ul><li>**0, 0,0.** Las redirecciones se permiten automáticamente.</li><li>**0, 0, 1.** El nombre remoto de la interfaz **IBackgroundCopyFile** se actualiza si se produce una redirección.</li><li>**0, 1, 0.** BITS produce un error en el trabajo si se produce una redirección.</li></ul></li><li>Establezca el 12 bit de la derecha para permitir el redireccionamiento de HTTPS a HTTP.</li></ul> |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establecen las marcas de seguridad para habilitar una comprobación de CRL para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)