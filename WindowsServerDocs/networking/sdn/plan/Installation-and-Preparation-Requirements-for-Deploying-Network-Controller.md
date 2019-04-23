---
title: Requisitos para implementar la controladora de red
description: Prepare su centro de datos para la implementación de controladora de red, lo que requiere uno o varios equipos o máquinas virtuales y un equipo o máquina virtual. Antes de poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de archivo de registro (si es necesario) y el registro DNS dinámico.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 9db7609f6f1273c46cba1dd29f81c297bb26f94b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829866"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisitos para implementar la controladora de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Prepare su centro de datos para la implementación de controladora de red, lo que requiere uno o varios equipos o máquinas virtuales y un equipo o máquina virtual. Antes de poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de archivo de registro (si es necesario) y el registro DNS dinámico.


## <a name="network-controller-requirements"></a>Requisitos del controlador de red

Implementación de la controladora de red requiere uno o varios equipos o máquinas virtuales que actúan como la controladora de red y un equipo o máquina virtual para que actúe como un cliente de administración de controladora de red. 

- Todas las máquinas virtuales y equipos planificados como nodos de controladora de red deben ejecutar Windows Server 2016 Datacenter edition. 
- Cualquier equipo o máquina virtual (VM) en el que se instala el controlador de red debe estar ejecutando la edición Datacenter de Windows Server 2016. 
- El equipo cliente de administración o la máquina virtual de controladora de red debe estar ejecutando Windows 10. 

  
## <a name="configuration-requirements"></a>Requisitos de configuración

Antes de implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de archivo de registro (si es necesario) y el registro DNS dinámico.
  
### <a name="step-1-configure-your-security-groups"></a>Paso 1. Configurar los grupos de seguridad
  
Lo primero que desea hacer es crear dos grupos de seguridad para la autenticación Kerberos. 

Crear grupos para los usuarios que tienen permiso para: 

1. Configurar la controladora de red<p>Puede asignar este grupo de administradores de la controladora de red, por ejemplo. 
2.  Configurar y administrar la red mediante el uso de la controladora de red<p>Puede asignar los usuarios este controlador de red de grupo, por ejemplo. Use Representational State Transfer (REST) para configurar y administrar la controladora de red.

>[!NOTE]
>Todos los usuarios que agregue deben ser miembros del grupo usuarios del dominio en los equipos y usuarios de Active Directory.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Paso 2. Configurar ubicaciones de archivos de registro si es necesario

Lo que desea hacer es configurar las ubicaciones de archivo para almacenar los registros de depuración de controladora de red en el equipo de la controladora de red o la máquina virtual o en un recurso compartido de archivos remoto. 

>[!NOTE]
>Si almacena los registros en un recurso compartido remoto, asegúrese de que el recurso compartido es accesible desde la controladora de red.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Paso 3. Configurar el registro DNS dinámico para la controladora de red
  
Por último, lo que desea hacer es implementar nodos de clúster de la controladora de red en la misma subred o en subredes diferentes. 

|Si...  |En ese caso...  |
|---------|---------|
|En la misma subred, |Debe proporcionar la dirección IP de REST de controladora de red. |
|En subredes diferentes, |Debe proporcionar el nombre DNS de REST de controladora de red, que se crea durante el proceso de implementación. También debe hacer lo siguiente:<ul><li>Configurar las actualizaciones dinámicas de DNS para el nombre DNS del controlador de red en el servidor DNS.</li><li>Restringir las actualizaciones dinámicas de DNS a solo los nodos de controladora de red.</li></ul> |
---

> [!NOTE]
> Pertenencia a **Admins. del dominio**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.
  
1. Permitir actualizaciones dinámicas de DNS para una zona.

   a. Abra el Administrador de DNS y en el árbol de consola, haga clic en la zona correspondiente y, a continuación, haga clic en **propiedades**. 
      
   b. En el **General** , compruebe que el tipo de zona es **principal** o **integrado en Active Directory**.

   c. En **actualizaciones dinámicas**, compruebe que **solo Secure** está seleccionada y, a continuación, haga clic en **Aceptar**.

2. Configurar permisos de seguridad de la zona de DNS para los nodos de controladora de red

   a.  Haga clic en la pestaña **Seguridad** y, a continuación, en **Opciones avanzadas**. 

   b. En **configuración de seguridad avanzada**, haga clic en **agregar**. 
  
   c. Haz clic en **Seleccionar una entidad de seguridad**. 

   d. En el **Seleccionar usuario, equipo, cuenta de servicio o grupo** cuadro de diálogo, haga clic en **tipos de objeto**. 

   e. En **tipos de objeto**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.

   f. En el **Seleccionar usuario, equipo, cuenta de servicio o grupo** cuadro de diálogo, escriba el nombre de NetBIOS de uno de los nodos de controladora de red en la implementación y, a continuación, haga clic en **Aceptar**.

   g. En **entrada de permiso**, compruebe los siguientes valores:

      - **Tipo** = permitir
      - **Se aplica a** = este objeto y todos los descendientes
  
   h. En **permisos**, seleccione **escribir todas las propiedades** y **eliminar**y, a continuación, haga clic en **Aceptar**.

3. Repita para todos los equipos y máquinas virtuales en el clúster de controladora de red.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Paso 4. Configurar el nombre de entidad de servicio si la autenticación mediante Kerberos basada en

Si la controladora de red usa autenticación basada en Kerberos para la comunicación con clientes de administración, debe configurar un nombre Principal de servicio (SPN) para la controladora de red en Active Directory. La controladora de red se configura automáticamente el SPN. Todo lo que necesita hacer es proporcionar permisos para las máquinas de controladora de red registrar y modificar el SPN. Para obtener más información, consulte [configurar entidad de seguridad nombres servicio (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opciones de implementación

### <a name="network-controller-deployment"></a>Implementación de la controladora de red

El programa de instalación es de alta disponibilidad con tres nodos de controladora de red configurados en máquinas virtuales. También se muestra es dos inquilinos con red virtual del inquilino 2 dividida en dos subredes virtuales para simular un nivel web y un nivel de base de datos.  

![Planeación de controladora de red de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implementación de equilibrador de carga de software y la controladora de red

Para alta disponibilidad, hay dos o más nodos SLB/MUX.
   
![Planeación de controladora de red de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implementación de controladora de red, equilibrador de carga de Software y puerta de enlace RAS

Hay tres máquinas virtuales de puerta de enlace; dos están activas y uno es redundante.

![Planeación de controladora de red de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Para la automatización de la implementación basada en TP5, Active Directory debe estar disponible y accesible desde estas subredes. Para obtener más información acerca de Active Directory, consulte [Introducción a servicios de dominio de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  
  
>[!IMPORTANT] 
>Si implementa con VMM, asegúrese de las máquinas virtuales de infraestructura (servidor VMM, AD/DNS, SQL Server, etc.) no están hospedados en cualquiera de los cuatro hosts que se muestra en los diagramas.  


## <a name="next-steps"></a>Pasos siguientes
[Plan de infraestructura de red definidas por un Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Temas relacionados
- [Controladora de red](../technologies/network-controller/Network-Controller.md) 
- [Alta disponibilidad del controlador de red](../technologies/network-controller/network-controller-high-availability.md) 
- [Implementar la controladora de red mediante Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Instalar el rol de servidor de controladora de red con el administrador del servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
