---
title: bitsadmin setcredentials
description: Tema de los comandos de Windows para **bitsadmin setcredentials** -agrega credenciales a un trabajo.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 923dcff7d268d40b72db3254e2a97c808c7c7253
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877396"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Agrega credenciales a un trabajo.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Destino|SERVIDOR o PROXY|
|Scheme|es uno de los siguientes:</br>-BÁSICOS: esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto sin cifrar en el servidor o proxy.</br>-IMPLÍCITA, un esquema de autenticación de desafío / respuesta que usa una cadena de datos especificado por el servidor para el desafío.</br>-NTLM, un esquema de autenticación de desafío / respuesta que utiliza las credenciales del usuario para la autenticación en un entorno de red de Windows.</br>-NEGOTIATE, también conocido como el protocolo Simple y protegida de negociación (Snego) es un esquema de autenticación de desafío / respuesta que negocia con el servidor o proxy para determinar qué esquema utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</br>-PASSPORT, un servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.|
|Nombre de usuario|El nombre de las credenciales proporcionadas|
|Contraseña|La contraseña asociada con el proporcionado *nombre de usuario*|

## <a name="BKMK_examples"></a>Ejemplos

Las siguientes credenciales de Adds de ejemplo para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)