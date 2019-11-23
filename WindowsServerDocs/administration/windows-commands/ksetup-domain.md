---
title: 'ksetup: dominio'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375119"
---
# <a name="ksetupdomain"></a>ksetup: dominio



Establece el nombre de dominio para todas las operaciones de Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName >|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como contoso.com o contoso.|

## <a name="remarks"></a>Observaciones

Ninguno.

## <a name="BKMK_Examples"></a>Example

Establezca una conexión a un dominio válido, como Microsoft, mediante el subcomando/mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Cuando la conexión se realiza correctamente, recibirá un TGT nuevo o se actualizará un TGT existente.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)