---
title: pwlauncher
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ddda3ab7831643c3c2c096ae87893d97f90155c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722752"
---
# <a name="pwlauncher"></a>pwlauncher



Habilita o deshabilita las opciones de inicio de Windows to Go (pwlauncher). La herramienta de línea de comandos **pwlauncher** le permite configurar el equipo para que arranque automáticamente en un área de trabajo de Windows to Go (suponiendo que una esté presente), sin necesidad de especificar el firmware o cambiar las opciones de inicio.



## <a name="syntax"></a>Sintaxis

```
Pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/Enable|Habilita las opciones de inicio de Windows to go para que el equipo arranque automáticamente desde un dispositivo USB cuando esté presente|
|/Disable|Deshabilita las opciones de inicio de Windows to go para que el equipo no pueda arrancar desde un dispositivo USB, a menos que se configure manualmente en el firmware.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

El mayor obstáculo para un usuario que desea usar Windows to Go es conseguir que el equipo arranque desde USB. Esto se hace tradicionalmente escribiendo el firmware y probando diferentes opciones de configuración hasta que el equipo esté configurado correctamente. Esto no es una tarea sencilla para la mayoría de los usuarios y es extremadamente arriesgado porque el firmware contiene opciones que pueden representar un sistema inutilizable si no se usan correctamente. Para ayudar a solucionar este problema, los sistemas operativos Windows 8and posteriores incluyen una característica denominada opciones de inicio de Windows to Go que permite a un usuario configurar su equipo para que arranque desde USB desde Windows, sin necesidad de escribir su firmware, siempre y cuando su firmware admita el arranque desde USB. La habilitación de un sistema para que siempre arranque desde USB tiene implicaciones que debe tener en cuenta. Por ejemplo, un dispositivo USB que incluye malware podría arrancar accidentalmente para poner en peligro el sistema, o bien se podría conectar varias unidades USB para producir un conflicto de arranque. Por esta razón, la configuración predeterminada tiene las opciones de inicio de Windows to go deshabilitadas de forma predeterminada. Además, se requieren privilegios de administrador para configurar las opciones de inicio de Windows to go. Si habilita las opciones de inicio de Windows to Go con la herramienta de línea de comandos pwlauncher o la aplicación de **Opciones de inicio de Windows to** Go, el equipo intentará arrancar desde cualquier dispositivo USB que esté insertado en el equipo antes de iniciarse.

## <a name="examples"></a>Ejemplos

Para ver cómo puede usar el comando **pwlauncher** para habilitar el arranque desde USB:
```
Pwlauncher /enable
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)