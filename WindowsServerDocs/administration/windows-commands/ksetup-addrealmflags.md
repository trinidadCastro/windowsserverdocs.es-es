---
title: ksetup:addrealmflags
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbc878bd0ee25ad92c640710ab6b46bbc0eaf62a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827686"
---
# <a name="ksetupaddrealmflags"></a>ksetup:addrealmflags



Agrega marcas de dominio adicionales para el dominio Kerberos especificado. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre de dominio Kerberos|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Los indicadores de territorio especifican características adicionales de un dominio Kerberos que no se basa en el sistema operativo Windows Server. Equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2, pueden usar un servidor de Kerberos para administrar la autenticación en lugar de usar un dominio que se está ejecutando un sistema operativo Windows Server, y estos sistemas participan en una Dominio Kerberos. Esta entrada establece las características del territorio. En la tabla siguiente describe cada uno.

|Valor|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|Todos|Se establecen todas las marcas de dominio Kerberos.|
|0x00|Ninguno|Se ha establecido ningún marcador de dominio Kerberos y no características adicionales están habilitadas.|
|0x01|SendAddress|La dirección IP se incluirá dentro de los vales de concesión de vales.|
|0x02|TcpSupported|El protocolo de Control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio.|
|0x04|Delegado|Todos los miembros de este dominio están de confianza para delegación.|
|0x08|NcSupported|Este campo admite la canonización de nombre, lo que permite los estándares de nomenclatura de dominio Kerberos y DNS.|
|0x80|RC4|Este campo admite el cifrado RC4 para habilitar la confianza entre territorios, que permite el uso de TLS.|

Marcas de dominio Kerberos se almacenan en el registro bajo **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** Realm-nombre*. Esta entrada no existe en el registro de forma predeterminada. Puede usar el [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando llenar el registro.

Puede ver qué marcas de dominio Kerberos están disponible y configurado mediante la visualización de la salida de ksetup o ksetup /dumpstate.

## <a name="BKMK_Examples"></a>Ejemplos

Lista de las marcas de dominio disponible para el dominio CONTOSO:
```
Ksetup /listrealmflags
```
Establecer dos marcas para el dominio CONTOSO:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Agregar una marca más que no está actualmente en el conjunto:
```
ksetup /addrealmflags CONTOSO SendAddress
```
Ejecute el **ksetup** comando para comprobar que la marca del dominio está establecida al ver la salida y busca **marcas Realm =**.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)