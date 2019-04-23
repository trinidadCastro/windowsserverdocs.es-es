---
title: Actualizar las implementaciones de servicios de escritorio remoto a Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de servicios de escritorio remoto a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
notes: https://social.technet.microsoft.com/wiki/contents/articles/22069.remote-desktop-services-upgrade-guidelines-for-windows-server-2012-r2.aspx
ms.openlocfilehash: f683a7d9346494e7f1fb6faf716ca9c90cfef8d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875756"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>Actualizar las implementaciones de servicios de escritorio remoto a Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Admite actualizaciones del sistema operativo con instalado el rol RDS
Se admiten actualizaciones a Windows Server 2016 solo desde Windows Server 2012 R2 y Windows Server 2016.

## <a name="flow-for-deployment-upgrades"></a>Flujo para las actualizaciones de implementación
Con el fin de mantener el tiempo de inactividad al mínimo, es mejor seguir los pasos siguientes:

1. **Servidores de agente de conexión a Escritorio remoto** debe ser el primero que debe actualizarse. Si no hay configuración activo/activo en la implementación, quite todos menos un servidor de la implementación y realizar una actualización en contexto. Realizar actualizaciones en los demás servidores de agente de conexión a Escritorio remoto sin conexión y, a continuación, volver a agregarlos a la implementación. La implementación no estará disponible durante la actualización de servidores de agente de conexión a Escritorio remoto.

   > [!NOTE] 
   > Es obligatorio para actualizar los servidores de agente de conexión a Escritorio remoto. No se admiten servidores de agente de conexión a Escritorio remoto de Windows Server 2012 R2 en una implementación mixta con servidores de Windows Server 2016. Una vez que los servidores de agente de conexión a Escritorio remoto se ejecuta Windows Server 2016 la implementación será funcional, aunque el resto de los servidores de la implementación aún se estén ejecutando Windows Server 2012 R2.

2. **Los servidores de licencias de escritorio remoto** debe actualizarse antes de actualizar los servidores Host de sesión de escritorio remoto.
   > [!NOTE] 
   > Servidores de licencias de Windows Server 2012 y 2012 R2 RD funcionará con las implementaciones de Windows Server 2016, pero solo pueden procesar las CAL de Windows Server 2012 R2 y anteriores. No se utilizan CAL de Windows Server 2016. Consulte [licencia de su implementación de RDS con licencias de acceso de cliente (CAL)](rds-client-access-license.md) para obtener más información acerca de los servidores de licencias de escritorio remoto.

3. **Servidores Host de sesión de escritorio remoto** pueden actualizarse a continuación. Para evitar tiempo de inactividad durante la actualización, el administrador puede dividir los servidores que debe actualizarse en 2 pasos tal como se detalla a continuación. Todo será funcional después de la actualización. Para actualizar, use los pasos descritos en [servidores de actualización de Host de sesión de escritorio remoto a Windows Server 2016](upgrade-to-rdsh.md).

4. **Servidores Host de virtualización de escritorio remoto** pueden actualizarse a continuación. Para actualizar, use los pasos descritos en [servidores de actualización de Host de virtualización de escritorio remoto a Windows Server 2016](upgrade-to-rdvh.md).

5. **Servidores de acceso Web de RD** se pueden actualizar en cualquier momento.
   > [!NOTE]
   > Actualización Web de escritorio remoto, puede restablecer las propiedades IIS (por ejemplo, los archivos de configuración). Para no perder los cambios, realizar notas o copias de las personalizaciones realizadas en el sitio Web de escritorio remoto en IIS.

   > [!NOTE] 
   > Windows Server 2012 y servidores de acceso Web de RD R2 2012 funcionará con las implementaciones de Windows Server 2016.

6. **Servidores de puerta de enlace de escritorio remoto** se pueden actualizar en cualquier momento.
   > [!NOTE]
   > Windows Server 2016 no incluye las directivas de protección de acceso a redes (NAP), tendrá que quitarse. La manera más fácil para quitar las directivas correctas se está ejecutando el Asistente para actualización. Si hay que debe eliminar las directivas de NAP, la actualización se bloquea y cree un archivo de texto en el escritorio que incluye las directivas específicas. Para administrar las directivas de NAP, abra la herramienta de servidor de directivas de red. Después de eliminarlos, haga clic en **actualizar** en la herramienta de configuración para continuar con el proceso de actualización. 

   > [!NOTE] 
   > Windows Server 2012 y servidores de puerta de enlace de escritorio remoto de R2 2012 funcionará con las implementaciones de Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Implementación de VDI – actualización del SO de invitado admitidos
Los administradores tendrán las siguientes opciones para la actualización de las colecciones de la máquina virtual:

### <a name="upgrade-managed-shared-vm-collections"></a>Actualizar máquina virtual administrada de compartir colecciones 
Los administradores deberán crear plantillas de máquina virtual con la versión de sistema operativo deseada y usarlo para revisar todas las máquinas virtuales en el grupo. 

Se admiten los siguientes escenarios de aplicación de revisiones:
- Se pueden aplicar parches a Windows 7 SP1 para Windows 8 o Windows 8.1
- Se pueden aplicar parches a Windows 8 a Windows 8.1
- Windows 8.1 se pueden aplicar parches a Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Actualizar las colecciones de máquina virtual compartidas no administradas 
Los usuarios finales no se puede actualizar sus escritorios personales. Los administradores deben realizar la actualización. Los pasos exactos son todavía se puede determinar.