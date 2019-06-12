---
title: ksetup:delrealmflags
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd2a3897a07a2eda4c05526b0ae8c55dda35e1e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437993"
---
# <a name="ksetupdelrealmflags"></a>ksetup:delrealmflags



Quita las marcas del dominio del dominio Kerberos especificado.  Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y se muestra como el dominio Kerberos predeterminado cuando **Ksetup** se ejecuta.|

## <a name="remarks"></a>Comentarios

Los indicadores de territorio especifican características adicionales de un dominio Kerberos que no se basa en el sistema operativo Windows Server. Equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2, pueden usar un servidor de Kerberos para administrar la autenticación en lugar de usar un dominio que se está ejecutando un sistema operativo Windows Server, y estos sistemas participan en una Dominio Kerberos. Esta entrada establece las características del territorio. En la tabla siguiente describe cada uno.

|Valor|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|Todos|Se establecen todas las marcas de dominio Kerberos.|
|0x00|Ninguno|Se ha establecido ningún marcador de dominio Kerberos y no características adicionales están habilitadas.|
|0x01|SendAddress|La dirección IP se incluirá en el vale de concesión-vales.|
|0x02|TcpSupported|El protocolo de Control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio.|
|0x04|Delegado|Todos los miembros de este dominio están de confianza para delegación.|
|0x08|NcSupported|Este campo admite la canonización de nombre, lo que permite los estándares de nomenclatura de dominio Kerberos y DNS.|
|0x80|RC4|Este campo admite el cifrado RC4 para habilitar la confianza entre territorios, que permite el uso de TLS.|

Marcas de dominio Kerberos se almacenan en el registro en **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>RealmName</em>. Esta entrada no existe en el registro de forma predeterminada. Puede usar el [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando llenar el registro.

Puede ver qué marcas de dominio Kerberos están disponible y configurado mediante la visualización de la salida de **ksetup** o **/dumpstate ksetup**.

## <a name="BKMK_Examples"></a>Ejemplos

Lista de las marcas de dominio disponible para el dominio CONTOSO:
```
Ksetup /listrealmflags
```
Quitar dos indicadores que están actualmente en el conjunto:
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Ejecute el **ksetup** comando para comprobar que la marca del dominio está establecida al ver la salida y busca **marcas Realm =** .

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)