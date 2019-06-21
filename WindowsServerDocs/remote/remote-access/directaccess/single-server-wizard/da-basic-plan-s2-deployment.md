---
title: Paso 2 Planear la implementación de DirectAccess básica
description: Este tema forma parte de la Guía de implementar un único servidor de DirectAccess mediante el Asistente de iniciado para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c82d5e48f26d9defceb3b7583e06eeedbc71a082
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281663"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Paso 2 Planear la implementación de DirectAccess básica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura de DirectAccess, el siguiente paso de implementación de DirectAccess en un único servidor con la configuración básica es planear la configuración para el Asistente para introducción.  
  
|Tarea|Descripción|  
|----|--------|  
|Planificar la implementación del cliente|De forma predeterminada, el Asistente para introducción implementa DirectAccess en todos los equipos portátiles y equipos portátiles en el dominio al aplicar un filtro WMI a la configuración GPO de cliente|  
|Planear la implementación de servidor de DirectAccess|Planea cómo implementar el servidor de DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Planear la implementación de cliente  
A la hora de planificar la implementación del cliente, debes tomar dos decisiones:  
  
1.  ¿DirectAccess estará disponible solo para equipos móviles o para cualquier equipo?  
  
    Al configurar los clientes de DirectAccess en el Asistente para introducción, puede elegir permitir que los equipos móviles sólo en los grupos de seguridad especificados se conecten usando DirectAccess. Si restringe el acceso a los equipos móviles, DirectAccess configura automáticamente un filtro WMI para asegurarse de que el cliente de DirectAccess que GPO se aplica solo a los equipos móviles en los grupos de seguridad especificado. El Administrador de DirectAccess necesita permisos para crear o modificar filtros WMI de directiva de grupo para habilitar a esta configuración.  
  
2.  ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    La configuración de DirectAccess se incluye en el GPO de cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifique en el Asistente para introducción. Puedes especificar grupos de seguridad incluidos en cualquier dominio compatible. Antes de configurar DirectAccess, se deben crear los grupos de seguridad. Puede agregar equipos al grupo de seguridad después de completar la implementación de DirectAccess, pero tenga en cuenta que, si agrega los equipos cliente que residen en un dominio diferente al grupo de seguridad, no se aplicará el GPO de cliente para estos clientes. Por ejemplo, si creaste SG1 en el dominio A para clientes de DirectAccess y después agregaste clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B. Para evitar este problema, crea un nuevo grupo de seguridad de cliente para cada dominio que contenga equipos cliente. Si no deseas crear un nuevo grupo de seguridad, puedes ejecutar el cmdlet Add-DAClient con el nombre del nuevo GPO para el nuevo dominio.  
  
## <a name="bkmk_2_2_server"></a>Planear la implementación de servidor de DirectAccess  
Hay una serie de decisiones al planear la implementación del servidor de DirectAccess:  
  
-   **Topología de red** -hay dos topologías disponibles para implementar un servidor de DirectAccess:  
  
    -   **Dos adaptadores** -con dos adaptadores de red, DirectAccess puede configurarse con un adaptador de red conectado directamente a Internet y el otro está conectado a la red interna. Otra opción consiste en instalar el servidor detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red está conectado a la red perimetral y el otro está conectado a la red interna.  
  
    -   **Adaptador de red único** -en esta configuración, el cliente de DirectAccess se instala servidor detrás de un dispositivo perimetral como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  
  
-   **Los adaptadores de red** -el Asistente para DirectAccess detecta automáticamente los adaptadores de red configurados en el servidor de DirectAccess. Asegúrese de que se seleccionen los adaptadores correctos de la **revisión** página.  
  
-   **Certificado IP-HTTPS** -dado que no hay ninguna PKI necesaria en esta implementación, el Asistente aprovisiona automáticamente certificados autofirmados para IP-HTTPS y el servidor de ubicación de red (si no hay certificados están presentes) y habilita automáticamente Proxy de Kerberos. El asistente también permite NAT64 y DNS64 para la traducción de protocolo en el entorno de sólo IPv4. Una vez que el asistente haya terminado de aplicar la configuración correctamente, haga clic en **Cerrar**.  
  
-   **Los clientes de Windows 7** -no se puede habilitar la compatibilidad con clientes de Windows 7 desde el Asistente para introducción. Esto se puede habilitar desde el Asistente para configuración avanzada. Para obtener más información, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuración de VPN** -antes de configurar DirectAccess, decida si va a proporcionar acceso a VPN a los clientes remotos. Debe proporcionar acceso a VPN si tiene equipos cliente de su organización que no admiten la conectividad de DirectAccess (ya sea porque no están administrados o ejecute un sistema operativo para el que no se admite DirectAccess). El Asistente para introducción configura la asignación de direcciones IP de VPN usa DHCP y los clientes VPN se autentiquen mediante Active Directory.  
  
-   **Forzar tunelización** -si tiene pensado usar túnel forzado o quizás quieras incorporarlo en el futuro, debe usar [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para implementar una configuración de dos túneles. Debido a consideraciones de seguridad, no se admite el túnel forzado en una configuración de túnel único.  
  
## <a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 1: Planear la infraestructura básica de DirectAccess](da-basic-plan-s1-infrastructure.md)  
  


