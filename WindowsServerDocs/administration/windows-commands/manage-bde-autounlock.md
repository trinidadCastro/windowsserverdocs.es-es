---
title: Manage-BDE AUTOLOCK
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1cc467c4afcfa2df344e9190a341a9aad086c1ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724215"
---
# <a name="manage-bde-autounlock"></a>Manage-BDE: desbloqueo automático



Administra el desbloqueo automático de las unidades de datos protegidas por BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-enable|Habilita el desbloqueo automático para una unidad de datos.|
|-disable|Deshabilita el desbloqueo automático para una unidad de datos.|
|-clearallkeys|Quita todas las claves externas almacenadas en la unidad del sistema operativo.|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para ilustrar el uso del comando **-AUTOLOCK** para habilitar el desbloqueo automático de la unidad de datos E.
```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)