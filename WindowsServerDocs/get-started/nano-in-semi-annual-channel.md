---
title: Cambios en Nano Server en el canal semianual de Windows Server
description: En el nuevo modelo de mantenimiento de Windows Server, Nano Server es solo un sistema operativo del contenedor con algunos cambios de características.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 7e68d292c32ce58c786a3242203330fcae985913
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847776"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Cambios en Nano Server en el canal semianual de Windows Server

>Se aplica a: Windows Server, canal semianual


Tal como se describe en [Introducción a canal semianual de Windows Server](semi-annual-channel-overview.md), Windows Server, versión 1803 es la versión más reciente en el canal semianual.

Si ya estás ejecutando Nano Server, este modelo de servicio resultará familiar, ya que anteriormente era atendido por el modelo Rama actual para empresas (CBB). El nuevo Canal semianual de Windows Server es solo un nombre nuevo para el mismo modelo. En este modelo, las actualizaciones de características de Nano Server se publicarán de dos a tres veces al año.

Sin embargo, con esta versión de Windows Server, versión 1803, Nano Server solo está disponible como una **imagen base de sistema operativo de contenedor**. Debes ejecutarlo como un contenedor en un host de contenedor, como por ejemplo, una instalación Server Core de Windows Server. Ejecuta un contenedor basado en servidor Nano en esta versión difiere de las versiones anteriores de las siguientes maneras:

- Nano Server se ha optimizado para las aplicaciones de .NET Core.
- Nano Server es incluso más pequeño que la versión de Windows Server 2016.
- Ya no se incluyen de forma predeterminada PowerShell Core, .NET Core y WMI, pero puedes incluir paquetes del contenedor de [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) y [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) al crear el contenedor.
- En Nano Server ya no está incluida una pila de servicio. Microsoft publica un contenedor de Nano actualizado en Docker Hub que debes volver a implementar.
- Con Docker puedes solucionar los problemas que tengas con el nuevo contenedor Nano.
- Ahora puedes ejecutar contenedores Nano en IoT Core.

## <a name="related-topics"></a>Temas relacionados
Cuando se inicie el Programa Insider, encontrarás más información en la [Documentación de contenedores de Windows](http://aka.ms/windowscontainers).
