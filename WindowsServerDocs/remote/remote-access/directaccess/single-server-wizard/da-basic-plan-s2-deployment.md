---
title: Paso 2 planear la implementación básica de DirectAccess
description: Este tema forma parte de la guía de implementación de un único servidor de DirectAccess con el Asistente para Introducción para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3009b6002d9d4cd116795c46305ff02fda02ef63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388495"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Paso 2 planear la implementación básica de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura de DirectAccess, el siguiente paso en la implementación de DirectAccess en un único servidor con la configuración básica es planear la configuración del Asistente para Introducción.  
  
|Tarea|Descripción|  
|----|--------|  
|Planificar la implementación del cliente|De forma predeterminada, el Asistente para Introducción implementa DirectAccess en todos los equipos portátiles y portátiles del dominio aplicando un filtro WMI al GPO de configuración de cliente|  
|Planeación de la implementación del servidor de DirectAccess|Planea cómo implementar el servidor de DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Planeación de la implementación de cliente  
A la hora de planificar la implementación del cliente, debes tomar dos decisiones:  
  
1.  ¿DirectAccess estará disponible solo para equipos móviles o para cualquier equipo?  
  
    Al configurar los clientes de DirectAccess en el Asistente para Introducción, puede permitir que solo los equipos móviles de los grupos de seguridad especificados se conecten mediante DirectAccess. Si restringe el acceso a los equipos móviles, DirectAccess configura automáticamente un filtro WMI para asegurarse de que el GPO de cliente de DirectAccess se aplica solo a los equipos móviles de los grupos de seguridad especificados. El administrador de DirectAccess necesita permisos para crear o modificar los filtros WMI de directiva de grupo para habilitar esta configuración.  
  
2.  ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    La configuración de DirectAccess se incluye en el GPO de cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que se especifican en el Asistente para Introducción. Puedes especificar grupos de seguridad incluidos en cualquier dominio compatible. Antes de configurar DirectAccess, deben crearse los grupos de seguridad. Puede agregar equipos al grupo de seguridad después de completar la implementación de DirectAccess, pero tenga en cuenta que si agrega equipos cliente que residen en un dominio diferente al grupo de seguridad, el GPO de cliente no se aplicará a estos clientes. Por ejemplo, si creaste SG1 en el dominio A para clientes de DirectAccess y después agregaste clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B. Para evitar este problema, crea un nuevo grupo de seguridad de cliente para cada dominio que contenga equipos cliente. Si no deseas crear un nuevo grupo de seguridad, puedes ejecutar el cmdlet Add-DAClient con el nombre del nuevo GPO para el nuevo dominio.  
  
## <a name="bkmk_2_2_server"></a>Planeación de la implementación del servidor de DirectAccess  
Al planear la implementación del servidor de DirectAccess, hay que tomar varias decisiones:  
  
-   **Topología de red** : hay dos topologías disponibles al implementar un servidor de DirectAccess:  
  
    -   **Dos adaptadores** : con dos adaptadores de red, DirectAccess se puede configurar con un adaptador de red conectado directamente a Internet y el otro está conectado a la red interna. Otra opción consiste en instalar el servidor detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red está conectado a la red perimetral y el otro está conectado a la red interna.  
  
    -   **Adaptador de red único** : en esta configuración, el servidor de DirectAccess se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  
  
-   **Adaptadores de red** : el Asistente de DirectAccess detecta automáticamente los adaptadores de red configurados en el servidor de DirectAccess. Puede asegurarse de que se seleccionan los adaptadores correctos en la página **revisar** .  
  
-   **Certificado IP-https** : dado que no se requiere ninguna PKI en esta implementación, el asistente aprovisiona automáticamente certificados autofirmados para IP-https y el servidor de ubicación de red (si no hay certificados presentes) y habilita automáticamente Kerberos proxi. El Asistente también habilita NAT64 y DNS64 para la traducción de protocolos en el entorno de solo IPv4. Una vez que el asistente haya terminado de aplicar la configuración correctamente, haga clic en **Cerrar**.  
  
-   **Clientes de Windows 7** : no puede habilitar la compatibilidad con los clientes de Windows 7 desde el asistente para introducción. Se puede habilitar desde el Asistente para configuración avanzada. Para obtener más información, vea [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuración de VPN** : antes de configurar DirectAccess, decida si va a proporcionar acceso VPN a los clientes remotos. Debe proporcionar acceso a VPN si tiene equipos cliente de su organización que no admiten la conectividad de DirectAccess (ya sea porque no están administrados o ejecutan un sistema operativo para el que no se admite DirectAccess). El Asistente para Introducción configura la asignación de direcciones IP de VPN mediante DHCP y configura los clientes VPN para que se autentiquen mediante Active Directory.  
  
-   **Tunelización forzada** : Si tiene previsto usar el túnel forzado o puede agregarlo en el futuro, debe usar [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para implementar una configuración de dos túneles. Debido a las consideraciones de seguridad, no se admite el túnel forzado en una configuración de un solo túnel.  
  
## <a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 1: Planear la infraestructura básica de DirectAccess](da-basic-plan-s1-infrastructure.md)  
  


