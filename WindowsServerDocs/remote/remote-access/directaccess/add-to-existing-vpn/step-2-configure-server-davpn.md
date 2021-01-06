---
title: 'Paso 2: configurar el servidor VPN de DirectAccess'
description: Obtenga información sobre cómo configurar las opciones de cliente y servidor necesarias para una implementación de acceso remoto básica mediante el Asistente para habilitar DirectAccess.
manager: brianlic
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e0419184c07f23faf458af4c37efb7761cc93f47
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947051"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Paso 2: configurar el servidor VPN de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar los ajustes de servidor y cliente necesarios para una implementación de acceso remoto básica mediante el asistente para habilitar DirectAccess.

En la tabla siguiente se proporciona información general sobre los pasos que puede completar con este tema.

|Tarea       |Descripción|
|-----------|-----------|
|Configurar los clientes de DirectAccess|Configura el servidor de acceso remoto con los grupos de seguridad que contienen a los clientes de DirectAccess.|
|Configurar la topología de red|Configura los ajustes del servidor de acceso remoto.|
|Configurar la lista de búsqueda de sufijos DNS|Modifica la lista de búsqueda de sufijos, si así lo deseas.|
|Configuración del GPO|Modifica los GPO, si así lo deseas.|

## <a name="to-start-the-enable-directacces-wizard"></a>Cómo iniciar el asistente para habilitar DirectAcces

1. En Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **acceso remoto**. El Asistente para habilitar DirectAccess se inicia automáticamente a menos que haya seleccionado no **volver a mostrar esta pantalla**.

2. Si el asistente no se inicia automáticamente, haz clic con el botón secundario en el nodo de servidores en el árbol Enrutamiento y acceso remoto y haz clic en **Habilitar DirectAccess**.

3. Haga clic en **Next**.

## <a name="configure-directaccess-clients"></a>Configurar los clientes de DirectAccess

Para que un equipo cliente esté aprovisionado para utilizar DirectAccess, debe pertenecer al grupo de seguridad seleccionado. Una vez configurado DirectAccess, los equipos cliente del grupo de seguridad están aprovisionados para recibir la directiva de grupo de DirectAccess.

1. En la página **Seleccionar grupos**, haz clic en **Agregar**.

2. En el cuadro de diálogo **Seleccionar grupos**, seleccione los grupos de seguridad que contengan a los equipos cliente de DirectAccess.

3. Activa la casilla **Habilitar DirectAccess solo para equipos móviles** para que solo los equipos móviles puedan acceder a la red interna.

4. Activa la casilla **Usar túnel forzado** para enrutar todo el tráfico cliente (a la red interna y a Internet) a través del servidor de acceso remoto.

5. Haga clic en **Next**.

## <a name="configure-the-network-topology"></a>Configurar la topología de red

Para implementar el acceso remoto, debes configurar el servidor de acceso remoto con los adaptadores de red correctos, una dirección URL pública para el servidor de acceso remoto al que pueden conectarse los equipos cliente (la dirección a la que se conectan) y un certificado IP-HTTPS cuyo firmante coincida con la dirección a la que se conectan.

1. En la página **Topología de red**, haz clic en la topología de implementación que se utilizará en tu organización. En **Escriba el nombre público o dirección IPv4 que usan los clientes para conectarse al servidor de acceso remoto**, escribe el nombre público de la implementación (este nombre coincide con el nombre del firmante del certificado IP-HTTPS, por ejemplo, edge1.contoso.com) y haz clic en **Siguiente**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurar la lista de búsqueda de sufijos DNS

En el caso de los clientes DNS, puedes configurar una lista de búsqueda de sufijos de dominio DNS que amplíe o revise sus capacidades de búsqueda DNS. Al añadir sufijos a la lista, puedes buscar nombres de equipos cortos e incompletos en más de un dominio DNS especificado. A continuación, si se produce un error en una consulta DNS, el servicio Cliente DNS puede usar esta lista para agregar otras finalizaciones de sufijo de nombre al nombre original y repetir las consultas DNS en el servidor DNS en busca de estos FQDN alternativos.

1. Selecciona **Configurar clientes de DirectAccess con la lista de búsqueda de sufijos de cliente DNS** para especificar otros sufijos para las búsquedas de nombres de clientes.

2. Escriba un nuevo nombre de sufijo en **nuevo sufijo** y, a continuación, haga clic en **Agregar**. Además, puede cambiar el orden de búsqueda y quitar los sufijos de los **sufijos de dominio que se usarán**.

>Tenga en cuenta En un escenario de espacio de nombres \( no contiguo en el que uno o varios equipos de dominio tienen un sufijo DNS que no coincide con el dominio de Active Directory al que pertenecen los equipos \) , debe asegurarse de que la lista de búsqueda se personalice para incluir todos los sufijos necesarios. El asistente de acceso remoto configurará de manera predeterminada el nombre DNS de Active Directory como sufijo DNS principal en el cliente. El administrador debe asegurarse de que agrega el sufijo DNS que utilizan los clientes para la resolución de nombres.

En el caso de los equipos y servidores, el siguiente comportamiento predeterminado de la búsqueda de DNS está predeterminado y se usa al completar y resolver nombres cortos y no completos. Cuando la lista de búsqueda de sufijos está vacía o no se especifica, el sufijo DNS principal del equipo se anexa a nombres cortos no completos y se usa una consulta DNS para resolver el FQDN resultante.

Si se produce un error en esta consulta, el equipo puede probar consultas adicionales para FQDN alternativos anexando cualquier sufijo DNS específico de la conexión configurado para las conexiones de red. Si no se configura ningún sufijo específico de la conexión o se produce un error en las consultas de los FQDN específicos de la conexión resultante, entonces el cliente puede comenzar a reintentar consultas en función de una reducción sistemática del sufijo principal (también conocido como devolución).

Por ejemplo, si el sufijo principal es "example.microsoft.com", el proceso de devolución puede reintentar las consultas para el nombre corto buscándolo en los dominios "microsoft.com" y "com".

Cuando la lista de búsqueda de sufijos no está vacía y tiene al menos un sufijo DNS especificado, los intentos de calificar y resolver nombres DNS cortos se limitan a buscar solo los FQDN que se permiten en la lista de sufijos especificada.

Si las consultas de todos los FQDN formados como resultado de anexar e intentar cada sufijo de la lista no se resuelven, el proceso de consulta produce un error y genera el resultado "nombre no encontrado".

> [!WARNING]
> Si se utiliza la lista de sufijos de dominio, los clientes siguen enviando consultas alternativas basadas en diferentes nombres de dominio DNS cuando una consulta no se responda o no se resuelva. Si un nombre se resuelve al utilizar una entrada de la lista de sufijos, no se intentarán las entradas de la lista que no se han usado. Por esta razón, lo más eficaz es ordenar la lista con los sufijos de dominio más utilizados en primer lugar.
>
> Las búsquedas de sufijos de nombres de dominio se usan únicamente cuando una entrada de nombre DNS no está completa. Para completar un nombre DNS, debe ponerse un punto (.) al final del nombre.

## <a name="gpo-configuration"></a>Configuración del GPO

Cuando se configura el acceso remoto, la configuración de DirectAccess se recopila en objetos de directiva de grupo (GPO).

En **configuración de GPO**, se muestran el nombre del GPO del servidor de DirectAccess y el nombre del GPO del cliente. Además, permite modificar los ajustes de selección del GPO.

Dos GPO se rellenan automáticamente con la configuración de DirectAccess y se distribuyen de esta manera:

1. **GPO de cliente de DirectAccess**. Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad especificados para los equipos cliente.

2. **GPO de servidor de DirectAccess**. Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como un servidor de acceso remoto en la implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.

## <a name="summary"></a>Resumen

Una vez completada la configuración de acceso remoto, se muestra el **Resumen** . Puede cambiar la configuración establecida o hacer clic en **Finalizar** para aplicar la configuración.
