---
title: Bitsadmin removeclientcertificate
description: Tema de los comandos de Windows para **removeclientcertificate bitsadmin** -quita el certificado de cliente del trabajo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b720800fe80037f38ff01ac3a90d5cbb41a6ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868666"
---
# <a name="bitsadmin-removeclientcertificate"></a>Bitsadmin removeclientcertificate



Quita el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se quita el certificado de cliente desde el trabajo denominado *myJob*.
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)