---
title: ksetup listrealmflags
description: Artículo de referencia del comando ksetup listrealmflags, en el que se enumeran las marcas de dominio Kerberos disponibles que puede informar de ksetup.
ms.topic: reference
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7c522449053a18cdd1e2a9e533dbce5d6e9f17c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025479"
---
# <a name="ksetup-listrealmflags"></a>ksetup listrealmflags

Muestra las marcas de dominio Kerberos disponibles a las que puede informar **ksetup**.

## <a name="syntax"></a>Sintaxis

```
ksetup /listrealmflags
```

### <a name="remarks"></a>Observaciones

- Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos que no se basan en el sistema operativo Windows Server. Los equipos que ejecutan Windows Server pueden usar un servidor Kerberos para administrar la autenticación en el dominio Kerberos, en lugar de usar un dominio que ejecute un sistema operativo Windows Server. Esta entrada establece las características del dominio Kerberos y son las siguientes:

| Valor | Marca de dominio Kerberos | Descripción |
| ----- | ---------- | ----------- |
| 0xF | All | Se establecen todas las marcas de dominio Kerberos. |
| 0x00 | None | No se establecen marcas de dominio Kerberos y no se habilitan características adicionales. |
| 0x01 | sendaddress | La dirección IP se incluirá en los vales de concesión de vales. |
| 0x02 | tcpsupported | El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos. |
| 0x04 | delegado | Todos los usuarios de este dominio Kerberos son de confianza para la delegación. |
| 0x08 | ncsupported | Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos. |
| 0x80 | RC4 | Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS. |

- Las marcas de dominio Kerberos se almacenan en el registro en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Esta entrada no existe en el registro de forma predeterminada. Puede usar el [comando ksetup addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

## <a name="examples"></a>Ejemplos

Para enumerar las marcas de dominio Kerberos conocidas en este equipo, escriba:

```
ksetup /listrealmflags
```

Para establecer las marcas de dominio Kerberos disponibles que **ksetup** no conoce, escriba:

```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```

**De**

```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup addrealmflags, comando](ksetup-addrealmflags.md)

- [ksetup setrealmflags, comando](ksetup-setrealmflags.md)

- [ksetup delrealmflags, comando](ksetup-delrealmflags.md)
