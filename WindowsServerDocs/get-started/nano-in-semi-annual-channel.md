---
title: Cambios en Nano Server en la versión de Windows Server del Canal semianual
description: En el nuevo modelo de servicio de Windows Server, Nano Server es solo un sistema operativo del contenedor con algunos cambios de características.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 0ede3b13c1180b5cf8031738d69b68abd9e4c631
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826138"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Cambios en Nano Server en la versión de Windows Server del Canal semianual

>Se aplica a: Windows Server, Canal semianual

Si ya estás ejecutando Nano Server, el modelo de servicio del [Canal semianual de Windows Server](../get-started-19/servicing-channels-19.md) te resultará familiar, ya que anteriormente era atendido por el modelo Rama actual para empresas (CBB). El Canal semianual de Windows Server no es más que el mismo modelo, pero con distinto nombre. En este modelo, las actualizaciones de características de Nano Server se publicarán de dos a tres veces al año.

Sin embargo, a partir de la versión 1803 de Windows Server, Nano Server solo está disponible como **imagen del sistema operativo base del contenedor**. Debes ejecutarlo como un contenedor en un host de contenedor, como por ejemplo, una instalación básica de Windows Server. La ejecución de un contenedor basado en Nano Server en esta versión difiere de las versiones anteriores en lo siguiente:

- Nano Server se ha optimizado para los programas de .NET Core.
- Nano Server es aún menor que la versión de Windows Server 2016.
- Ya no se incluyen de forma predeterminada PowerShell Core, .NET Core y WMI, pero puedes incluir paquetes del contenedor de [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) y [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) al crear el contenedor.
- En Nano Server ya no está incluida una pila de servicio. Microsoft publica un contenedor Nano actualizado en Docker Hub que debes volver a implementar.
- Mediante Docker puedes solucionar los problemas que tengas con el nuevo contenedor Nano.
- Ahora puedes ejecutar contenedores Nano en IoT Core.

## <a name="related-topics"></a>Temas relacionados

- [Documentación acerca de los contenedores de Windows](https://aka.ms/windowscontainers)
- [Introducción al Canal semianual de Windows Server](../get-started-19/servicing-channels-19.md)
