---
title: ksetup addrealmflags
description: Tema de referencia del comando ksetup addrealmflags, que agrega marcas de dominio Kerberos adicionales al dominio Kerberos especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0862462f47189f4904421943e4d3de55c856ace
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818035"
---
# <a name="ksetup-addrealmflags"></a>ksetup addrealmflags

Agrega marcas de dominio Kerberos adicionales al dominio Kerberos especificado.

## <a name="syntax"></a>Sintaxis

```
ksetup /addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM. |

#### <a name="remarks"></a>Observaciones

- Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos que no se basan en el sistema operativo Windows Server. Los equipos que ejecutan Windows Server pueden usar un servidor Kerberos para administrar la autenticación en el dominio Kerberos, en lugar de usar un dominio que ejecute un sistema operativo Windows Server. Esta entrada establece las características del dominio Kerberos y son las siguientes:

| Value | Marca de dominio Kerberos | Descripción |
| ----- | ---------- | ----------- |
| 0xF | All | Se establecen todas las marcas de dominio Kerberos. |
| 0x00 | None | No se establecen marcas de dominio Kerberos y no se habilitan características adicionales. |
| 0x01 | sendaddress | La dirección IP se incluirá en los vales de concesión de vales. |
| 0x02 | tcpsupported | El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos. |
| 0x04 | delegado | Todos los usuarios de este dominio Kerberos son de confianza para la delegación. |
| 0x08 | ncsupported | Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos. |
| 0x80 | RC4 | Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS. |

- Las marcas de dominio Kerberos se almacenan en el registro en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Esta entrada no existe en el registro de forma predeterminada. Puede usar el comando [ksetup addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

- Puede ver las marcas disponibles y establecer el dominio Kerberos mediante la visualización de la salida de **ksetup** o `ksetup /dumpstate` .

### <a name="examples"></a>Ejemplos

Para enumerar las marcas de dominio Kerberos disponibles para el dominio Kerberos CONTOSO, escriba:

```
ksetup /listrealmflags
```

Para establecer dos marcas en el dominio de CONTOSO, escriba:

```
ksetup /setrealmflags CONTOSO ncsupported delegate
```

Para agregar una marca más que no esté actualmente en el conjunto, escriba:

```
ksetup /addrealmflags CONTOSO SendAddress
```

Para comprobar que se ha establecido la marca de dominio Kerberos, escriba `ksetup` y, a continuación, vea la salida y busque el texto, **marcas de dominio Kerberos =**. Si no ve el texto, significa que no se ha establecido la marca.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup listrealmflags, comando](ksetup-listrealmflags.md)

- [ksetup setrealmflags, comando](ksetup-setrealmflags.md)

- [ksetup delrealmflags, comando](ksetup-delrealmflags.md)

- [ksetup dumpstate, comando](ksetup-dumpstate.md)
