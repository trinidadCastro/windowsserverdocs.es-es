---
title: Instalar al servidor de directivas de red
description: Puedes usar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o la agregar Roles y características de asistente en Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>Instalar al servidor de directivas de red

Puedes usar este tema para instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o la agregar Roles y características de asistente. NPS es un servicio de rol del rol de servidor de servicios de acceso y directivas de redes.

> [!NOTE]
> De forma predeterminada, NPS escucha para el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si Firewall de Windows con seguridad avanzada está habilitado cuando instalas NPS, las excepciones del firewall para estos puertos se crean automáticamente durante el proceso de instalación de protocolo de Internet versión 6 \(IPv6\) y tráfico IPv4. Si se configuran los servidores de acceso de red para enviar el tráfico RADIUS sobre puertos que no sean de estos valores predeterminados, quite las excepciones que se crea en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y crear excepciones para los puertos que usas para el tráfico RADIUS.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro de la **administradores de dominio** grupo.

## <a name="to-install-nps-by-using-windows-powershell"></a>Para instalar NPS mediante Windows PowerShell

Para realizar este procedimiento mediante Windows PowerShell, ejecutar Windows PowerShell como administrador, escribe el siguiente comando y, a continuación, presione ENTRAR.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Para instalar NPS mediante el administrador del servidor

1.  En NPS1, en el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.

2.  En **antes de comenzar**, haz clic en **siguiente**.

    > [!NOTE]
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.

3.  En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.

4.  En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, selecciona **servicios de acceso y directivas de redes**. Abre un cuadro de cuadro de diálogo que pregunta si deben agregar características que son necesarias para los servicios de acceso y directivas de redes. Haz clic en **agregar características**y, a continuación, haz clic en **siguiente**

6.  En **Select features**, haz clic en **siguiente**y en **servicios de acceso y directivas de redes**, revisa la información que se proporciona y, a continuación, haz clic en **siguiente**.

7.  En **seleccione los servicios de rol**, haz clic en **el servidor de directivas de red**.  En **agregar características que son necesarias para el servidor de directivas de red**, haz clic en **agregar características**. Haz clic en **siguiente**.

8.  En **Confirmar selecciones de instalación**, haz clic en **reiniciar automáticamente el servidor de destino en caso necesario**. Cuando aparezca el mensaje para confirmar esta selección, haz clic en **Sí**y, a continuación, haz clic en **instalar**. La página de progreso de la instalación muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "se completó la instalación en *ComputerName*" se muestra, donde *ComputerName* es el nombre del equipo en el que instalaste el servidor de directivas de red. Haz clic en **cerrar **.

Para obtener más información, consulta [administrar NPS servidores](nps-manage-servers.md).