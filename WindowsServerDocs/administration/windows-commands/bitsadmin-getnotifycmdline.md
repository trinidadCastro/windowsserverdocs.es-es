---
title: bitsadmin getnotifycmdline
description: Tema de comandos de Windows para **bitsadmin getnotifycmdline**, que recupera el comando de línea de comandos que se ejecuta cuando el trabajo finaliza la transferencia de datos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24b49b3fa0c2dafb999d8cb9c6e0c13ae68bf6f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850598"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Recupera el comando de línea de comandos que se debe ejecutar cuando el trabajo finaliza la transferencia de datos.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el comando de línea de comandos que usa el servicio cuando se completa el trabajo denominado *myDownloadJob* .

```
C:\>bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)