---
title: Paso 2 configurar el servidor VPN de DirectAccess
description: Este tema forma parte de la Guía de agregar DirectAccess a una implementación de acceso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dfd5663a07bf10f7c27acb25d2dec9af3e7c7b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281837"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Paso 2 configurar el servidor VPN de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar los ajustes de servidor y cliente necesarios para una implementación de acceso remoto básica mediante el asistente para habilitar DirectAccess.

En la tabla siguiente proporciona información general de los pasos que puede completar mediante el uso de este tema.

|Tarea       |Descripción|
|-----------|-----------|
|Configurar los clientes de DirectAccess|Configura el servidor de acceso remoto con los grupos de seguridad que contienen a los clientes de DirectAccess.|
|Configurar la topología de red|Configura los ajustes del servidor de acceso remoto.|
|Configurar la lista de búsqueda de sufijos DNS|Modifica la lista de búsqueda de sufijos, si así lo deseas.|
|Configuración del GPO|Modifica los GPO, si así lo deseas.|

## <a name="to-start-the-enable-directacces-wizard"></a>Cómo iniciar el asistente para habilitar DirectAcces

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **acceso remoto**. El Asistente para habilitar DirectAccess se inicia automáticamente a menos que hayas seleccionado **no volver a mostrar esta pantalla**. 

2. Si el asistente no se inicia automáticamente, haga clic en el nodo del servidor en el árbol de enrutamiento y acceso remoto y, a continuación, haga clic en **habilitar DirectAccess**.

3. Haz clic en **Siguiente**.

## <a name="configure-directaccess-clients"></a>Configurar los clientes de DirectAccess

Para que un equipo cliente esté aprovisionado para utilizar DirectAccess, debe pertenecer al grupo de seguridad seleccionado. Una vez configurado DirectAccess, los equipos cliente del grupo de seguridad están aprovisionados para recibir la directiva de grupo de DirectAccess.

1. En la página **Seleccionar grupos** , haz clic en **Agregar**.

2. En el cuadro de diálogo **Seleccionar grupos**, seleccione los grupos de seguridad que contengan a los equipos cliente de DirectAccess.

3. Activa la casilla **Habilitar DirectAccess solo para equipos móviles** para que solo los equipos móviles puedan acceder a la red interna.

4. Activa la casilla **Usar túnel forzado** para enrutar todo el tráfico cliente (a la red interna y a Internet) a través del servidor de acceso remoto.

5. Haz clic en **Siguiente**.

## <a name="configure-the-network-topology"></a>Configurar la topología de red

Para implementar el acceso remoto, debes configurar el servidor de acceso remoto con los adaptadores de red correctos, una dirección URL pública para el servidor de acceso remoto al que pueden conectarse los equipos cliente (la dirección a la que se conectan) y un certificado IP-HTTPS cuyo firmante coincida con la dirección a la que se conectan.

1. En la página **Topología de red** , haz clic en la topología de implementación que se utilizará en tu organización. En **Escriba el nombre público o dirección IPv4 que usan los clientes para conectarse al servidor de acceso remoto**, escribe el nombre público de la implementación (este nombre coincide con el nombre del firmante del certificado IP-HTTPS, por ejemplo, edge1.contoso.com) y haz clic en **Siguiente**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurar la lista de búsqueda de sufijos DNS

En el caso de los clientes DNS, puedes configurar una lista de búsqueda de sufijos de dominio DNS que amplíe o revise sus capacidades de búsqueda DNS. Al añadir sufijos a la lista, puedes buscar nombres de equipos cortos e incompletos en más de un dominio DNS especificado. A continuación, si se produce un error en una consulta DNS, el servicio cliente DNS puede usar esta lista para agregar otras finalizaciones de sufijo de nombre a su nombre original y repetir las consultas DNS al servidor DNS para estos FQDN alternativos.

1. Selecciona **Configurar clientes de DirectAccess con la lista de búsqueda de sufijos de cliente DNS** para especificar otros sufijos para las búsquedas de nombres de clientes.

2. Escriba un nombre de sufijo nuevo en **nuevo sufijo** y, a continuación, haga clic en **agregar**. Además, puede cambiar el orden de búsqueda y eliminar sufijos desde **sufijos de dominio para usar**.

>[NOTA] En un escenario de espacio de nombres no contiguo \(donde uno o más equipos de dominio tiene un sufijo DNS que no coincide con el dominio de Active Directory al que pertenecen los equipos\), debe asegurarse de que la lista de búsqueda se personalice para incluir todos los sufijos necesarios. El asistente de acceso remoto configurará de manera predeterminada el nombre DNS de Active Directory como sufijo DNS principal en el cliente. El administrador debe asegurarse de que agrega el sufijo DNS que utilizan los clientes para la resolución de nombres.

Para los equipos y servidores, el siguiente comportamiento de búsqueda DNS de forma predeterminada es predeterminado y usa para completar y resolver nombres cortos y no completos. Cuando la lista de búsqueda de sufijos está vacía o no especificado, el sufijo DNS principal del equipo está anexado a short nombres no completos y se usa una consulta DNS para resolver el FQDN resultante. 

Si se produce un error en esta consulta, el equipo puede intentar consultas adicionales de FQDN alternativos mediante la anexión de cualquier sufijo DNS específico de la conexión configurada para las conexiones de red. Si hay configurado ningún sufijo específico de la conexión o las consultas de estos FQDN específicos de la conexión resultantes no pueden, a continuación, el cliente puede volver a intentar las consultas basadas en la reducción sistemática del sufijo primario (también denominado devolución).

Por ejemplo, si el sufijo primario es "ejemplo.Microsoft.com", el proceso de devolución puede volver a intentar las consultas del nombre corto, búsquelo en los dominios "microsoft.com" y "com".

Cuando el sufijo de búsqueda lista no está vacía y tiene al menos un sufijo DNS especificado, los intentos de certificar y resolver nombres DNS cortos se limitan a la búsqueda únicamente aquellos FQDN que se realiza gracias a la lista de sufijos especificados. 

Si las consultas de todos los FQDN formados como resultado de anexar e intentar cada sufijo de la lista no se resuelven, el proceso de consulta produce un error y genera el resultado "nombre no encontrado". 

> [!WARNING]
> Si se utiliza la lista de sufijos de dominio, los clientes siguen enviando consultas alternativas basadas en diferentes nombres de dominio DNS cuando una consulta no se responda o no se resuelva. Si un nombre se resuelve al utilizar una entrada de la lista de sufijos, no se intentarán las entradas de la lista que no se han usado. Por esta razón, lo más eficaz es ordenar la lista con los sufijos de dominio más utilizados en primer lugar.
> 
> Las búsquedas de sufijos de nombres de dominio se usan únicamente cuando una entrada de nombre DNS no está completa. Para completar un nombre DNS, debe ponerse un punto (.) al final del nombre.

## <a name="gpo-configuration"></a>Configuración del GPO

Al configurar el acceso remoto, la configuración de DirectAccess se recopila en los objetos de directiva de grupo (GPO). 

En **configuración del GPO**, muestra el nombre de GPO de servidor de DirectAccess y el nombre de GPO de cliente. Además, permite modificar los ajustes de selección del GPO.

Dos GPO se rellenan automáticamente con la configuración de DirectAccess y distribuidos de esta manera:

1. **GPO de cliente de DirectAccess**. Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad especificados para los equipos cliente.

2. **GPO de servidor de DirectAccess**. Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como servidor de acceso remoto en la implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.

## <a name="summary"></a>Resumen

Una vez completada la configuración de acceso remoto el **resumen** se muestra. Puede cambiar la configuración o haga clic en **finalizar** para aplicar la configuración.
