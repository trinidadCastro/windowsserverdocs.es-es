---
title: Instalación del servidor de directivas de redes
description: Puede usar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Asistente para agregar roles y características en Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d982ddfe05ec9df3429de4997aaddc4d0b850072
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948301"
---
# <a name="install-network-policy-server"></a>Instalación del servidor de directivas de redes

Puede usar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Asistente para agregar roles y características. NPS es un servicio de rol del rol de servidor Servicios de acceso y directivas de redes.

> [!NOTE]
> De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si el Firewall de Windows con seguridad avanzada está habilitado cuando se instala NPS, las excepciones de Firewall para estos puertos se crean automáticamente durante el proceso de instalación del \( tráfico IPv6 e IPv4 del Protocolo de Internet versión 6 \) . Si los servidores de acceso a la red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que se usan para el tráfico RADIUS.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo **Admins. del dominio**.

## <a name="to-install-nps-by-using-windows-powershell"></a>Para instalar NPS mediante Windows PowerShell

Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione Entrar.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Para instalar NPS mediante Administrador del servidor

1.  En NPS1, en Administrador del servidor, haga clic en **Administrar** y, a continuación, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

4.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haga clic en **Next**.

5.  En **Seleccionar roles de servidor**, en **roles**, seleccione **servicios de acceso y directivas de redes**. Se abre un cuadro de diálogo en el que se pregunta si debe agregar características necesarias para servicios de acceso y directivas de redes. Haga clic en **Agregar características** y, a continuación, en **siguiente** .

6.  En **Seleccionar características**, haga clic en **Siguiente**. En **Servicios de acceso y directivas de redes**, repase la información proporcionada y haga clic en **Siguiente**.

7.  En **Seleccionar servicios de rol**, haga clic en **Servidor de directivas de redes**.  En **¿Desea agregar características requeridas para Servidor de directivas de redes?**, haga clic en **Agregar características**. Haga clic en **Next**.

8.  En **Confirmar selecciones de instalación**, haga clic en **Reiniciar automáticamente el servidor de destino en caso necesario**. Si se le pide confirmar la selección, haga clic en **Sí** y, a continuación, haga clic en **Instalar**. La página Progreso de la instalación muestra el estado durante el proceso de instalación. Una vez completado el proceso, se muestra el mensaje "instalación correcta en *NombreDeEquipo*", donde *NombreDeEquipo* es el nombre del equipo en el que instaló el servidor de directivas de redes. Haga clic en **Cerrar**.

Para obtener más información, vea [administrar NPSs](nps-manage-servers.md).