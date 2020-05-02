---
title: 'ksetup: dominio'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f127eaf33e9ef6d597851c31a4167ceaa3516abb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724682"
---
# <a name="ksetupdomain"></a>ksetup: dominio



Establece el nombre de dominio para todas las operaciones de Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como contoso.com o contoso.|

## <a name="remarks"></a>Observaciones

Ninguno.

## <a name="examples"></a>Ejemplos

Establezca una conexión a un dominio válido, como Microsoft, mediante el subcomando/mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Cuando la conexión se realiza correctamente, recibirá un TGT nuevo o se actualizará un TGT existente.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)