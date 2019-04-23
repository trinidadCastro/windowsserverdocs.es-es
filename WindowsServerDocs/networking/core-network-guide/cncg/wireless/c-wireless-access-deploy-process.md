---
title: Proceso de implementación de acceso inalámbrico
description: Este tema forma parte de la Guía de redes de Windows Server 2016 "Implementar mediante 802.1X basado en contraseña X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879846"
---
# <a name="wireless-access-deployment-process"></a>Proceso de implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Se produce el proceso que se utiliza para implementar el acceso inalámbrico en estas fases:

## <a name="stage-1--ap-deployment"></a>Fase 1: implementación de Asia Pacífico

Planear, implementar y configurar los puntos de acceso para la conectividad de cliente inalámbrica y para su uso con NPS. Dependiendo de las dependencias de red y preferencias, podrá previamente\-establecer la configuración en tus AP inalámbricos antes de instalarlos en la red o puede configurarlos de forma remota después de la instalación.

## <a name="stage-2--adds-group-configuration"></a>Fase 2: configuración del grupo de AD DS

En AD DS, debe crear grupos de seguridad de uno o varios usuarios inalámbricos.

A continuación, identifique los usuarios que tienen permiso de acceso inalámbrico a la red.

Por último, agregue los usuarios a los grupos de seguridad usuarios inalámbricos adecuado que ha creado.

>[!NOTE]
>De forma predeterminada, el **permiso de acceso de red** en Propiedades de marcado de la cuenta de usuario se establece con la configuración de **controlar el acceso a través de la directiva de red NPS**. A menos que tenga razones específicas para cambiar esta configuración, se recomienda que mantenga el valor predeterminado. Esto le permite controlar el acceso de red a través de las directivas de red que se configuran en NPS.

## <a name="stage-3--group-policy-configuration"></a>Fase 3: configuración de directiva de grupo

Configurar la red inalámbrica \(IEEE 802.11\) extensión de directivas de directiva de grupo mediante el uso del Editor de administración de directivas de grupo de Microsoft Management Console \(MMC\).

Configurar dominio\-los equipos de miembros mediante la configuración de directivas de redes inalámbricas, debe aplicar la directiva de grupo. Cuando un equipo en primer lugar está unido al dominio, automáticamente se aplica la directiva de grupo. Si se realizan cambios en la directiva de grupo, se aplica automáticamente la nueva configuración:

- Directiva de grupo en pre\-determina los intervalos

- Si cierra la sesión de un usuario de dominio y, a continuación, hacer una copia de sesión en la red

- Al reiniciar el equipo cliente e iniciando sesión en el dominio

También puede forzar la directiva de grupo para actualizar mientras esté conectado a un equipo, ejecute el comando **gpupdate** en el símbolo del sistema.

## <a name="stage-4--nps-configuration"></a>Fase 4: configuración de NPS

Usar a un Asistente para configuración de NPS para agregar puntos de acceso inalámbricos como clientes RADIUS y para crear las directivas de red que usa NPS al procesar las solicitudes de conexión.

Cuando se usa al Asistente para crear las directivas de red, especifique PEAP como el tipo de EAP y el grupo de seguridad de los usuarios inalámbricos que se creó en la segunda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Fase 5: implementar a los clientes inalámbricos

Use los equipos cliente para conectarse a la red.

Para equipos miembros del dominio que se pueden iniciar sesión en la red LAN cableada, los valores de configuración inalámbrica necesarios se aplican automáticamente cuando se actualiza la directiva de grupo.

Si ha habilitado la configuración de red inalámbrica \(IEEE 802.11\) directivas para conectarse automáticamente cuando el equipo está dentro de intervalo de la red inalámbrica, la inalámbrica, el dominio de difusión\-luego actuará de equipos combinados intenta automáticamente conectarse a la LAN inalámbrica.

Para conectarse a la red inalámbrica, los usuarios sólo necesitan proporcionar sus credenciales usuario de dominio nombre y la contraseña cuando se le pida en Windows.

Para planear la implementación de acceso inalámbrico, consulte [planeamiento de implementación de acceso inalámbrico](d-wireless-access-planning.md).
