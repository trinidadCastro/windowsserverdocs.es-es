---
title: Instalar el Servidor de directivas de redes
description: Puede usar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o el agregar Roles y características de asistente en Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831196"
---
# <a name="install-network-policy-server"></a>Instalar el Servidor de directivas de redes

Puede utilizar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o el agregar Roles y características de asistente. NPS es un servicio de rol del rol de servidor Servicios de acceso y directivas de redes.

> [!NOTE]
> De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si está habilitado Firewall de Windows con seguridad avanzada al instalar NPS, excepciones de firewall para estos puertos se crean automáticamente durante el proceso de instalación para el protocolo de Internet versión 6 \(IPv6\) y tráfico IPv4. Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que usa para Tráfico RADIUS.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo **Admins. del dominio**.

## <a name="to-install-nps-by-using-windows-powershell"></a>Para instalar NPS mediante Windows PowerShell

Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione ENTRAR.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Para instalar NPS mediante el administrador del servidor

1.  En NPS1, en Administrador del servidor, haga clic en **Administrar** y, a continuación, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

4.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, seleccione **servicios de acceso y directivas de redes**. Se abre un cuadro de diálogo que pregunta si deben agregar las características necesarias para servicios de acceso y directivas de redes. Haga clic en **agregar características**y, a continuación, haga clic en **siguiente**

6.  En **Seleccionar características**, haga clic en **Siguiente**. En **Servicios de acceso y directivas de redes**, repase la información proporcionada y haga clic en **Siguiente**.

7.  En **Seleccionar servicios de rol**, haga clic en **Servidor de directivas de redes**.  En **¿Desea agregar características requeridas para Servidor de directivas de redes?**, haga clic en **Agregar características**. Haz clic en **Siguiente**.

8.  En **Confirmar selecciones de instalación**, haga clic en **Reiniciar automáticamente el servidor de destino en caso necesario**. Si se le pide confirmar la selección, haga clic en **Sí** y, a continuación, haga clic en **Instalar**. La página Progreso de la instalación muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "instalación correcta en *ComputerName*" se muestra, donde *ComputerName* es el nombre del equipo en el que instaló el servidor de directivas de red. Haga clic en **Cerrar**.

Para obtener más información, consulte [NPSs administrar](nps-manage-servers.md).