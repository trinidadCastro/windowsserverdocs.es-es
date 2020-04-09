---
title: Requisitos para implementar la controladora de red
description: Prepare su centro de información para la implementación de la controladora de red, que requiere uno o varios equipos o máquinas virtuales, y un equipo o máquina virtual. Para poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de los archivos de registro (si es necesario) y el registro de DNS dinámico.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: da9164eea4ab7e2fb38864fb69c47252448b77b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854428"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisitos para implementar la controladora de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Prepare su centro de información para la implementación de la controladora de red, que requiere uno o varios equipos o máquinas virtuales, y un equipo o máquina virtual. Para poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de los archivos de registro (si es necesario) y el registro de DNS dinámico.


## <a name="network-controller-requirements"></a>Requisitos de la controladora de red

La implementación de la controladora de red requiere uno o varios equipos o máquinas virtuales que actúan como controladora de red y un equipo o una máquina virtual para servir como cliente de administración para la controladora de red. 

- Todas las máquinas virtuales y los equipos planeados como nodos de controladora de red deben ejecutar Windows Server 2016 Datacenter Edition. 
- Cualquier equipo o máquina virtual (VM) en la que instale la controladora de red debe ejecutar la edición Datacenter de Windows Server 2016. 
- El equipo o la máquina virtual del cliente de administración de la controladora de red deben ejecutar Windows 10. 


## <a name="configuration-requirements"></a>Requisitos de configuración

Antes de implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de los archivos de registro (si es necesario) y el registro de DNS dinámico.

### <a name="step-1-configure-your-security-groups"></a>Paso 1. Configurar los grupos de seguridad

Lo primero que desea hacer es crear dos grupos de seguridad para la autenticación Kerberos. 

Cree grupos para los usuarios que tienen permiso para: 

1. Configurar controladora de red<p>Puede asignar un nombre a este grupo administradores de la controladora de red, por ejemplo. 
2.  Configuración y administración de la red mediante el uso de la controladora de red<p>Puede asignar a este grupo el nombre usuarios de la controladora de red, por ejemplo. Use la transferencia de estado representacional (REST) para configurar y administrar la controladora de red.

>[!NOTE]
>Todos los usuarios que agregue deben ser miembros del grupo usuarios del dominio en Active Directory usuarios y equipos.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Paso 2. Configurar las ubicaciones del archivo de registro si es necesario

Lo siguiente que desea hacer es configurar las ubicaciones de archivo para almacenar los registros de depuración de la controladora de red en el equipo o la máquina virtual de la controladora de red o en un recurso compartido de archivos remoto. 

>[!NOTE]
>Si almacena los registros en un recurso compartido de archivos remoto, asegúrese de que el recurso compartido es accesible desde el controlador de red.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Paso 3. Configurar el registro de DNS dinámico para la controladora de red

Por último, lo siguiente que desea hacer es implementar nodos de clúster de controladora de red en la misma subred o en subredes diferentes. 


|         Si...         |                                                                                                                                                         Entonces...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  En la misma subred,  |                                                                                                                                Debe proporcionar la dirección IP de REST de la controladora de red.                                                                                                                                 |
| En subredes diferentes, | Debe proporcionar el nombre DNS de REST de la controladora de red, que se crea durante el proceso de implementación. También debe hacer lo siguiente:<ul><li>Configure las actualizaciones dinámicas de DNS para el nombre DNS de la controladora de red en el servidor DNS.</li><li>Restrinja las actualizaciones dinámicas de DNS solo a los nodos de la controladora de red.</li></ul> |

---

> [!NOTE]
> El requisito mínimo para realizar estos procedimientos es la pertenencia al grupo **Admins**. del dominio o un grupo equivalente.

1. Permitir actualizaciones dinámicas de DNS para una zona.

   a. Abra el administrador de DNS y, en el árbol de consola, haga clic con el botón secundario en la zona correspondiente y, a continuación, haga clic en **propiedades**. 

   b. En la pestaña **General** , compruebe que el tipo de zona sea **principal** o **integrado en Active Directory**.

   c. En **actualizaciones dinámicas**, compruebe que está seleccionado **solo protección** y, a continuación, haga clic en **Aceptar**.

2. Configurar permisos de seguridad de zona DNS para los nodos de controladora de red

   a.  Haz clic en la pestaña **Seguridad** y, después, en **Avanzadas**. 

   b. En **configuración de seguridad avanzada**, haga clic en **Agregar**. 

   c. Haga clic en **Seleccionar una entidad de seguridad**. 

   d. En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , haga clic en **tipos de objeto**. 

   e. En **tipos de objeto**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.

   f. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo** , escriba el nombre NetBIOS de uno de los nodos de la controladora de red de la implementación y, a continuación, haga clic en **Aceptar**.

   g. En **entrada de permiso**, compruebe los siguientes valores:

      - **Tipo** = permitir
      - **Se aplica a** = este objeto y todos los descendientes

   h. En **permisos**, seleccione **escribir todas las propiedades** y **eliminar**y, a continuación, haga clic en **Aceptar**.

3. Repita el procedimiento para todos los equipos y máquinas virtuales del clúster de controladora de red.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Paso 4. Configurar el nombre de entidad de seguridad de servicio si se usa la autenticación basada en Kerberos

Si la controladora de red usa la autenticación basada en Kerberos para la comunicación con los clientes de administración, debe configurar un nombre de entidad de seguridad de servicio (SPN) para la controladora de red en Active Directory. La controladora de red configura automáticamente el SPN. Lo único que debe hacer es proporcionar permisos para que los equipos de la controladora de red registren y modifiquen el SPN. Para obtener más información, consulte [configurar nombres principales de servicio (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opciones de implementación

### <a name="network-controller-deployment"></a>Implementación de controladora de red

La configuración tiene alta disponibilidad con tres nodos de controlador de red configurados en máquinas virtuales. También se muestra que dos inquilinos con la red virtual del inquilino 2 se dividen en dos subredes virtuales para simular un nivel Web y un nivel de base de datos.  

![Planeación de SDN de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implementación de la controladora de red y del equilibrador de carga de software

Para lograr una alta disponibilidad, hay dos o más nodos SLB/MUX.

![Planeación de SDN de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implementación de controladora de red, Load Balancer de software y puerta de enlace RAS

Hay tres máquinas virtuales de puerta de enlace; dos están activos y uno es redundante.

![Planeación de SDN de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  



Para la automatización de la implementación basada en TP5, Active Directory debe estar disponible y accesible desde estas subredes. Para obtener más información sobre Active Directory, consulte [información general sobre Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  

>[!IMPORTANT] 
>Si implementa con VMM, asegúrese de que las máquinas virtuales de infraestructura (servidor VMM, AD/DNS, SQL Server, etc.) no se hospedan en ninguno de los cuatro hosts mostrados en los diagramas.  


## <a name="next-steps"></a>Pasos siguientes
[Planear una infraestructura de red definida por software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Temas relacionados
- [Controladora de red](../technologies/network-controller/Network-Controller.md) 
- [Alta disponibilidad de la controladora de red](../technologies/network-controller/network-controller-high-availability.md) 
- [Implementación de controladora de red con Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Instalación del rol de servidor de Controladora de red mediante el Administrador del servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
