---
title: pwlauncher
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4cf65643e0c5a4b28e06619a8156792b6e5456ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371966"
---
# <a name="pwlauncher"></a>pwlauncher



Habilita o deshabilita las opciones de inicio de Windows to Go (pwlauncher). La herramienta de línea de comandos **pwlauncher** le permite configurar el equipo para que arranque automáticamente en un área de trabajo de Windows to Go (suponiendo que una esté presente), sin necesidad de especificar el firmware o cambiar las opciones de inicio.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/Enable|Habilita las opciones de inicio de Windows to go para que el equipo arranque automáticamente desde un dispositivo USB cuando esté presente|
|/Disable|Deshabilita las opciones de inicio de Windows to go para que el equipo no pueda arrancar desde un dispositivo USB, a menos que se configure manualmente en el firmware.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

El mayor obstáculo para un usuario que desea usar Windows to Go es conseguir que el equipo arranque desde USB. Esto se hace tradicionalmente escribiendo el firmware y probando diferentes opciones de configuración hasta que el equipo esté configurado correctamente. Esto no es una tarea sencilla para la mayoría de los usuarios y es extremadamente arriesgado porque el firmware contiene opciones que pueden representar un sistema inutilizable si no se usan correctamente. Para ayudar a solucionar este problema, los sistemas operativos Windows 8and posteriores incluyen una característica denominada "opciones de inicio de Windows to Go" que permite a un usuario configurar su equipo para que arranque desde USB desde Windows, sin tener que escribir su firmware en ningún momento, siempre y cuando firmware admite el arranque desde USB. La habilitación de un sistema para que siempre arranque desde USB tiene implicaciones que debe tener en cuenta. Por ejemplo, un dispositivo USB que incluye malware podría arrancar accidentalmente para poner en peligro el sistema, o bien se podría conectar varias unidades USB para producir un conflicto de arranque. Por esta razón, la configuración predeterminada tiene las opciones de inicio de Windows to go deshabilitadas de forma predeterminada. Además, se requieren privilegios de administrador para configurar las opciones de inicio de Windows to go. Si habilita las opciones de inicio de Windows to Go con la herramienta de línea de comandos pwlauncher o la aplicación de **Opciones de inicio de Windows to** Go, el equipo intentará arrancar desde cualquier dispositivo USB que esté insertado en el equipo antes de iniciarse.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se muestra cómo se puede usar el comando **pwlauncher** para habilitar el arranque desde USB:
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)