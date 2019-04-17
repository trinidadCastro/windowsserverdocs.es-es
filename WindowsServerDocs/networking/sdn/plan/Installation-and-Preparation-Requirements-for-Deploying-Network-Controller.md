---
title: Instalación y requisitos de preparación para implementar el controlador de red
description: Puedes usar este tema para preparar su centro de datos para la implementación de controlador de red.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>Instalación y requisitos de preparación para implementar el controlador de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para preparar su centro de datos para la implementación de controlador de red.  
  
> [!NOTE]  
> Además de este tema, la siguiente documentación de controlador de red está disponible.  
> 
> - [Controlador de red](../technologies/network-controller/Network-Controller.md)
> - [Alta disponibilidad del controlador de red](../technologies/network-controller/network-controller-high-availability.md)
> - [Implementar el controlador de red mediante Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar el rol de servidor de controlador de red mediante el administrador del servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

Siguiente es los pasos de instalación, el software y otros requisitos y preparación que debe seguir antes de implementar el controlador de red.

## <a name="installation-requirements"></a>Requisitos de instalación

Siguiente es los requisitos de instalación de controlador de red.

- Para las implementaciones de Windows Server 2016, puedes implementar el controlador de red en uno o más equipos, uno o más de máquinas virtuales o una combinación de equipos y máquinas virtuales. Todas las máquinas virtuales y equipos planificados como nodos de controlador de red deben ejecutar Windows Server 2016 Datacenter edition.

## <a name="software-requirements"></a>Requisitos de software

Implementación de controladores de red requiere uno o más equipos o máquinas virtuales que actúan como el controlador de red y un equipo o máquina virtual para que actúe como un cliente de administración para el controlador de red. Estos equipos o máquinas virtuales deben ejecutar los siguientes sistemas operativos.  

- Cualquier equipo o máquina virtual (VM) en el que se instala el controlador de red debe estar ejecutando Datacenter edition de Windows Server 2016.  
  
- El equipo de cliente de administración o máquina virtual para el controlador de red debe estar ejecutándose Windows 8, Windows 8.1 o Windows 10.  
  
## <a name="additional-requirements"></a>Requisitos adicionales

A continuación se muestran pasos adicionales que debe seguir antes de implementar el controlador de red.
  
### <a name="configure-security-groups"></a>Configurar grupos de seguridad
  
Si los equipos o máquinas virtuales de controlador de red y el cliente de administración están unidos a un dominio, configurar los siguientes grupos de seguridad para la autenticación de Kerberos.

- Crea un grupo de seguridad y agrega todos los usuarios que tienen permiso para configurar el controlador de red. Por ejemplo, crear un grupo llamado **administradores de controlador de red **. Todos los usuarios que agregues a este grupo también deben ser miembros de la **los usuarios del dominio** grupo de equipos y usuarios de Active Directory.  
  
    > [!NOTE]  
    > Para obtener más información sobre cómo crear un grupo de equipos y usuarios de Active Directory, consulte [crear un nuevo grupo ](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx).  

- Crea un grupo de seguridad y agrega todos los usuarios que tienen permiso para configurar y administrar la red mediante el controlador de red.  Por ejemplo, crear un nuevo grupo denominado **los usuarios de controlador de red **. Todos los usuarios que agregas al nuevo grupo también deben ser miembros de la **los usuarios del dominio** grupo de equipos y usuarios de Active Directory. Configuración del controlador de red y la gestión se realiza mediante la transferencia de estado representacional \(REST\).

### <a name="configure-log-file-locations-if-needed"></a>Configurar las ubicaciones de archivo de registro si es necesario

Puedes almacenar los registros de depuración de controlador de red en el equipo de controlador de red o máquina virtual o en un recurso compartido de archivos. Si quieres almacenar los registros en un recurso compartido de archivos, asegúrate de que el recurso compartido es accesible desde el controlador de red.

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>Configurar el registro DNS dinámico para el controlador de red
  
Puedes implementar nodos del clúster de controlador de red en la misma subred o en subredes diferentes. 

>[!NOTE]
>Si los nodos de controlador de red se encuentran en la misma subred, debes proporcionar la dirección IP de REST de controlador de red al configurar el registro DNS dinámico para el controlador de red. Si los nodos que se encuentran en diferentes subredes, debes proporcionar el nombre DNS de REST de controlador de red cuando se configura el registro DNS dinámico.

Si los nodos de controlador de red se encuentran en diferentes subredes, debes realizar la siguiente configuración de DNS adicional:

- Crear un nombre DNS para el controlador de red durante el proceso de implementación

- Configurar las actualizaciones dinámicas de DNS para el nombre DNS de controlador de red en el servidor DNS

- Restringir las actualizaciones dinámicas de DNS a solo los nodos de controlador de red

Puedes usar los siguientes procedimientos para configurar las actualizaciones dinámicas y para restringir la actualización dinámica de registro de nombres de controlador de red.

> [!NOTE]
> Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>Para permitir que las actualizaciones dinámicas de DNS para una zona

1. Abre el Administrador de DNS.

2. En el árbol de consola, haz clic en la zona correspondiente y, a continuación, haz clic en **propiedades **. La zona **propiedades** abre el cuadro de diálogo.

3. En la **General**, compruebe que el tipo de zona es **principal** o **integrado en Active Directory**.

4. En **actualizaciones dinámicas**, comprueba que **seguro solo** está seleccionado. Si no está seleccionada, cambia el valor de **actualizaciones dinámicas** a **seguro solo**y, a continuación, haz clic en **Aceptar**.

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>Para configurar los permisos de seguridad de zona DNS para los nodos de controlador de red

1.  Abre el Administrador de DNS.

2.  En el árbol de consola, haz clic en la zona correspondiente y, a continuación, haz clic en **propiedades **. La zona **propiedades** abre el cuadro de diálogo.

3.  Haz clic en el **seguridad** pestaña y, a continuación, haz clic en **avanzadas**. La **configuración de seguridad avanzada** abre el cuadro de diálogo.

4. En **configuración de seguridad avanzada**, haz clic en **agregar**. La **entrada de permiso** abre el cuadro de diálogo.
  
5. Haz clic en **seleccionar una entidad de seguridad**. La **Seleccionar usuarios, equipo, cuenta de servicio o grupo** abre el cuadro de diálogo.

6. En la **Seleccionar usuarios, equipo, cuenta de servicio o grupo** cuadro de diálogo, haz clic en **tipos de objeto**. La **tipos de objeto** abre el cuadro de diálogo. 

7. En **tipos de objeto**, selecciona **equipos**y, a continuación, haz clic en **Aceptar**.

8. En la **Seleccionar usuarios, equipo, cuenta de servicio o grupo** cuadro de diálogo, escribe el nombre NetBIOS de uno de los nodos de controlador de red en la implementación y, a continuación, haz clic en **Aceptar**.

9. En **entrada de permiso**, asegúrate de que el valor de **tipo** es **permitir**y el valor de **se aplica a** es **este objeto y todos los descendientes**.
  
10. En permisos, seleccione **escribir todas las propiedades** y **eliminar**y, a continuación, haz clic en **Aceptar**.

11. Repite los pasos **5** a través de **10** para todos los equipos y máquinas virtuales en el clúster de controlador de red.

Para obtener más información, consulta [planear una infraestructura de red definido de Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).
