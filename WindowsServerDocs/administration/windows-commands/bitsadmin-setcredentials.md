---
title: bitsadmin setcredentials
description: Tema de comandos de Windows para bitsadmin SetCredentials, que agrega credenciales a un trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918bda93407e029cedaaf5eab937d1bb23dc3c4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849648"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Agrega credenciales a un trabajo.

**BITS 1,2 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Destino|SERVIDOR o PROXY|
|Regímenes|Uno de los siguientes:</br>-BASIC: esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto no cifrado al servidor o proxy.</br>-DIGEST: un esquema de autenticación de desafío-respuesta que usa una cadena de datos especificada por el servidor para el desafío.</br>-NTLM: un esquema de autenticación de desafío-respuesta que usa las credenciales del usuario para la autenticación en un entorno de red de Windows.</br>-NEGOTIATE, también conocido como el protocolo de negociación simple y protegido (Snego) es un esquema de autenticación de desafío-respuesta que negocia con el servidor o proxy para determinar el esquema que se va a utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</br>-PASSPORT: servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.|
|Nombre de usuario|El nombre de las credenciales proporcionadas|
|Password|La contraseña asociada al nombre de *usuario* proporcionado.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se agregan credenciales al trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)