---
title: Configuración de una estación conectada RDP a través de LAN en Multipoint Services
description: Aprenda a configurar un sistema RDP a través de LAN en Multipoint Services
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9c43f207ad968c76fef027f09c2a13f1b36f1998
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971732"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configuración de una estación conectada RDP a través de LAN en Multipoint Services
Una estación conectada RDP a través de LAN es un cliente ligero, un escritorio tradicional o un equipo portátil que se conecta a multipoint Services en una red de área local (LAN) mediante el Protocolo de escritorio remoto (RDP). Para obtener más información sobre este y otros tipos de estación, consulte [estaciones Multipoint](MultiPoint-services-Stations.md).

## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Para configurar una estación Multipoint mediante un equipo o un cliente ligero en una LAN

1.  Encienda el equipo que ejecuta Multipoint Services.

2.  Asegúrese de que el equipo de MultiPoint Server esté conectado a la LAN mediante un conmutador, un enrutador u otro dispositivo de red y que tenga una dirección IP adecuada. (Una dirección IP que empieza por 169,254 (una dirección APIPA) puede indicar que hay un problema con la conexión LAN o que el servidor DHCP no se puede alcanzar o no funciona correctamente.)

3.  Conecte el equipo cliente o el cliente ligero a la LAN.

4.  Encienda el equipo cliente o el cliente ligero.

5.  En el equipo cliente o en el cliente ligero, inicie Conexión a Escritorio remoto o una aplicación equivalente y escriba el nombre o la dirección IP del equipo que ejecuta Multipoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurar un dispositivo de Windows 10 para la administración remota mediante servicios de conector
Cualquier equipo o portátil que ejecute Windows 10 se puede administrar de forma remota siempre que:
- se han habilitado los servicios del conector
- la máquina se ha agregado a los equipos administrados en MultiPoint Server.

En el equipo que ejecuta Windows 10, siga estos pasos para habilitar el conector Multipoint:

1. En el cuadro de búsqueda, escriba "activar o desactivar las características de Windows" y seleccione el resultado de búsqueda adecuado.

2. En la lista de características, habilite el **conector Multipoint**. Esto habilitará los servicios de Multipoint Connector necesarios para administrar el dispositivo.

En MultiPoint Server:
1. Abra Multipoint Manager y seleccione **Agregar o quitar equipos personales** o **Agregar o quitar Multipoint Services**.

2. Seleccione los equipos remotos que desea administrar y haga clic en **Aceptar**.  Se le pedirán las credenciales de administrador en los equipos remotos.  Una vez hecho esto, verá los equipos remotos en la pestaña Inicio de Multipoint Manager.

Cuando el administrador de paneles se configura correctamente, puede supervisar los usuarios que trabajan en el dispositivo administrado.

> [!IMPORTANT]
> Al supervisar dispositivos Windows 10 administrados, los usuarios de administratrive no se pueden supervisar, salvo que se ha cambiado la configuración del servidor en consecuencia. Consulte [Editar configuración del servidor](Edit-Server-Settings.md)