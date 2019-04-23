---
title: waitfor
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4a91dd3822cf4d8dd904f473f146a2f0ee54c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840166"
---
# <a name="waitfor"></a>waitfor



Envía o espera una señal en un sistema. **WAITFOR** se utiliza para sincronizar los equipos a través de una red.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<equipo >|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificadas en el comando.|
|/u [\<Domain>\]<User>|Ejecuta el script con las credenciales de cuenta de usuario especificada. De forma predeterminada, **waitfor** usa las credenciales del usuario actual.|
|/p [\<Password>]|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/si|Envía la señal especificada a través de la red.|
|/t \<tiempo de espera >|Especifica el número de segundos que esperar una señal. De forma predeterminada, **waitfor** espera indefinidamente.|
|\<SignalName>|Especifica la señal que **waitfor** espera o envía. *NombreDeSeñal* no distingue mayúsculas de minúsculas.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Los nombres de señal no pueden superar los 225 caracteres. Caracteres válidos son 0-9, a-z, A-z y el ASCII extendido el juego de caracteres (128-255).
-   Si no usas **/s**, la señal se difunde a todos los sistemas en un dominio. Si usas **/s**, se envía una señal solo con el sistema especificado.
-   Puede ejecutar varias instancias de **waitfor** en un único equipo, pero cada instancia de **waitfor** debe esperar una señal diferente. Solo una instancia de **waitfor** puede esperar una señal determinada en un equipo determinado.
-   Puede activar una señal manualmente mediante el **/si** opción de línea de comandos.
-   **WAITFOR** se ejecuta solo en XP de Windows y los servidores que ejecutan un sistema operativo Windows Server 2003, pero pueden enviar señales a cualquier equipo que ejecute un sistema operativo de Windows.
-   Los equipos pueden recibir señales sólo si están en el mismo dominio que el equipo que envía la señal.
-   Puede usar **waitfor** al probar las compilaciones de software. Por ejemplo, el equipo que compila puede enviar una señal a varios equipos que ejecutan **waitfor** después de la compilación se ha completado correctamente. Al recibir la señal, el archivo por lotes que incluye **waitfor** puede indicar a los equipos para iniciar inmediatamente la instalación de software o ejecutar pruebas en la compilación.

## <a name="BKMK_examples"></a>Ejemplos

Para esperar hasta que se recibe la señal "espresso\build007", escriba:
```
waitfor espresso\build007
```
De forma predeterminada, **waitfor** espera indefinidamente una señal.

Para esperar 10 segundos para la señal "espresso\compile007" poder recibir antes de agotar el tiempo, escriba:
```
waitfor /t 10 espresso\build007
```
Para activar manualmente la señal "espresso\build007", escriba:
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)