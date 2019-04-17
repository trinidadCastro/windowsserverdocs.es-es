---
title: Proceso de implementación de acceso inalámbrico
description: Este tema es parte de la Guía de redes de Windows Server 2016 "Implementar basada en contraseña 802.1X X autenticados Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>Proceso de implementación de acceso inalámbrico

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

El proceso que usan para implementar acceso inalámbrico se produce en estas fases:

## <a name="stage-1--ap-deployment"></a>Escenario 1: implementación de punto de acceso

Planear, implementar y configurar los puntos de acceso para la conectividad inalámbrica de cliente y para su uso con NPS. Según las dependencias de red y preferencias, puedes cualquier pre\-configurar las opciones en los puntos de acceso inalámbricos antes de instalarlos en la red o configurar remotamente tras la instalación.

## <a name="stage-2--ad-ds-group-configuration"></a>Fase 2: configuración de grupo de AD DS

En AD DS, debes crear uno o varios usuarios inalámbricos grupos de seguridad.

A continuación, identificar los usuarios que tienen permiso de acceso inalámbrico a la red.

Por último, agrega los usuarios a los grupos de seguridad de los usuarios inalámbrica adecuados que creaste.

>[!NOTE]
>De manera predeterminada, la **permiso de acceso de red** en Propiedades de marcado de la cuenta de usuario se establece con la opción **controlar el acceso a través de la directiva de red NPS**. A menos que tengas motivos específicos para cambiar esta configuración, se recomienda que mantengas el valor predeterminado. Esto te permite controlar el acceso de red a través de las directivas de red que se configura en NPS.

## <a name="stage-3--group-policy-configuration"></a>Escenario 3: configuración de directiva de grupo

Configurar la red inalámbrica \ (IEEE 802.11\) extensión de directivas de la directiva de grupo mediante el uso de la \(MMC\) el Editor de administración de directivas de grupo de Microsoft Management Console.

Para configurar los equipos miembro domain\ con la configuración de las directivas de red inalámbrica, debes aplicar la directiva de grupo. Cuando un equipo primero está unido al dominio, directiva de grupo se aplica automáticamente. Si se realizan cambios en la directiva de grupo, la nueva configuración se aplica automáticamente:

- Directiva de grupo a intervalos determinado pre\

- Si un usuario de dominio cierra y vuelva a la red

- Reiniciando el equipo cliente y iniciando sesión en el dominio

También puede hacer que la directiva de grupo para actualizar al iniciar sesión un equipo ejecutando el comando **gpupdate** en el símbolo del sistema.

## <a name="stage-4--nps-server-configuration"></a>Escenario 4: configuración del servidor NPS

Usar a un asistente de configuración de NPS para agregar puntos de acceso inalámbrico como clientes RADIUS y para crear las directivas de red que usa NPS al procesar las solicitudes de conexión.

Al usar al Asistente para crear las directivas de red, especificar PEAP como el tipo de EAP y el grupo de seguridad de los usuarios inalámbricos que se creó en la segunda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Paso 5: implementar los clientes inalámbricos

Usar los equipos cliente para conectarse a la red.

Para equipos miembros del dominio que se pueden iniciar sesión en la LAN con cable, las opciones de configuración inalámbrica necesarios se aplican automáticamente cuando se actualiza la directiva de grupo.

Si has habilitado la configuración de red inalámbrica \ (IEEE 802.11\) directivas para conectarse automáticamente cuando el equipo esté dentro del alcance de difusión de la red inalámbrica, los equipos unidos a un domain\ inalámbrica tendrá automáticamente intentan conectarse a la LAN inalámbrica.

Para conectarse a la red inalámbrica, los usuarios solo necesitan proporcionar sus usuario nombre y la contraseña credenciales de dominio cuando se te pida por Windows.

Para planear la implementación de acceso inalámbrico, consulta [planeación de implementación de acceso inalámbrico](d-wireless-access-planning.md).
