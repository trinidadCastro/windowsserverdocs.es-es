---
title: Cambios en Nano Server en el canal semianual de Windows Server
description: En el nuevo modelo de mantenimiento de Windows Server, Nano Server es solo un sistema operativo del contenedor con algunos cambios de características.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c9fede02b90e285803a8bcdbc983f264d65a4589
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976513"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Cambios en Nano Server en el canal semianual de Windows Server

>Se aplica a: Windows Server, canal semianual

Si ya está ejecutando Nano Server, el [canal semianual de ventana Server](..\get-started-19\servicing-channels-19.md) modelo de servicio le resultarán familiar, ya que anteriormente se atendió la rama actual para el modelo de empresas (CBB). Canal semianual de Windows Server es simplemente un nombre nuevo para el mismo modelo. En este modelo, las actualizaciones de características de Nano Server se publicarán de dos a tres veces al año.

Sin embargo, a partir de Windows Server, versión 1803, Nano Server solo está disponible como un **imagen de sistema operativo base del contenedor**. Debes ejecutarlo como un contenedor en un host de contenedor, como por ejemplo, una instalación Server Core de Windows Server. Ejecuta un contenedor basado en servidor Nano en esta versión difiere de las versiones anteriores de las siguientes maneras:

- Nano Server se ha optimizado para las aplicaciones de .NET Core.
- Nano Server es incluso más pequeño que la versión de Windows Server 2016.
- Ya no se incluyen de forma predeterminada PowerShell Core, .NET Core y WMI, pero puedes incluir paquetes del contenedor de [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) y [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) al crear el contenedor.
- En Nano Server ya no está incluida una pila de servicio. Microsoft publica un contenedor de Nano actualizado en Docker Hub que debes volver a implementar.
- Con Docker puedes solucionar los problemas que tengas con el nuevo contenedor Nano.
- Ahora puedes ejecutar contenedores Nano en IoT Core.

## <a name="related-topics"></a>Temas relacionados

- [Documentación de contenedores de Windows](http://aka.ms/windowscontainers)
- [Información general del canal semianual de ventana Server](..\get-started-19\servicing-channels-19.md)
