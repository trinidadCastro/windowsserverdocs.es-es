---
title: pwlauncher
description: Artículo de referencia para el comando pwlauncher, que habilita o deshabilita las opciones de inicio de Windows to Go (pwlauncher).
ms.topic: reference
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e59182378aa8277a0ff68b31be29c9c315b174b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028153"
---
# <a name="pwlauncher"></a>pwlauncher

Habilita o deshabilita las opciones de inicio de Windows to Go (pwlauncher). La herramienta de línea de comandos **pwlauncher** le permite configurar el equipo para que arranque automáticamente en un área de trabajo de Windows to Go (suponiendo que una esté presente), sin necesidad de especificar el firmware o cambiar las opciones de inicio.

Las opciones de inicio de Windows to go permiten que un usuario configure su equipo para que arranque desde USB desde dentro de Windows, sin necesidad de escribir su firmware, siempre y cuando su firmware admita el arranque desde USB. La habilitación de un sistema para que siempre arranque desde USB tiene implicaciones que debe tener en cuenta. Por ejemplo, un dispositivo USB que incluye malware podría arrancar accidentalmente para poner en peligro el sistema, o bien se podría conectar varias unidades USB para producir un conflicto de arranque. Por esta razón, la configuración predeterminada tiene las opciones de inicio de Windows to go deshabilitadas de forma predeterminada. Además, se requieren privilegios de administrador para configurar las opciones de inicio de Windows to go. Si habilita las opciones de inicio de Windows to Go con la herramienta de línea de comandos pwlauncher o la aplicación de **Opciones de inicio de Windows to** Go, el equipo intentará arrancar desde cualquier dispositivo USB que esté insertado en el equipo antes de iniciarse.

## <a name="syntax"></a>Sintaxis

```
pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /Enable | Habilita las opciones de inicio de Windows to Go, por lo que el equipo arrancará automáticamente desde un dispositivo USB cuando esté presente. |
| /Disable | Deshabilita las opciones de inicio de Windows to Go, por lo que el equipo no se puede arrancar desde un dispositivo USB a menos que se configure manualmente en el firmware. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para habilitar el arranque desde USB:

```
pwlauncher /enable
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
