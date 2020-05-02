---
title: bitsadmin setcredentials
description: Tema de referencia del comando bitsadmin SetCredentials, que agrega credenciales a un trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fbedcc65931e7d3cfb1719786f423b0d071b411
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719314"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Agrega credenciales a un trabajo.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setcredentials <job> <target> <scheme> <username> <password>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| Destino | Usar **servidor** o **proxy**. |
| scheme | Use uno de los siguientes:<ul><li>**Fundamentales.** Esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto no cifrado al servidor o proxy.</li><li>**Comprobación.** Un esquema de autenticación de desafío-respuesta que usa una cadena de datos especificada por el servidor para el desafío.</li><li>**NTLM.** Un esquema de autenticación de desafío-respuesta que usa las credenciales del usuario para la autenticación en un entorno de red de Windows.</li><li>**NEGOTIATE (también conocido como protocolo de negociación simple y protegida).** Un esquema de autenticación de desafío-respuesta que negocia con el servidor o proxy para determinar el esquema que se va a utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</li><li>**Cuenta.** Servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.</li></ul> |
| nombre_de_usuario | Nombre del usuario. |
| password | La contraseña asociada al nombre de *usuario*proporcionado. |

## <a name="examples"></a>Ejemplos

Para agregar credenciales al trabajo denominado *myDownloadJob*:

```
bitsadmin /setcredentials myDownloadJob SERVER BASIC Edward password20
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
