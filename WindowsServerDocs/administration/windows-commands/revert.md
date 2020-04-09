---
title: revertir
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7dae609e4615868b03e4b5ea9a681f553aa0667
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835758"
---
# <a name="revert"></a>revertir



Revierte un volumen a una instantánea especificada. Esto solo se admite para las instantáneas en el contexto CLIENTACCESSIBLE. Estas instantáneas son persistentes y solo las puede realizar el proveedor del sistema. Si se usa sin parámetros, **Revert** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowCopyID >|Especifica el identificador de la instantánea a la que se revierte el volumen.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)