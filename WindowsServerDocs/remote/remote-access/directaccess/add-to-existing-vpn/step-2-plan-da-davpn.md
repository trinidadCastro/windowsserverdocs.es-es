---
title: Paso 2 Planear la implementación de DirectAccess
description: Este tema forma parte de la Guía de agregar DirectAccess a una implementación de acceso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b4e0e8647fa2011eae73efa8bcbd696c422f12c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859686"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>Paso 2 Planear la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planificar la infraestructura de acceso remoto, el siguiente paso para habilitar DirectAccess consiste en planificar la configuración del asistente para habilitar DirectAccess.  
  
|Tarea|Descripción|  
|----|--------|  
|Planificar la implementación del cliente|Planifica cómo permitir que los equipos cliente se conecten utilizando DirectAccess. Decide qué equipos administrados se configurarán como clientes de DirectAccess.|  
|Planificar la implementación del servidor de acceso remoto|Planifica cómo implementar el servidor de acceso remoto.|  
  
## <a name="bkmk_2_1_client"></a>Planear la implementación de cliente  
A la hora de planificar la implementación del cliente, debes tomar dos decisiones:  
  
-   ¿DirectAccess estará disponible solo para equipos móviles o para cualquier equipo?  
  
    Al configurar clientes de DirectAccess con el asistente para habilitar DirectAccess, es posible permitir que solamente se conecten utilizando DirectAccess los equipos móviles de los grupos de seguridad especificados. Si restringes el acceso a los equipos móviles, el acceso remoto configura automáticamente un filtro WMI para garantizar que el GPO de cliente de DirectAccess se aplique solo a los equipos móviles de los grupos de seguridad especificados. El administrador del acceso remoto necesita permisos si desea crear o modificar los filtros WMI de directiva de grupo para habilitar esta configuración.  
  
-   ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    La configuración de DirectAccess se incluye en el GPO de cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifiques en el asistente para habilitar DirectAccess. Puedes especificar grupos de seguridad incluidos en cualquier dominio compatible. Antes de configurar el acceso remoto, debes crear los grupos de seguridad. Puedes agregar equipos al grupo de seguridad después de finalizar la implementación del acceso remoto, pero ten en cuenta que si agregas equipos cliente que residen en un dominio diferente al del grupo de seguridad, el GPO de cliente no se aplicará a dichos clientes. Por ejemplo, si creaste SG1 en el dominio A para clientes de DirectAccess y después agregaste clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B. Para evitar este problema, crea un nuevo grupo de seguridad de cliente para cada dominio que contenga equipos cliente. Si no deseas crear un nuevo grupo de seguridad, puedes ejecutar el cmdlet Add-DAClient con el nombre del nuevo GPO para el nuevo dominio.  
  
## <a name="bkmk_2_2_server"></a>Planear la implementación de servidor de acceso remoto  
Debes tomar una serie de decisiones al planificar la implementación del servidor de acceso remoto:  
  
-   **Topología de red**-hay dos topologías disponibles para implementar un servidor de acceso remoto:  
  
    -   **Dos adaptadores**-con dos adaptadores de red, acceso remoto puede configurarse con un adaptador de red conectado directamente a Internet y el otro está conectado a la red interna. Otra opción consiste en instalar el servidor detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red está conectado a la red perimetral y el otro está conectado a la red interna.  
  
    -   **Adaptador de red único**-en esta configuración, el acceso remoto se instala servidor detrás de un dispositivo perimetral como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  
  
-   **Los adaptadores de red**-Asistente para habilitar el DirectAccess detecta automáticamente los adaptadores de red configurados en el servidor de acceso remoto, en función de las interfaces utilizadas por la VPN. Debes asegurarte de que están seleccionados los adaptadores correctos.  
  
-   **Dirección ConnectTo**-los equipos cliente usan la dirección ConnectTo para conectarse al servidor de acceso remoto. La dirección que elijas debe coincidir con el nombre del firmante del certificado IP-HTTPS que implementes para la conexión IP-HTTPS, y debe estar disponible en el DNS público. Consulta Planificar certificados de sitios web para IP-HTTPS.  
  
-   **Certificado IP-HTTPS**-si VPN SSTP está configurado, el Asistente para habilitar DirectAccess selecciona el certificado que SSTP utilizado para IP-HTTPS. Si VPN SSTP no está configurado, el asistente intentará comprobar si se ha configurado un certificado para IP-HTTPS. De no ser así, aprovisionará automáticamente certificados autofirmados para IP-HTTPS y habilitará automáticamente la autenticación Kerberos. El asistente también habilitará NAT64 y DNS64 para la traducción de protocolo en el entorno de solo IPv4.  
  
-   **Prefijos IPv6**-si el asistente detecta que se ha implementado IPv6 en los adaptadores de red, crean automáticamente prefijos IPv6 para la red interna, un prefijo IPv6 para asignar a los equipos cliente de DirectAccess y un prefijo IPv6 para asignarlo a VPN equipos cliente. Si los prefijos generados automáticamente no son correctos para su infraestructura ISATAP o IPv6 nativa, debes cambiarlos de forma manual. Consulte 1.1 topología de red y del servidor de planeación y configuración.  
  
-   **Los clientes de Windows 7**: de forma predeterminada, los equipos cliente de Windows 7 no se pueden conectar a una implementación de acceso remoto de Windows Server 2012. Si tiene equipos cliente Windows 7 en su organización que necesiten acceso remoto a los recursos internos, puede permitirles conectarse. Todos los equipos cliente a los que desees permitir el acceso a los recursos internos deben pertenecer a un grupo de seguridad que especifiques en el asistente para habilitar DirectAccess.  
  
    > [!NOTE]
    > Permitir que el cliente de Windows 7 los equipos se conecten mediante DirectAccess, deberá usar la autenticación de certificado de equipo.
  
-   **Autenticación**-Asistente para habilitar el DirectAccess usa Active Directory para autenticar las credenciales de usuario. Para implementar la autenticación en dos fases, consulta [Implementar el acceso remoto con autenticación OTP](../../ras/otp/Deploy-RA-OTP.md).  
  

  


