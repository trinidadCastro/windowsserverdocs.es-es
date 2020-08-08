---
title: Descripción de las extensiones del centro de administración de Windows
description: Descripción de las extensiones del SDK del centro de administración de Windows (Project Honolulu)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 44038185bb4f9cb61920033ce5edc67afb3de99b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964581"
---
# <a name="understanding-windows-admin-center-extensions"></a>Descripción de las extensiones del centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En caso de que aún no esté familiarizado con el funcionamiento del centro de administración de Windows, comencemos con la arquitectura de alto nivel. El centro de administración de Windows se compone de dos componentes principales:

- **Servicio Web** ligero que sirve páginas web de interfaz de usuario del centro de administración de Windows para solicitudes de explorador Web.
- **Componente de puerta de enlace** que escucha las solicitudes de API de REST de las páginas web y retransmite llamadas WMI o scripts de PowerShell que se ejecutan en un servidor o clúster de destino.

![Arquitectura del centro de administración de Windows](../media/understand-extensions/wac-architecture-500px.png)

Las páginas web de la interfaz de usuario del centro de administración de Windows atendidas por el servicio web tienen dos componentes principales de la interfaz de usuario desde una perspectiva de extensibilidad, soluciones y herramientas, que se implementan como extensiones, y, un tercer tipo de extensión denominado complementos de puerta de enlace.

## <a name="solution-extensions"></a>Extensiones de soluciones

En la pantalla principal del centro de administración de Windows, de forma predeterminada, puede agregar conexiones que son de uno de los cuatro tipos: conexiones de Windows Server, conexiones de PC de Windows, conexiones de clúster de conmutación por error y conexiones de clúster hiperconvergidas. Una vez agregada una conexión, el nombre y el tipo de la conexión se mostrarán en la pantalla principal. Al hacer clic en el nombre de la conexión se intentará conectar con el servidor o clúster de destino y, a continuación, cargar la interfaz de usuario para la conexión.

![Arquitectura del centro de administración de Windows](../media/understand-extensions/solutions-ui.png)

Cada uno de estos tipos de conexión se asignan a una solución y las soluciones se definen mediante un tipo de extensión denominado extensiones de "solución". Las soluciones suelen definir un tipo único de objeto que desea administrar a través del centro de administración de Windows, como servidores, equipos o clústeres de conmutación por error. También puede definir una nueva solución para conectarse a otros dispositivos, como los conmutadores de red y los servidores Linux, o incluso servicios como Servicios de Escritorio remoto.

## <a name="tool-extensions"></a>Extensiones de herramientas

Al hacer clic en una conexión en la pantalla principal del centro de administración de Windows y conectarse, se cargará la extensión de la solución para el tipo de conexión seleccionado y, a continuación, se mostrará la interfaz de usuario de la solución, incluida una lista de herramientas en el panel de navegación izquierdo. Al hacer clic en una herramienta, la interfaz de usuario de la herramienta se carga y se muestra en el panel derecho.

![Arquitectura de la interfaz de usuario del centro de administración de Windows](../media/understand-extensions/ui-architecture.png)

Cada una de estas herramientas se define a través de un segundo tipo de extensión denominada "Tool" Extensions. Cuando se carga una herramienta, puede ejecutar llamadas WMI o scripts de PowerShell en un servidor o clúster de destino y Mostrar información en la interfaz de usuario o ejecutar comandos en función de los datos proporcionados por el usuario. Una extensión de herramienta define las soluciones en las que se debe mostrar, lo que da lugar a un conjunto diferente de herramientas para cada solución. Si va a crear una nueva extensión de solución, deberá escribir una o varias extensiones de herramientas que proporcionen funcionalidad para la solución.

![Lista de herramientas para cada solución](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>Complementos de puerta de enlace

El servicio de puerta de enlace expone las API de REST para que la interfaz de usuario llame y retransmite comandos y scripts para que se ejecuten en el destino. El servicio de puerta de enlace se puede ampliar mediante complementos de puerta de enlace que admiten diferentes protocolos. El centro de administración de Windows se incluye previamente con dos complementos de puerta de enlace, uno para la ejecución de scripts de PowerShell y otro para los comandos WMI. Si necesita comunicarse con el destino a través de un protocolo distinto de PowerShell o WMI, como REST, puede compilar un complemento de puerta de enlace para este.

## <a name="next-steps"></a>Pasos siguientes

En función de las capacidades que desee compilar en el centro de administración de Windows, puede ser suficiente [crear una extensión de herramienta](develop-tool.md) para una solución de clúster o servidor existente y es el primer paso más sencillo para crear extensiones. Sin embargo, si la característica es para administrar un dispositivo, un servicio o algo totalmente nuevo, en lugar de un servidor o un clúster, debe considerar la posibilidad de [compilar una extensión de solución](develop-solution.md) con una o más herramientas. Y, por último, si necesita comunicarse con el destino a través de un protocolo que no sea WMI o PowerShell, deberá [compilar un complemento de puerta de enlace](develop-gateway-plugin.md). [Siga leyendo](developing-extensions.md) para obtener información sobre cómo configurar el entorno de desarrollo y empezar a escribir la primera extensión.
