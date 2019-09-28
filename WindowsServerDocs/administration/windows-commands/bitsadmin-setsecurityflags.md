---
title: bitsadmin setsecurityflags
description: 'El tema comandos de Windows para **bitsadmin setsecurityflags** : establece marcas para http que determinan si los bits deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a usar cuando un servidor redirija la solicitud HTTP.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acc5a64ef7c82b14e6815b6d51dda5ea4700dcad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380410"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags



Establece marcas para HTTP que determinan si los BITS deben comprobar la lista de revocación de certificados, omitir determinados errores de certificado y definir la Directiva que se va a utilizar cuando un servidor redirija la solicitud HTTP. El valor es un entero sin signo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Valor|Ver comentarios|

## <a name="remarks"></a>Comentarios

El parámetro **Value** puede contener una o varias de las siguientes marcas de notificación.

|.|Representación binaria|
|------|---------------------|
|Habilitar comprobación de CRL|Establecer el bit menos significativo|
|Omitir nombre común no válido en el certificado de servidor|Establecer el segundo bit de la derecha|
|Omitir fecha no válida en certificado de servidor|Establecer el tercer bit desde la derecha|
|Omitir la entidad de certificación no válida en el certificado de servidor|Establecer el cuarto bit desde la derecha|
|Omitir el uso no válido del certificado|Establecer el quinto bit desde la derecha|
|Directiva de redireccionamiento|Controlado por el noveno a 11 bits desde la derecha</br>0, 0-se permitirán automáticamente las redirecciones.</br>0, 0, 1: el nombre remoto de la interfaz IBackgroundCopyFile se actualizará si se produce una redirección.</br>0, 1, 0: los BITS producirán un error en el trabajo si se produce una redirección.|
|Permitir redirección de HTTPS a HTTP|Establecer el 12 bit a la derecha|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establecen las marcas de seguridad para habilitar una comprobación de CRL para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)