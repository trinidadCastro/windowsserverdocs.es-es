---
title: 'Escritorio remoto: comparación de los redireccionamientos de las aplicaciones cliente'
description: Descubra en qué se diferencian las diversas aplicaciones de Escritorio remoto en lo que respecta a los redireccionamientos.
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 12/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 7d06703386b60ad88271f3239700ef1a97e5c82c
ms.sourcegitcommit: e57536e28902ae52d3040141bbd2aa00e91bbdd3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644605"
---
# <a name="compare-the-clients-redirections"></a>Comparación de los clientes: redireccionamientos

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2

A menudo nos preguntan cómo se comparan los diferentes clientes de Escritorio remoto entre sí. ¿Todas ellas hacen lo mismo? Estas son las respuestas a esas preguntas.

## <a name="redirection-support"></a>Compatibilidad con el redireccionamiento

En las tablas siguientes se compara la compatibilidad con dispositivos y otras redirecciones entre los distintos clientes. Estas tablas cubren los redireccionamientos a las que puedes tener acceso una vez te encuentres en una sesión remota.

Si lo haces de forma remota en tu escritorio personal, hay redireccionamientos adicionales que puedes configurar en la **configuración adicional** de la sesión. Si tu organización administra tus aplicaciones o escritorio remotos, el administrador puede habilitar o deshabilitar los redireccionamientos mediante la configuración de la directiva de grupo o las propiedades del Protocolo de escritorio remoto.

### <a name="input-redirection"></a>Redireccionamiento de entrada

| Redireccionamiento | Bandeja de entrada de Windows</br>(MSTSC) | Escritorio de Windows</br>(MSRDC) | Cliente de Microsoft Store</br>(URDC) | Android | iOS | macOS | Cliente web    |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|---------------|
| Keyboard    | X                         | X                           | X             | X       | X   | X     | X             |
| Mouse       | X                         | X                           | X             | X       | X\* | X     | X             |
| Tocar       | X                         | X                           | X             | X       | X   |       | X (excepto Internet Explorer) |
| Lápiz         | X                         | X                           |               | X (como táctil) |  X (como táctil)  |       |               |

*Consulta la [lista de dispositivos de entrada compatibles para el cliente de iOS de Escritorio remoto](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redireccionamiento de puertos

| Redireccionamiento | Bandeja de entrada de Windows</br>(MSTSC) | Escritorio de Windows</br>(MSRDC) | Cliente de Microsoft Store</br>(URDC) | Android | iOS | macOS | cliente web |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|------------|
| Puerto serie | X                         | X                           |               |         |     |       |            |
| USB         | X                         | X                           |               |         |     |       |            |

Cuando se habilita el redireccionamiento del puerto USB, los dispositivos USB conectados al puerto USB se reconocen automáticamente en la sesión remota.

### <a name="other-redirection-devices-etc"></a>Otro redireccionamiento (dispositivos, etc.)

| Redireccionamiento         | Bandeja de entrada de Windows</br>(MSTSC) | Escritorio de Windows</br>(MSRDC) | Cliente de Microsoft Store</br>(URDC) | Android | iOS         | macOS                           | Cliente web    |
|---------------------|---------------------------|-----------------------------|---------------|---------|-------------|---------------------------------|---------------|
| Cámaras             | X                         | X                           |               |     X    |   X         | X                               |               |
| Portapapeles           | X                         | X                           | X             | Texto    | Texto, imágenes | X                               | texto          |
| Unidad o almacenamiento local | X                         | X                           |               | X       |   X        | X                               |               |
| Ubicación            | X                         | X                           |               |         |             |                                 |               |
| Micrófonos         | X                         | X                           | X             |         |  X          | X                               |               |
| Impresoras            | X                         | X                           |               |         |             | X (solo CUPS)                   | Impresión PDF     |
| Escáneres            | X                         | X                           |               |         |             |                                 |               |
| Smart Cards         | X                         | X                           |               |         |             | X (no es compatible con el inicio de sesión de Windows) |               |
| Altavoces            | X                         | X                           | X             | X       | X           | X                               | X (excepto Internet Explorer) |

*Para el redireccionamiento de la impresora: la aplicación macOS admite el controlador de impresora Publisher Imagesetter de manera predeterminada. No admiten el redireccionamiento de los controladores de impresora nativos.

## <a name="other-resources"></a>Otros recursos

Si busca comparaciones de características, consulte [Comparación de los clientes: características](remote-desktop-features.md).
