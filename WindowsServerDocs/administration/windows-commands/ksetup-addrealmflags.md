---
title: 'ksetup: addrealmflags'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ab18620f0ed25cfb0d50bb1ab1a5d92caa9c54
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841820"
---
# <a name="ksetupaddrealmflags"></a>ksetup: addrealmflags



Agrega marcas de dominio Kerberos adicionales al dominio Kerberos especificado. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre de dominio Kerberos|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos que no se basa en el sistema operativo Windows Server. Los equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 pueden usar un servidor Kerberos para administrar la autenticación en lugar de usar un dominio que ejecute un sistema operativo Windows Server, y estos sistemas participarán en un dominio Kerberos. Esta entrada establece las características del dominio Kerberos. En la tabla siguiente se describe cada uno de ellos.

|Valor|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|Todos|Se establecen todas las marcas de dominio Kerberos.|
|0x00|Ninguno|No se establecen marcas de dominio Kerberos y no se habilitan características adicionales.|
|0x01|SendAddress|La dirección IP se incluirá en los vales de concesión de vales.|
|0x02|TcpSupported|El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos.|
|0x04|Delegado|Todos los usuarios de este dominio Kerberos son de confianza para la delegación.|
|0x08|NcSupported|Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos.|
|0x80|RC4|Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS.|

Las marcas de dominio Kerberos se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>realm-name</em>. Esta entrada no existe en el registro de forma predeterminada. Puede usar el comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

Puede ver qué marcas de dominio están disponibles y establecerse mediante la visualización de la salida de ksetup o ksetup/dumpstate.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Enumere las marcas de dominio Kerberos disponibles para el dominio Kerberos CONTOSO:
```
Ksetup /listrealmflags
```
Establezca dos marcas en el dominio Kerberos de CONTOSO:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Agregue una marca más que no esté actualmente en el conjunto:
```
ksetup /addrealmflags CONTOSO SendAddress
```
Ejecute el comando **ksetup** para comprobar que la marca de dominio Kerberos está establecida visualizando la salida y buscando **marcas de dominio Kerberos =** .

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)