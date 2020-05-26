---
title: Manage-BDE WipeFreeSpace
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc252bd0d9d8227badb35bafea96575e37fca243
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820795"
---
# <a name="manage-bde-wipefreespace"></a>Manage-BDE: WipeFreeSpace



Borra el espacio disponible en el volumen quitando los fragmentos de datos que puedan haber existido en el espacio. Ejecutar este comando en un volumen cifrado mediante el método de cifrado solo del espacio usado proporciona el mismo nivel de protección que el método de cifrado de cifrado de volumen completo.

## <a name="syntax"></a>Sintaxis

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado.|
|-Cancelar|Cancela un borrado de espacio disponible que está en curso.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para ilustrar el uso del comando **-w** para crear el borrado del espacio libre en la unidad C.
```
manage-bde -w C:
```
Para ilustrar el uso del comando **-w** con el parámetro **-Cancel** para cancelar el borrado del espacio libre en la unidad C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)