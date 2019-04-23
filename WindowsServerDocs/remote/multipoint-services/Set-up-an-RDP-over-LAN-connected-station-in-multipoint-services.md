---
title: Configurar una estación de RDP a través de LAN conectados en MultiPoint Services
description: Obtenga información sobre cómo configurar un sistema RDP a través de LAN en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843706"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configurar una estación de RDP a través de LAN conectados en MultiPoint Services
Una estación de RDP a través de LAN conectados es un cliente ligero, de escritorio tradicional o equipo portátil que se conecta a MultiPoint Services en una red de área local (LAN) mediante el protocolo de escritorio remoto (RDP). Para obtener más información sobre este y otros tipos de estación, vea [estaciones de MultiPoint](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Para configurar una estación de MultiPoint mediante un equipo o un cliente ligero en una LAN  
  
1.  Encienda el equipo que está ejecutando MultiPoint Services.  
  
2.  Asegúrese de que el equipo del servidor MultiPoint está conectado a la LAN mediante un conmutador, enrutador u otro dispositivo de red y tiene una dirección IP correcta. (Una dirección IP que comienza por 169.254 (una dirección APIPA) podría indicar un problema con la conexión LAN o que el servidor DHCP no se puede alcanzar o no funciona correctamente).  
  
3.  Conecte el equipo cliente o cliente ligero para la LAN.  
  
4.  Encienda el equipo cliente o un cliente ligero.  
  
5.  En el equipo cliente o un cliente ligero, inicie conexión a Escritorio remoto o una aplicación equivalente y escriba el nombre o dirección IP del equipo que ejecuta MultiPoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurar un dispositivo Windows 10 para la administración remota mediante servicios del conector
Cualquier equipo PC o portátil con Windows 10 se puede administrar de forma remota siempre que:
- se han habilitado los servicios del conector  
- la máquina se agregó en los equipos administrados en el servidor MultiPoint.  

En el equipo que ejecuta Windows 10, siga estos pasos para habilitar el conector de MultiPoint:

1. En el cuadro de búsqueda, escriba "Windows activar o desactivar las características" y seleccione el resultado de búsqueda adecuada. 

2. En la lista de características habilitar **MultiPoint conector**. Esto permitirá a los servicios del conector de MultiPoint que son necesarias para administrar el dispositivo. 

En el servidor MultiPoint:
1. Abra MultiPoint Manager y seleccione **agregar o quitar equipos personales** o **Add o remove MultiPoint Services**.

2. Seleccione los equipos remotos que desea administrar y haga clic en **Aceptar**.  Se le pedirá las credenciales de administrador en los equipos remotos.  Una vez hecho esto, verá los equipos remotos en la ficha Inicio MultiPoint Manager.

Correctamente cuando el Administrador de panel de conjunto puede supervisar los usuarios que trabajan en el dispositivo administrado.

> [!IMPORTANT]  
> Cuando la supervisión administrados no se puede supervisar a los usuarios de Windows 10 dispositivos administratrive excepto el servidor de configuración se han cambiado en consecuencia. Consulte [editar la configuración del servidor](Edit-Server-Settings.md)