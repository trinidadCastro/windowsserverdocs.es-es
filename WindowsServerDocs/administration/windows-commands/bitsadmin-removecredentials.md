---
title: bitsadmin removecredentials
description: Tema de comandos de Windows para bitsadmin **removecredentials**, que quita las credenciales de un trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ff7e2a813c7cc6b60e04d55ef63804a2aed796
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849848"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Quita las credenciales de un trabajo.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| target | Usar **servidor** o **proxy**. |
| scheme | Use una de las siguientes opciones:<ul><li>**fundamentales.** Esquema de autenticación en el que el nombre de usuario y la contraseña se envían en texto no cifrado al servidor o proxy.</li><li>**comprobación.** Un esquema de autenticación de desafío-respuesta que usa una cadena de datos especificada por el servidor para el desafío.</li><li>**NTLM.** Un esquema de autenticación de desafío-respuesta que usa las credenciales del usuario para la autenticación en un entorno de red de Windows.</li><li>**Negotiate (también conocido como protocolo de negociación simple y protegida).** Un esquema de autenticación de desafío-respuesta que negocia con el servidor o proxy para determinar el esquema que se va a utilizar para la autenticación. Algunos ejemplos son el protocolo Kerberos y NTLM.</li><li>**cuenta.** Servicio de autenticación centralizado proporcionado por Microsoft que ofrece un inicio de sesión único para los sitios miembro.</li></ul> |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se quitan las credenciales del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /removecredentials myDownloadJob server basic
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)