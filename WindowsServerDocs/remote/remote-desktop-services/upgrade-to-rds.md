---
title: Actualización de las implementaciones de Servicios de Escritorio remoto en Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de Servicios de Escritorio remoto a Windows Server 2016.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 29648db89b61a9d22aad6d5aa814cfe7f425a970
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370696"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>Actualización de las implementaciones de Servicios de Escritorio remoto en Windows Server 2016

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Actualizaciones admitidas del sistema operativo con el rol de RDS instalado
Las actualizaciones a Windows Server 2016 solo se admiten desde Windows Server 2012 R2 y Windows Server 2016.

## <a name="flow-for-deployment-upgrades"></a>Flujo de las actualizaciones de implementación
Con el fin de minimizar el tiempo de inactividad, es mejor seguir los pasos descritos a continuación:

1. Se deberían actualizar en primer lugar los **servidores de Agente de conexión a Escritorio remoto**. Si hay una configuración activa/activa en la implementación, quita todos los servidores excepto uno de la implementación y realiza una actualización local. Realiza actualizaciones de los demás servidores de Agente de conexión a Escritorio remoto sin conexión y, después, vuelve a agregarlos a la implementación. La implementación no estará disponible durante la actualización de los servidores de Agente de conexión a Escritorio remoto.

   > [!NOTE] 
   > Es obligatorio actualizar los servidores de Agente de conexión a Escritorio remoto. No se admiten servidores de Agente de conexión a Escritorio remoto de Windows Server 2012 R2 en una implementación mixta con servidores de Windows Server 2016. Una vez que los servidores de Agente de conexión a Escritorio remoto ejecuten Windows Server 2016 la implementación será funcional, incluso aunque el resto de los servidores de la implementación aún estén ejecutando Windows Server 2012 R2.

2. Se deben actualizar los **servidores de licencias de Escritorio remoto** antes de actualizar los servidores host de sesión de Escritorio remoto.
   > [!NOTE] 
   > Los servidores de licencias de Escritorio remoto de Windows Server 2012 y 2012 R2 funcionan con las implementaciones de Windows Server 2016, pero solo pueden procesar las licencias de acceso cliente de Windows Server 2012 R2 y versiones anteriores. No pueden usar licencias de acceso cliente de Windows Server 2016. Consulta [Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)](rds-client-access-license.md) para más información sobre los servidores de licencias de Escritorio remoto.

3. A continuación, se pueden actualizar los **servidores host de sesión de Escritorio remoto**. Para evitar el tiempo de inactividad durante la actualización, el administrador puede dividir los servidores que se van a actualizar en 2 pasos como se indica a continuación. Todos volverán a funcionar después de la actualización. Para actualizar, usa los pasos descritos en [Actualización de los servidores host de sesión de Escritorio remoto a Windows Server 2016](upgrade-to-rdsh.md).

4. A continuación, se pueden actualizar los **servidores host de virtualización de Escritorio remoto**. Para actualizar, usa los pasos descritos en [Actualización de los servidores host de virtualización de Escritorio remoto a Windows Server 2016](upgrade-to-rdvh.md).

5. Los **servidores de acceso web de Escritorio remoto** se pueden actualizar en cualquier momento.
   > [!NOTE]
   > La actualización del acceso web de Escritorio remoto puede restablecer las propiedades de IIS (por ejemplo, los archivos de configuración). Para no perder los cambios, toma notas o copias de las personalizaciones realizadas en el sitio web de Escritorio remoto en IIS.

   > [!NOTE] 
   > Los servidores de acceso web de Escritorio remoto de Windows Server 2012 y 2012 R2 funcionarán con las implementaciones de Windows Server 2016.

6. Los **servidores de puerta de enlace de Escritorio remoto** se pueden actualizar en cualquier momento.
   > [!NOTE]
   > Windows Server 2016 no incluye directivas de protección de acceso a redes (NAP), por lo que estas se tendrán que eliminar. La manera más fácil de eliminar las directivas correctas es mediante la ejecución del asistente para actualización. Si hay directivas NAP que debes eliminar, la actualización bloqueará y creará un archivo de texto en el escritorio que incluye las directivas específicas. Para administrar las directivas NAP, abre la herramienta del servidor de directivas de red. Después de eliminarlas, haz clic en **Actualizar** en la herramienta de configuración para continuar con el proceso de actualización. 

   > [!NOTE] 
   > Los servidores de puerta de enlace de Escritorio remoto de Windows Server 2012 y 2012 R2 funcionarán con las implementaciones de Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Implementación de VDI: Actualización de los sistemas operativos invitados compatibles
Los administradores tendrán las siguientes opciones para la actualización de las colecciones de máquinas virtuales:

### <a name="upgrade-managed-shared-vm-collections"></a>Actualización de colecciones de máquinas virtuales administradas y compartidas 
Los administradores deberán crear plantillas de máquinas virtuales con la versión de sistema operativo deseada y usarlas para revisar todas las máquinas virtuales del grupo. 

Se admiten los siguientes escenarios de aplicación de revisiones:
- Windows 7 SP1 se puede revisar a Windows 8 o Windows 8.1
- Windows 8 se puede revisar a Windows 8.1
- Windows 8.1 se puede revisar a Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Actualización de colecciones de máquinas virtuales compartidas y no administradas 
Los usuarios finales no pueden actualizar sus escritorios personales. Los administradores deben realizar la actualización. Los pasos exactos aún se deben determinar.