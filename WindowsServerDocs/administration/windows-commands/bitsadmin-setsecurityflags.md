---
title: bitsadmin setsecurityflags
description: Temas de comandos de Windows para bitsadmin setsecurityflags, que establece marcas para HTTP que determinan si los BITS deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a usar cuando un servidor redirija la solicitud HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a7b857bb398e3061a3435a730bf9a751ee2c5e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849148"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

Establece marcas para HTTP que determinan si los BITS deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a utilizar cuando un servidor redirija la solicitud HTTP. El valor es un entero sin signo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Valor|Ver comentarios|

## <a name="remarks"></a>Comentarios

El parámetro **Value** puede contener una o varias de las siguientes marcas de notificación.

|Acción|Representación binaria|
|------|---------------------|
|Habilitar comprobación de CRL|Establecer el bit menos significativo|
|Omitir nombre común no válido en el certificado de servidor|Establecer el segundo bit de la derecha|
|Omitir fecha no válida en certificado de servidor|Establecer el tercer bit desde la derecha|
|Omitir la entidad de certificación no válida en el certificado de servidor|Establecer el cuarto bit desde la derecha|
|Omitir el uso no válido del certificado|Establecer el quinto bit desde la derecha|
|Directiva de redireccionamiento|Controlado por el noveno a 11 bits desde la derecha</br>0, 0-se permitirán automáticamente las redirecciones.</br>0, 0, 1: el nombre remoto de la interfaz IBackgroundCopyFile se actualizará si se produce una redirección.</br>0, 1, 0: los BITS producirán un error en el trabajo si se produce una redirección.|
|Permitir redirección de HTTPS a HTTP|Establecer el 12 bit a la derecha|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establecen las marcas de seguridad para habilitar una comprobación de CRL para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)