---
title: 'Escritorio remoto: comparación de las aplicaciones cliente'
description: Obtenga información sobre cómo se comparan las distintas aplicaciones de escritorio remoto en cuanto a funciones y características admitidas.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: 79a6b264c38b4b843c2887c6a3eb6f236480243d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828966"
---
# <a name="compare-the-client-apps"></a>Comparar las aplicaciones cliente

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

A menudo se nos pide cómo comparan las diferentes aplicaciones cliente de escritorio remoto entre sí. ¿Todas ellas hacen lo mismo? Estas son las respuestas a esas preguntas.

## <a name="redirection-support"></a>Compatibilidad con la redirección

Las siguientes tablas comparan la compatibilidad con dispositivos y otros redireccionamientos en la aplicación de conexión a Escritorio remoto, aplicación Universal, aplicación Android, aplicación iOS, macOS web y la aplicación cliente. Estas tablas cubre las redirecciones que puede tener acceso a la vez en una sesión remota. 

Si, de forma remota a su escritorio personal, hay redirecciones adicionales que se pueden configurar en el **configuración adicional** para la sesión. Si su organización administra las aplicaciones o escritorio remoto, el administrador puede habilitar o deshabilitar las redirecciones a través de la configuración de directiva de grupo.

### <a name="input-redirection"></a>Redirección de entrada

| Redirección | Escritorio remoto<br> Conexión | Universal | Android | iOS | macOS | cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Teclado    | X                             | X         | X       | X   | X     | X          |
| Mouse       | X                             | X         | X       | X*    | X     | X          |
| Función táctil       | X                             | X         | X       | X   |       |            |
| Otras       | Lápiz                           |           |         |     |       |            |
* Para ver el [lista de dispositivos de entrada admitidos por el cliente de escritorio remoto de iOS Beta](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirección de puertos   

| Redirección | Escritorio remoto <br>Conexión | Universal | Android | iOS | macOS | cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Puerto serie | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Cuando se habilita la redirección de puertos USB, los dispositivos USB conectados al puerto USB se reconocen automáticamente en la sesión remota.

### <a name="other-redirection-devices-etc"></a>Otro redireccionamiento (dispositivos, etcetera)



| Redirección         | Conexión a Escritorio remoto | Universal   | Android | iOS         | macOS                                    | cliente Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Cámaras             | X                         |             |         |             |                                          |               |
| Portapapeles           | X                         | texto, imagen | texto    | texto, imagen | X                                        | texto          |
| Unidad local y almacenamiento | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| Micrófonos         | X                         |X            |         |             | X                                        |               |
| Impresoras            | X                         |             |         |             | X (solo CUP)                            | Impresión PDF     |
| Escáneres            | X                         |             |         |             |                                          |               |
| Tarjetas inteligentes         | X                         |             |         |             | X (no admitida la autenticación de Windows) |               |
| Altavoces            | X                         | X           | X       | X           | X                                        | X (excepto Internet Explorer) |

* Para la redirección de impresora - la aplicación macOS es compatible con el controlador de impresora publicador claros de forma predeterminada. No admiten la redirección de los controladores de impresora nativo.
