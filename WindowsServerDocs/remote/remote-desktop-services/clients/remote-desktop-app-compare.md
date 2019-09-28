---
title: 'Escritorio remoto: comparación de las aplicaciones cliente'
description: Aprenda cómo se comparan las diferentes aplicaciones de Escritorio remoto cuando se trata de las características y funciones admitidas.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 237bb79fae6460bc3b31fb1753e2d679c8d67512
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404163"
---
# <a name="compare-the-client-apps"></a>Comparación de las aplicaciones cliente

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

A menudo nos preguntan cómo se comparan las diferentes aplicaciones cliente de Escritorio remoto entre sí. ¿Todas ellas hacen lo mismo? Estas son las respuestas a esas preguntas.

## <a name="redirection-support"></a>Compatibilidad con el redireccionamiento

En las tablas siguientes se compara la compatibilidad para dispositivos y otros redireccionamientos en la aplicación Conexión a Escritorio remoto, aplicación universal, aplicación Android, aplicación iOS, aplicación macOS y cliente web. Estas tablas cubren los redireccionamientos a las que puedes tener acceso una vez te encuentres en una sesión remota. 

Si lo haces de forma remota en tu escritorio personal, hay redireccionamientos adicionales que puedes configurar en la **configuración adicional** de la sesión. Si tu organización administra tus aplicaciones o escritorio remotos, tu administrador puede habilitar o deshabilitar los redireccionamientos mediante la configuración de la directiva de grupo.

### <a name="input-redirection"></a>Redireccionamiento de entrada

| Redireccionamiento | Escritorio remoto<br> Conexión | Universal | Android | iOS | macOS |          cliente web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Teclado   |               X               |     X     |    X    |  X  |   X   |               X               |
|    Mouse    |               X               |     X     |    X    | X\* |   X   |               X               |
|    Función táctil    |               X               |     X     |    X    |  X  |       | X (no se admiten Internet Explorer ni Edge) |
|    Otras    |              Lápiz              |           |         |     |       |                               |

*Consulta la [lista de dispositivos de entrada compatibles para el cliente beta de iOS de Escritorio remoto](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redireccionamiento de puertos   

| Redireccionamiento | Escritorio remoto <br>Conexión | Universal | Android | iOS | macOS | cliente web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Puerto serie | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Cuando se habilita el redireccionamiento del puerto USB, los dispositivos USB conectados al puerto USB se reconocen automáticamente en la sesión remota.

### <a name="other-redirection-devices-etc"></a>Otro redireccionamiento (dispositivos, etc.)



| Redireccionamiento         | Conexión a Escritorio remoto | Universal   | Android | iOS         | macOS                                    | cliente web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Cámaras             | X                         |             |         |             |                                          |               |
| Portapapeles           | X                         | texto, imagen | texto    | texto, imagen | X                                        | texto          |
| Unidad o almacenamiento local | X                         |             | X       |             | x                                        |               |
| Ubicación            | X                         |             |         |             |                                          |               |
| Micrófonos         | X                         |X            |         |             | X                                        |               |
| Impresoras            | X                         |             |         |             | X (solo CUPS)                            | Impresión PDF     |
| Escáneres            | X                         |             |         |             |                                          |               |
| Tarjetas inteligentes         | X                         |             |         |             | X (no se admite la autenticación de Windows) |               |
| Altavoces            | X                         | X           | X       | X           | X                                        | X (excepto Internet Explorer) |

*Para el redireccionamiento de la impresora: la aplicación macOS admite el controlador de impresora Publisher Imagesetter de manera predeterminada. No admiten el redireccionamiento de los controladores de impresora nativos.
