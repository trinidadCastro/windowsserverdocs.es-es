---
title: 'ksetup: dominio'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfaa8a37ae4ee5c9669b09f27a73b3d016324dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841588"
---
# <a name="ksetupdomain"></a>ksetup: dominio



Establece el nombre de dominio para todas las operaciones de Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName >|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Ninguno.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Establezca una conexión a un dominio válido, como Microsoft, mediante el subcomando/mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Cuando la conexión se realiza correctamente, recibirá un TGT nuevo o se actualizará un TGT existente.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)