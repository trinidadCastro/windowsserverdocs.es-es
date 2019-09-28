---
title: bitsadmin setcredentials
description: En el tema comandos de Windows para **bitsadmin SetCredentials** , se agregan credenciales a un trabajo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70ac9a01a2e713b5a2fb881f327a52552a6bbec6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380725"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Agrega credenciales a un trabajo.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Destino|SERVIDOR o PROXY|
|regímenes|es uno de los siguientes:</br>-BASIC: esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto no cifrado al servidor o proxy.</br>-DIGEST: un esquema de autenticación de desafío-respuesta que usa una cadena de datos especificada por el servidor para el desafío.</br>-NTLM: un esquema de autenticación de desafío-respuesta que usa las credenciales del usuario para la autenticación en un entorno de red de Windows.</br>-NEGOTIATE, también conocido como el protocolo de negociación simple y protegido (Snego) es un esquema de autenticación de desafío-respuesta que negocia con el servidor o proxy para determinar el esquema que se va a utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</br>-PASSPORT: servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.|
|Nombre de usuario|El nombre de las credenciales proporcionadas|
|Contraseña|La contraseña asociada al nombre de *usuario* proporcionado.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se agregan credenciales al trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)