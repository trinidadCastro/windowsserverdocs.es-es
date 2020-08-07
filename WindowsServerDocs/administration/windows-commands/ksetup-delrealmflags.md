---
title: ksetup delrealmflags
description: Artículo de referencia para el comando ksetup delrealmflags, que quita las marcas de dominio Kerberos del dominio Kerberos especificado.
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07d177f58f950b5e8e552e69c9f79054a379cc10
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887932"
---
# <a name="ksetup-delrealmflags"></a>ksetup delrealmflags

Quita las marcas de dominio Kerberos del dominio Kerberos especificado.

## <a name="syntax"></a>Sintaxis

```
ksetup /delrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el **dominio Kerberos** predeterminados = cuando se ejecuta **ksetup** . |

#### <a name="remarks"></a>Observaciones

- Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos que no se basan en el sistema operativo Windows Server. Los equipos que ejecutan Windows Server pueden usar un servidor Kerberos para administrar la autenticación en el dominio Kerberos, en lugar de usar un dominio que ejecute un sistema operativo Windows Server. Esta entrada establece las características del dominio Kerberos y son las siguientes:

| Valor | Marca de dominio Kerberos | Descripción |
| ----- | ---------- | ----------- |
| 0xF | Todas | Se establecen todas las marcas de dominio Kerberos. |
| 0x00 | None | No se establecen marcas de dominio Kerberos y no se habilitan características adicionales. |
| 0x01 | sendaddress | La dirección IP se incluirá en los vales de concesión de vales. |
| 0x02 | tcpsupported | El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos. |
| 0x04 | delegado | Todos los usuarios de este dominio Kerberos son de confianza para la delegación. |
| 0x08 | ncsupported | Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos. |
| 0x80 | RC4 | Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS. |

- Las marcas de dominio Kerberos se almacenan en el registro en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Esta entrada no existe en el registro de forma predeterminada. Puede usar el [comando ksetup addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

- Puede ver las marcas disponibles y establecer el dominio Kerberos mediante la visualización de la salida de **ksetup** o `ksetup /dumpstate` .

### <a name="examples"></a>Ejemplos

Para enumerar las marcas de dominio Kerberos disponibles para el dominio Kerberos CONTOSO, escriba:

```
ksetup /listrealmflags
```

Para quitar dos marcas actualmente en el conjunto, escriba:

```
ksetup /delrealmflags CONTOSO ncsupported delegate
```

Para comprobar que se han quitado las marcas de dominio Kerberos, escriba `ksetup` y, a continuación, vea la salida para buscar el texto, **marcas de dominio Kerberos =**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup listrealmflags, comando](ksetup-listrealmflags.md)

- [ksetup setrealmflags, comando](ksetup-setrealmflags.md)

- [ksetup addrealmflags, comando](ksetup-addrealmflags.md)

- [ksetup dumpstate, comando](ksetup-dumpstate.md)
