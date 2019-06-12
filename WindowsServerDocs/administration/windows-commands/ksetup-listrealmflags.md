---
title: ksetup:listrealmflags
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7db6caf4e63ea59fa40892679d3de0cfaca661e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438020"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



Muestra las marcas de dominio disponibles que se pueden notificar por **ksetup**. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Parámetros

Ninguno

## <a name="remarks"></a>Comentarios

Los indicadores de territorio especifican características adicionales de un dominio de Kerberos no basadas en Windows. Equipos que ejecutan Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 pueden usar un servidor de Kerberos no basadas en Windows para administrar la autenticación en lugar de usar un dominio que se está ejecutando un sistema operativo Windows Server. Estos sistemas participan en un dominio Kerberos en lugar de un dominio de Windows. Esta entrada establece las características del territorio. En la tabla siguiente describe cada uno.

|Valor|Marca de dominio Kerberos|Descripción|
|-----|----------|-----------|
|0xF|Todos|Se establecen todas las marcas de dominio Kerberos.|
|0x00|Ninguno|Se ha establecido ningún marcador de dominio Kerberos y no características adicionales están habilitadas.|
|0x01|SendAddress|La dirección IP se incluirá dentro de los vales de concesión de vales.|
|0x02|TcpSupported|El protocolo de Control de transmisión (TCP) y el protocolo de datagramas de usuario (UDP) se admiten en este dominio.|
|0x04|Delegado|Todos los miembros de este dominio están de confianza para delegación.|
|0x08|NcSupported|Este campo admite la canonización de nombre, lo que permite los estándares de nomenclatura de dominio Kerberos y DNS.|
|0x80|RC4|Este campo admite el cifrado RC4 para habilitar la confianza entre territorios, que permite el uso de TLS.|

Marcas de dominio Kerberos se almacenan en el registro en **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>nombre de dominio Kerberos</em>. Esta entrada no existe en el registro de forma predeterminada. Puede usar el [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando llenar el registro.

## <a name="BKMK_Examples"></a>Ejemplos

Lista de las marcas del dominio conocidos en este equipo:
```
ksetup /listrealmflags
```
Establecer las marcas del dominio disponible que **Ksetup** no sabe escribiendo cualquiera de los siguientes comandos en la línea de comandos:
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