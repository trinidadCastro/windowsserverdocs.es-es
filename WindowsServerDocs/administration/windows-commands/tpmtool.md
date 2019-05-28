---
title: tpmtool
description: Tema de los comandos de Windows para tpmtool - obtiene información sobre el módulo de plataforma segura.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517198"
---
>[!IMPORTANT]
>Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

# <a name="tpmtool"></a>tpmtool

Esta utilidad se puede usar para obtener información sobre la [módulo de plataforma segura (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxis

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|getdeviceinformation|Muestra la información básica del TPM.|
|GatherLogs [ruta de acceso de directorio de salida]|Recopila registros TPM y los coloca en el directorio especificado. De forma predeterminada, se colocan en el directorio actual.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tpmtool_examples"></a>Ejemplos

Para mostrar la información básica del TPM, escriba:
```
tpmtool getdeviceinformation
```
Para recopila registros TPM y colocarlos en el directorio actual, escriba:
```
tpmtool gatherlogs
```
Para recopila registros TPM y colocarlos en `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
