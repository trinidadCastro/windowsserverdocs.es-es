---
title: Paso 2 planear la implementación de DirectAccess
description: Este tema forma parte de la guía agregar DirectAccess a una implementación de acceso remoto (VPN) existente para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8a31dc9362e9165ae2385de62f8146b769fc8563
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969242"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>Paso 2 planear la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planificar la infraestructura de acceso remoto, el siguiente paso para habilitar DirectAccess consiste en planificar la configuración del asistente para habilitar DirectAccess.

|Tarea|Descripción|
|----|--------|
|Planeación de la implementación de cliente|Planifica cómo permitir que los equipos cliente se conecten utilizando DirectAccess. Decide qué equipos administrados se configurarán como clientes de DirectAccess.|
|Planificar la implementación del servidor de acceso remoto|Planifica cómo implementar el servidor de acceso remoto.|

## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Planeación de la implementación de cliente
A la hora de planificar la implementación del cliente, debes tomar dos decisiones:

-   ¿DirectAccess estará disponible solo para equipos móviles o para cualquier equipo?

    Al configurar clientes de DirectAccess con el asistente para habilitar DirectAccess, es posible permitir que solamente se conecten utilizando DirectAccess los equipos móviles de los grupos de seguridad especificados. Si restringes el acceso a los equipos móviles, el acceso remoto configura automáticamente un filtro WMI para garantizar que el GPO de cliente de DirectAccess se aplique solo a los equipos móviles de los grupos de seguridad especificados. El administrador del acceso remoto necesita permisos si desea crear o modificar los filtros WMI de directiva de grupo para habilitar esta configuración.

-   ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?

    La configuración de DirectAccess se incluye en el GPO de cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifiques en el asistente para habilitar DirectAccess. Puedes especificar grupos de seguridad incluidos en cualquier dominio compatible. Antes de configurar el acceso remoto, debes crear los grupos de seguridad. Puedes agregar equipos al grupo de seguridad después de finalizar la implementación del acceso remoto, pero ten en cuenta que si agregas equipos cliente que residen en un dominio diferente al del grupo de seguridad, el GPO de cliente no se aplicará a dichos clientes. Por ejemplo, si creaste SG1 en el dominio A para clientes de DirectAccess y después agregaste clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B. Para evitar este problema, crea un nuevo grupo de seguridad de cliente para cada dominio que contenga equipos cliente. Si no deseas crear un nuevo grupo de seguridad, puedes ejecutar el cmdlet Add-DAClient con el nombre del nuevo GPO para el nuevo dominio.

## <a name="planning-for-remote-access-server-deployment"></a><a name="bkmk_2_2_server"></a>Planificar la implementación del servidor de acceso remoto
Debes tomar una serie de decisiones al planificar la implementación del servidor de acceso remoto:

-   **Topología de red**: hay dos topologías disponibles al implementar un servidor de acceso remoto:

    -   **Dos adaptadores**: con dos adaptadores de red, el acceso remoto se puede configurar con un adaptador de red conectado directamente a Internet y el otro está conectado a la red interna. Otra opción consiste en instalar el servidor detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red está conectado a la red perimetral y el otro está conectado a la red interna.

    -   **Adaptador de red único**: en esta configuración, el servidor de acceso remoto se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. El adaptador de red se conecta a la red interna.

-   **Adaptadores de red**: el Asistente para habilitar DirectAccess detecta automáticamente los adaptadores de red configurados en el servidor de acceso remoto, en función de las interfaces usadas por VPN. Debes asegurarte de que están seleccionados los adaptadores correctos.

-   **Dirección ConnectTo**: los equipos cliente usan la dirección ConnectTo para conectarse al servidor de acceso remoto. La dirección que elijas debe coincidir con el nombre del firmante del certificado IP-HTTPS que implementes para la conexión IP-HTTPS, y debe estar disponible en el DNS público. Consulta Planificar certificados de sitios web para IP-HTTPS.

-   **Certificado IP-https**: Si la VPN SSTP está configurada, el Asistente para habilitar DirectAccess toma el certificado usado por SSTP para IP-https. Si VPN SSTP no está configurado, el asistente intentará comprobar si se ha configurado un certificado para IP-HTTPS. De no ser así, aprovisionará automáticamente certificados autofirmados para IP-HTTPS y habilitará automáticamente la autenticación Kerberos. El asistente también habilitará NAT64 y DNS64 para la traducción de protocolo en el entorno de solo IPv4.

-   **Prefijos IPv6**: Si el asistente detecta que IPv6 se ha implementado en los adaptadores de red, crea automáticamente prefijos IPv6 para la red interna, un prefijo IPv6 para asignar a los equipos cliente de DirectAccess y un prefijo IPv6 para asignarlos a los equipos cliente de VPN. Si los prefijos generados automáticamente no son correctos para su infraestructura ISATAP o IPv6 nativa, debes cambiarlos de forma manual. Consulta 1.1 Planificar la topología y la configuración de la red y del servidor.

-   **Clientes de Windows 7**: de forma predeterminada, los equipos cliente de Windows 7 no pueden conectarse a una implementación de acceso remoto de windows Server 2012. Si tiene equipos cliente de Windows 7 en su organización que requieren acceso remoto a recursos internos, puede permitirles conectarse. Todos los equipos cliente a los que desees permitir el acceso a los recursos internos deben pertenecer a un grupo de seguridad que especifiques en el asistente para habilitar DirectAccess.

    > [!NOTE]
    > Permitir que los equipos cliente de Windows 7 se conecten mediante DirectAccess requiere que se use la autenticación de certificado de equipo.

-   **Autenticación**: el Asistente para habilitar DirectAccess usa Active Directory para autenticar las credenciales del usuario. Para implementar la autenticación en dos fases, consulta [Implementar el acceso remoto con autenticación OTP](../../ras/otp/Deploy-RA-OTP.md).





