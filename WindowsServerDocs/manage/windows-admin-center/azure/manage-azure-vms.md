---
title: Administrar máquinas virtuales de IaaS de Azure
description: Administrar máquinas virtuales de IaaS de Azure con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297104"
---
# Administrar máquinas virtuales de IaaS de Azure con Windows Admin Center

Puedes usar Windows Admin Center para administrar tus máquinas virtuales de Azure, así como los equipos locales. Hay varias configuraciones diferentes posibles: elegir la configuración que tenga sentido para tu entorno:
- [Administrar máquinas virtuales de Azure desde una puerta de enlace de Windows Admin Center local](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Administrar máquinas virtuales de Azure desde una puerta de enlace de Windows Admin Center instalado en una VM de Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## Administrar con una puerta de enlace de Windows Admin Center local

Si ya has instalado Windows Admin Center en una puerta de enlace local (ya sea en Windows 10 o Windows Server 2016), puedes usar esta misma puerta de enlace para administrar Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 Máquinas virtuales en Azure. 

### Conectarse a las máquinas virtuales con una dirección IP pública

Si el destino de las máquinas virtuales (las VM que desea administrar con Windows Admin Center) tiene direcciones IP pública, agregarlos a la puerta de enlace de Windows Admin Center por dirección IP o FQDN. Hay un par consideraciones que hay que tener en cuenta:

- Debe habilitar el acceso de WinRM para el máquina virtual de destino ejecutando lo siguiente en PowerShell o el símbolo del sistema en el destino de la máquina virtual: `winrm quickconfig`
- Si se no has unido al dominio la VM de Azure, la máquina virtual se comporta como un servidor de grupo de trabajo, por lo que tendrás que tener en que cuenta para [usar Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- También debes habilitar las conexiones entrantes a puerto 5985 para WinRM a través de HTTP en orden para Windows Admin Center administrar la máquina virtual de destino:
   1. En la máquina virtual para habilitar las conexiones entrantes a puerto 5985 en el sistema operativo invitado de destino, ejecute el siguiente script de PowerShell:   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. También debe abrir el puerto en redes de Azure:

    - Seleccione la máquina virtual de Azure, seleccione **redes**, a continuación, **Agregar regla de puerto de entrada**. 
    - Asegúrate de que **básica** está seleccionada en la parte superior del panel **Agregar regla de seguridad entrante** .
    - En el campo de **intervalos de puertos** , escribe **5985**.
    
    Si la puerta de enlace de Windows Admin Center tiene una dirección IP estática, puedes seleccionar para permitir que solo WinRM acceso entrante desde la puerta de enlace de Windows Admin Center para aumentar la seguridad.
    Para ello, seleccione **Opciones avanzadas** en la parte superior del panel **Agregar regla de seguridad entrante** .

    Para el **código fuente**, selecciona **Las direcciones IP**y luego escribe la dirección IP de origen correspondiente a la puerta de enlace de Windows Admin Center.

    - **Protocolo** de seleccionar **TCP**.
    - El resto puede dejarse como valor predeterminado.

> [!NOTE]
> Debes crear una regla de puerto personalizado. La regla de puerto de WinRM proporcionada por las redes de Azure utiliza el puerto 5986 (a través de HTTPS) en lugar de 5985 (a través de HTTP). 

### Conectarse a las máquinas virtuales sin una dirección IP pública

Si el destino de las máquinas virtuales de Azure no tienen direcciones IP pública y quieres administrar estas máquinas virtuales de una puerta de enlace de Windows Admin Center implementado en la red local, debes configurar la red local para que tenga conectividad a la VNet en los que son el destino de las máquinas virtuales conectado. Hay 3 maneras, puedes hacerlo: ExpressRoute, VPN de sitio a sitio o punto a sitio VPN. [Obtén información sobre qué opción de conectividad tiene sentido en su entorno.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Si deseas usar una VPN de punto a sitio para conectar la puerta de enlace de Windows Admin Center a una VNet de Azure para administrar máquinas virtuales de Azure en ese VNet, puedes usar la característica de [Adaptador de red de Azure](https://aka.ms/WACNetworkAdapter) en Windows Admin Center. Para ello, se conecten al servidor en el que está instalado Windows Admin Center, ve a la herramienta de red y selecciona "Agregar adaptador de red de Azure". Al proporcionar la información necesaria y haz clic en "Configurar", Windows Admin Center configurará una VPN de punto a sitio para el VNet de Azure que especifiques, tras lo cual, puede conectarse a y administrar las VM de Azure desde la puerta de enlace de Windows Admin Center local.

Asegurarse de que se está ejecutando en el destino de las máquinas virtuales WinRM ejecutando lo siguiente en PowerShell o el símbolo del sistema en el destino de la máquina virtual: `winrm quickconfig`

Si se no has unido al dominio la VM de Azure, la máquina virtual se comporta como un servidor de grupo de trabajo, por lo que tendrás que tener en que cuenta para [usar Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si te encuentras algún problema, consulte la [Solución de problemas de Windows Admin Center](../support/troubleshooting.md) para ver si son necesarios para la configuración de los pasos adicionales (por ejemplo, si va a conectarse con una cuenta de administrador local o no están unidos a un dominio).

## Usar una puerta de enlace de Windows Admin Center implementado en Azure

Puedes administrar las VM de Azure sin ninguna dependencia de local mediante la implementación de Windows Admin Center en el VNet donde se conectan las máquinas virtuales de destino. 

Para administrar máquinas virtuales fuera de la VNet en el que se ha implementado la puerta de enlace de Windows Admin Center, debes establecer la conectividad de VNet a VNet entre el VNet de la puerta de enlace de Windows Admin Center y el VNet de los servidores de destino. Puedes establecer esta conectividad con el emparejamiento VNet, VNet a VNet conexión o una conexión de sitio a sitio. [Para obtener más información sobre qué conectividad VNet a VNet opción tiene sentido en su entorno.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center se pueden instalar en una VM existente o recientemente implementada en su entorno. La máquina virtual que se elija para la instalación de Windows Admin Center debe tener un nombre DNS e IP público.

[Más información sobre cómo implementar una puerta de enlace de Windows Admin Center en Azure](deploy-wac-in-azure.md)