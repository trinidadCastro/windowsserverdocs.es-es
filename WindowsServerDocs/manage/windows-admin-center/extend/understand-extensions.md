---
title: Descripción de las extensiones de Windows Admin Center
description: Descripción de las extensiones de SDK de Windows Admin Center (Proyecto Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 4bfabe4959fe16f5e240cbf1a972a902e37ffb52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385247"
---
# <a name="understanding-windows-admin-center-extensions"></a>Descripción de las extensiones de Windows Admin Center

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En caso de que aún no esté familiarizado con el funcionamiento del centro de administración de Windows, comencemos con la arquitectura de alto nivel. Windows Admin Center se compone de dos componentes principales:

- **Servicio web** ligero que sirve las páginas web de la interfaz de usuario de Windows Admin Center para solicitudes de navegadores web.
- **Componente de puerta de enlace** que atiende las solicitudes de API de REST desde las páginas web y retransmite las llamadas WMI o scripts de PowerShell para que se ejecuten en un clúster o servidor de destino.

![Arquitectura de Windows Admin Center](../media/understand-extensions/wac-architecture-500px.png)

Las páginas web de la interfaz de usuario de Windows Admin Center servidas por el servicio web tienen dos componentes principales de la interfaz de usuario desde una perspectiva de extensibilidad, soluciones y herramientas, que se implementan como extensiones, y un tercer tipo de extensión denominado complementos de puerta de enlace.

## <a name="solution-extensions"></a>Extensiones de soluciones

En la pantalla principal de Windows Admin Center, de forma predeterminada, puedes agregar conexiones de cuatro tipos: Windows Server, conexiones de PC Windows, conexiones de clúster de conmutación por error y conexiones de clúster hiperconvergido. Una vez que se agrega una conexión, el nombre y el tipo de la conexión se mostrarán en la pantalla principal. Al hacer clic en el nombre de la conexión se intenta conectar con el clúster o el servidor de destino y, a continuación, cargar la interfaz de usuario para la conexión.

![Arquitectura de Windows Admin Center](../media/understand-extensions/solutions-ui.png)

Cada uno de estos tipos de conexión se asignan a una solución y las soluciones se definen a través de un tipo de extensión llamado extensiones de "solución". Normalmente, las soluciones definen un único tipo de objeto que deseas administrar a través de Windows Admin Center, como servidores, PC o clústeres de conmutación por error. También podrías definir una nueva solución a la que conectarse y administrar otros dispositivos como conmutadores de red y servidores Linux o incluso servicios como Servicios de Escritorio remoto.

## <a name="tool-extensions"></a>Extensiones de herramientas

Al hacer clic en una conexión en la pantalla principal de Windows Admin Center y conectar, se cargará la extensión de la solución para el tipo de conexión seleccionado y, a continuación, se presenta la interfaz de usuario de la solución con una lista de herramientas en el panel de navegación izquierdo. Al hacer clic en una herramienta, la interfaz de usuario de la herramienta se carga y se muestra en el panel derecho.

![Arquitectura de la interfaz de usuario de Windows Admin Center](../media/understand-extensions/ui-architecture.png)

Cada una de estas herramientas se definen a través de un segundo tipo de extensión denominada extensiones de "herramienta". Cuando se carga una herramienta, puede ejecutar llamadas WMI o scripts de PowerShell en un clúster o un servidor de destino y mostrar información en la interfaz de usuario o ejecutar comandos en función de la entrada del usuario. Una extensión de herramienta define para qué soluciones se debe mostrar, resultando en un conjunto diferente de herramientas para cada solución. Si estás creando una nueva extensión de la solución, tendrás que escribir adicionalmente una o varias extensiones de herramienta que proporcionen funcionalidad para la solución.

![Lista de herramientas para cada solución](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>Complementos de puerta de enlace

El servicio de puerta de enlace expone API de REST para la interfaz de usuario para llamar y transmite comandos y scripts que se van a ejecutar en el destino. El servicio de puerta de enlace se puede ampliar mediante complementos de puerta de enlace que admiten protocolos diferentes. Windows Admin Center viene preconfigurado con dos complementos de puerta de enlace, uno para ejecutar scripts de PowerShell y otro para los comandos WMI. Si necesitas comunicarse con el destino a través de un protocolo distinto a PowerShell o WMI, como REST, puedes crear un complemento de puerta de enlace para esto.

## <a name="next-steps"></a>Pasos siguientes

Según las funcionalidades que desees crear en Windows Admin Center, [crear una extensión de la herramienta](develop-tool.md) para un clúster o servidor existente puede ser suficiente y es el primer paso más sencillo para crear las extensiones. Sin embargo, si la característica es para administrar un dispositivo, servicio o algo totalmente nuevo, en lugar de un servidor o clúster, deberías considerar [crear una extensión de la solución](develop-solution.md) con una o más herramientas. Y, por último, si necesita comunicarse con el destino a través de un protocolo que no sea WMI o PowerShell, deberá [compilar un complemento de puerta de enlace](develop-gateway-plugin.md). [Continúa leyendo](developing-extensions.md) para aprender a configurar el entorno de desarrollo y comenzar a escribir la primera extensión.
