---
title: 'ksetup: delrealmflags'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ba11f6c06f9479be584d847d77adf0a3142b94a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724677"
---
# <a name="ksetupdelrealmflags"></a>ksetup: delrealmflags



Quita las marcas de dominio Kerberos del dominio Kerberos especificado. 

## <a name="syntax"></a>Sintaxis

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> RealmName|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **Ksetup** .|

## <a name="remarks"></a>Observaciones

Las marcas de dominio Kerberos especifican características adicionales de un dominio Kerberos que no se basa en el sistema operativo Windows Server. Los equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 pueden usar un servidor Kerberos para administrar la autenticación en lugar de usar un dominio que ejecute un sistema operativo Windows Server, y estos sistemas participarán en un dominio Kerberos. Esta entrada establece las características del dominio Kerberos. En la tabla siguiente se describe cada uno de ellos.

|Value|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|All|Se establecen todas las marcas de dominio Kerberos.|
|0x00|None|No se establecen marcas de dominio Kerberos y no se habilitan características adicionales.|
|0x01|SendAddress|La dirección IP se incluirá en los vales de concesión de vales.|
|0x02|TcpSupported|El protocolo de control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio Kerberos.|
|0x04|Delegar|Todos los usuarios de este dominio Kerberos son de confianza para la delegación.|
|0x08|NcSupported|Este dominio Kerberos admite la canonización de nombres, que permite los estándares de nomenclatura DNS y dominio Kerberos.|
|0x80|RC4|Este dominio Kerberos es compatible con el cifrado RC4 para habilitar la confianza entre dominios, lo que permite el uso de TLS.|

Las marcas de dominio Kerberos se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\**<em>RealmName</em>. Esta entrada no existe en el registro de forma predeterminada. Puede usar el comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

Puede ver qué marcas de dominio están disponibles y establecerse mediante la visualización de la salida de **ksetup** o **ksetup/dumpstate**.

## <a name="examples"></a>Ejemplos

Enumere las marcas de dominio Kerberos disponibles para el dominio Kerberos CONTOSO:
```
Ksetup /listrealmflags
```
Quitar dos marcas que se encuentran actualmente en el conjunto:
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Ejecute el comando **ksetup** para comprobar que la marca de dominio Kerberos está establecida visualizando la salida y buscando **marcas de dominio Kerberos =**.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)