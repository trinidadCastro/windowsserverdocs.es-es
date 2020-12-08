---
title: 'Escritorio remoto: permitir el acceso a tu PC desde fuera de la red'
description: Obtén información acerca de las opciones de acceso remoto a tu PC desde fuera de la red del equipo
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: a9cf5f32a2cffb52b56e619e42b22483e7cbe48a
ms.sourcegitcommit: f18097c21e50a09aef2f1937f52608b0042ef0e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96755392"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Escritorio remoto: permitir el acceso a tu PC desde fuera de la red del equipo

>Se aplica a: Windows 10, Windows Server 2016

Cuando te conectas a tu PC mediante un cliente de Escritorio remoto, estás creando una conexión de punto a punto. Esto significa que necesitas obtener acceso directo al equipo (a veces se denomina “host”). Si necesitas conectarte al equipo desde fuera de la red en la que se ejecuta el mismo, debes habilitar ese acceso. Tienes un par de opciones: usar el enrutamiento de puerto o configurar una VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar el enrutamiento de puerto en el enrutador

El enrutamiento de puerto simplemente asigna el puerto en la dirección IP del enrutador (tu IP pública) al puerto y la dirección IP de el equipo al que quiere obtener acceso.

Los pasos específicos para habilitar el enrutamiento de puerto dependen del enrutador que estés usando, por lo que deberás buscar en línea las instrucciones del enrutador. Para obtener una explicación general de los pasos, echa un vistazo a [wikiHow to Set Up Port Forwarding on a Router](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router) (wikiHow para configurar el enrutamiento de puerto en un enrutador).

Antes de asignar el puerto necesitarás lo siguiente:

- Dirección IP interna del equipo: Ve a **Configuración > Redes e Internet > Estado > Ver las propiedades de la red** . Encuentra la configuración de red que tenga el estado "Operativa" y obtén la **dirección IPv4**.

   ![Configuración de red operativa](../media/rdclient-operational-network.png)

- Tu dirección IP pública (la IP del enrutador). Tienes varias maneras de encontrarla: puedes buscar (en Bing o Google) "mi IP" o consultar las [propiedades de la red Wi-Fi](https://binged.it/2Gwob34) (para Windows 10).
- Número de puerto que se va a asignar. En la mayoría de los casos es 3389, ya que es el puerto predeterminado que usan las conexiones del Escritorio remoto.
- Acceso de administrador a tu enrutador.

   >[!WARNING]
   > Vas a abrir el equipo a Internet: asegúrate de que tienes una contraseña segura establecida en el equipo.

Después de asignar el puerto, podrás conectarte al equipo host desde fuera de la red local; para ello, solo debes conectarte a la dirección IP pública del enrutador (esto se indica en la segunda viñeta anterior).

Recuerda que la dirección IP del enrutador puede cambiar: tu proveedor de acceso a Internet (ISP) puede asignarte una nueva IP en cualquier momento. Para evitar este problema, usa un DNS dinámico: esto te permitirá conectarte al equipo mediante un nombre de dominio fácil de recordar, en lugar de la dirección IP. El enrutador actualizará automáticamente el servicio DDNS con su nueva dirección IP, en caso de que cambie.

Con la mayoría de los enrutadores, puedes definir qué IP de origen o red de origen puede usar la asignación de puertos. Por lo tanto, si sabes que solo vas a conectarte desde el trabajo, puedes agregar la dirección IP de la red del trabajo; gracias a ello, no tendrás que abrir el puerto a todo Internet. Si el host que estás usando para conectarte usa una dirección IP dinámica, configura la restricción de origen para permitir el acceso desde todo el rango de ese ISP en particular.

También puedes configurar una dirección IP estática en tu PC para que la dirección IP interna no cambie. Si lo haces, el enrutamiento de puerto del enrutador siempre apuntará a la dirección IP correcta.


## <a name="use-a-vpn"></a>Usar una VPN

Si te conectas a una red de área local mediante una red privada virtual (VPN), no es necesario abrir el equipo a Internet. En cambio, cuando te conectes a la VPN, tu cliente de RD actuará como si fuera parte de la misma red y podrá obtener acceso al equipo. Existe una serie de servicios de VPN que puedes buscar y usar según lo que te convenga.
