---
title: Proceso de implementación de acceso inalámbrico
description: Este tema forma parte de la guía de redes de Windows Server 2016 "implementación de acceso inalámbrico autenticado mediante 802.1 X basado en contraseña".
manager: brianlic
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: eross-msft
ms.author: lizross
ms.openlocfilehash: 9c2326df824288b6adf4453d6ef272ba632eb6c2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969602"
---
# <a name="wireless-access-deployment-process"></a>Proceso de implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El proceso que se usa para implementar el acceso inalámbrico se produce en estas fases:

## <a name="stage-1--ap-deployment"></a>Fase 1: implementación de AP

Planee, implemente y configure los AP para la conectividad de clientes inalámbricos y para su uso con NPS. En función de sus preferencias y de las dependencias de red, puede \- configurar previamente los valores de los puntos de conexión inalámbricos antes de instalarlos en la red, o bien puede configurarlos de forma remota después de la instalación.

## <a name="stage-2--adds-group-configuration"></a>Fase 2: configuración de grupo de AD DS

En AD DS, debe crear uno o más grupos de seguridad de usuarios inalámbricos.

A continuación, identifique a los usuarios a los que se permite el acceso inalámbrico a la red.

Por último, agregue los usuarios a los grupos de seguridad de usuarios inalámbricos adecuados que creó.

>[!NOTE]
>De forma predeterminada, la configuración de **permisos de acceso a la red** en las propiedades de acceso telefónico de la cuenta de usuario está configurada con la opción **Controlar acceso a través de la Directiva de red NPS**. A menos que tenga razones específicas para cambiar este valor, se recomienda que mantenga el valor predeterminado. Esto le permite controlar el acceso a la red a través de las directivas de red que se configuran en NPS.

## <a name="stage-3--group-policy-configuration"></a>Fase 3: configuración de directiva de grupo

Configure la \( \) extensión de directivas IEEE 802,11 de red inalámbrica de directiva de grupo mediante el editor de administración de directivas de grupo MMC de Microsoft Management Console \( \) .

Para configurar \- los equipos miembros del dominio mediante la configuración de las directivas de red inalámbrica, debe aplicar Directiva de grupo. Cuando un equipo se une por primera vez al dominio, directiva de grupo se aplica automáticamente. Si se realizan cambios en directiva de grupo, se aplica automáticamente la nueva configuración:

- Por directiva de grupo a \- intervalos predeterminados

- Si un usuario de dominio cierra la sesión y vuelve a iniciarla en la red

- Reiniciando el equipo cliente e iniciando sesión en el dominio

También puede forzar la actualización de directiva de grupo mientras inicia sesión en un equipo mediante la ejecución del comando **gpupdate** en el símbolo del sistema.

## <a name="stage-4--nps-configuration"></a>Fase 4: configuración de NPS

Use un asistente de configuración de NPS para agregar puntos de acceso inalámbricos como clientes RADIUS y para crear las directivas de red que usa NPS al procesar las solicitudes de conexión.

Cuando use el Asistente para crear las directivas de red, especifique PEAP como el tipo de EAP y el grupo de seguridad usuarios inalámbricos que se creó en la segunda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Fase 5: implementación de clientes inalámbricos

Usar equipos cliente para conectarse a la red.

En el caso de los equipos miembros del dominio que pueden iniciar sesión en la LAN cableada, se aplicarán automáticamente las opciones de configuración inalámbrica necesarias cuando se actualice directiva de grupo.

Si ha habilitado la opción en las directivas de red inalámbrica \( IEEE 802,11 \) para conectarse automáticamente cuando el equipo está dentro del alcance de difusión de la red inalámbrica, los equipos inalámbricos Unidos a un dominio \- intentarán conectarse automáticamente a la LAN inalámbrica.

Para conectarse a la red inalámbrica, los usuarios solo necesitan proporcionar sus credenciales de nombre de usuario y contraseña de dominio cuando Windows las solicite.

Para planear la implementación de acceso inalámbrico, consulte planeamiento de la [implementación de acceso inalámbrico](d-wireless-access-planning.md).
