---
title: pwlauncher
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ec9748056b296bb0c74250b36c762fb86fa90ad
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564652"
---
# <a name="pwlauncher"></a>pwlauncher



Habilita o deshabilita el Windows a opciones de inicio Go (pwlauncher). El **pwlauncher** herramienta de línea de comandos le permite configurar el equipo arranque automáticamente en un área de trabajo de Windows To Go (suponiendo que uno está presente), sin necesidad de especificar su firmware o cambiar las opciones de inicio.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/enable|Habilita las opciones de inicio de Windows To Go para que el equipo arrancará automáticamente desde un dispositivo USB cuando está presente|
|/Disable|Deshabilita las opciones de inicio de Windows To Go para que el equipo no se ha arrancado desde un dispositivo USB a menos que configure manualmente en el firmware.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

El mayor obstáculo para un usuario que deseen usar Windows To Go recibe su equipo para arrancar desde USB. Tradicionalmente, esto se hace especificando el firmware y probar distintas opciones de configuración hasta que el equipo está configurado correctamente. Esto no es una tarea simple para la mayoría de los usuarios y es muy arriesgado porque el firmware contiene opciones que pueden inutilizar un sistema si se utiliza incorrectamente. Para ayudar a mitigar este problema, Windows 8and los sistemas operativos posteriores incluyen una característica denominada "Windows para opciones de inicio Go" que permite que un usuario configurar su equipo para que arranque desde una unidad USB de Windows-sin escribir jamás su firmware, siempre y cuando sus firmware admite el arranque desde USB. Habilitación de un sistema de arranque siempre desde una unidad USB, en primer lugar tiene implicaciones que deberían tenerse en cuenta. Por ejemplo, se puede arrancar un dispositivo USB que incluye el software malintencionado sin darse cuenta para poner en peligro el sistema, o varias unidades USB se pueden conectar a producir un conflicto de arranque. Por este motivo, la configuración predeterminada tiene el Windows para ir inicio opciones deshabilitada de forma predeterminada. Además, se requieren privilegios de administrador para configurar Windows para opciones de inicio Go. Si habilita las opciones de inicio de Windows To Go mediante la herramienta de línea de comandos pwlauncher o el **cambiar Windows a opciones de inicio Go** intentará arrancar desde cualquier dispositivo USB que se inserta en el equipo antes de que el equipo de aplicación se inició.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente muestra cómo puede usar el **pwlauncher** comando para habilitar el arranque desde USB:
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)