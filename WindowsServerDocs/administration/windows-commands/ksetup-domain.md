---
title: ksetup:domain
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857456"
---
# <a name="ksetupdomain"></a>ksetup:domain



Establece el nombre de dominio para todas las operaciones de Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio al que va a establecer una conexión. Use el nombre de dominio completo o un formulario simple del nombre, como contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Ninguno.

## <a name="BKMK_Examples"></a>Ejemplos

Establecer una conexión a un dominio válido, como Microsoft mediante el subcomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Cuando la conexión se realiza correctamente, recibirá un nuevo TGT o un TGT existente se actualizarán.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)