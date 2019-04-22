---
title: bitsadmin removecredentials
description: Tema de los comandos de Windows para **removecredentials bitsadmin** -quita credenciales de un trabajo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cbd65442ff0d74ec1179a49df5d4a94785f3dd25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822296"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Quita credenciales de un trabajo.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Destino|SERVIDOR o PROXY|
|Scheme|es uno de los siguientes:</br>-BÁSICOS: esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto sin cifrar en el servidor o proxy.</br>-IMPLÍCITA, un esquema de autenticación de desafío / respuesta que usa una cadena de datos especificado por el servidor para el desafío.</br>-NTLM, un esquema de autenticación de desafío / respuesta que utiliza las credenciales del usuario para la autenticación en un entorno de red de Windows.</br>-NEGOTIATE, también conocido como el protocolo Simple y protegida de negociación (Snego) es un esquema de autenticación de desafío / respuesta que negocia con el servidor o proxy para determinar qué esquema utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</br>-PASSPORT, un servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente quita las credenciales del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)