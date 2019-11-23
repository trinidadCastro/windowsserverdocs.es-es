---
title: 'ksetup: listrealmflags'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f103875dc10dfbf7b0c604a8e2060fe58ee7a92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374972"
---
# <a name="ksetuplistrealmflags"></a>ksetup: listrealmflags



Muestra las marcas de dominio Kerberos disponibles a las que puede informar **ksetup**. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Parámetros

Ninguno

## <a name="remarks"></a>Observaciones

Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos no basado en Windows. Los equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 pueden usar un servidor Kerberos no basado en Windows para administrar la autenticación en lugar de usar un dominio que ejecute un sistema operativo Windows Server. Estos sistemas participan en un dominio Kerberos en lugar de en un dominio de Windows. Esta entrada establece las características del dominio Kerberos. En la tabla siguiente se describe cada uno de ellos.

|Valor|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|Todos|Se establecen todas las marcas de dominio Kerberos.|
|0x00|Ninguno|No se establecen marcas de dominio Kerberos y no se habilitan características adicionales.|
|0x01|SendAddress|La dirección IP se incluirá en los vales de concesión de vales.|
|0x02|TcpSupported|El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos.|
|0x04|Delegado|Todos los usuarios de este dominio Kerberos son de confianza para la delegación.|
|0x08|NcSupported|Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos.|
|0x80|RC4|Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS.|

Las marcas de dominio Kerberos se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>nombre de dominio Kerberos</em>. Esta entrada no existe en el registro de forma predeterminada. Puede usar el comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

## <a name="BKMK_Examples"></a>Example

Enumerar las marcas de dominio Kerberos conocidas en este equipo:
```
ksetup /listrealmflags
```
Establezca las marcas de dominio Kerberos disponibles que **Ksetup** no conoce escribiendo cualquiera de los siguientes comandos en la línea de comandos:
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)