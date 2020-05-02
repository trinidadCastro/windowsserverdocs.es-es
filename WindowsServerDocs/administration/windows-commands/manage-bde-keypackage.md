---
title: Manage-BDE KeyPackage
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c13502145d80693a64b284bf480fabfd03af0db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724165"
---
# <a name="manage-bde-keypackage"></a>Manage-BDE: KeyPackage



Genera un paquete de claves para una unidad. El paquete de claves se puede usar junto con la herramienta de reparación para reparar unidades dañadas.

## <a name="syntax"></a>Sintaxis

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|-ID|Cree un paquete de claves mediante el protector de clave con el identificador especificado por este valor de identificador.|
|-path|Ubicación en la que se va a guardar el paquete de claves creado.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Ilustra el uso del comando **-KeyPackage** para crear un paquete de claves para la unidad C según el protector de clave identificado por el GUID y guardar el paquete de claves en F:\Folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Use **Manage-BDE – protectors-Get** junto con la letra de unidad para la que desea crear un paquete de claves para obtener una lista de los GUID disponibles que se usarán como valor de identificador.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)