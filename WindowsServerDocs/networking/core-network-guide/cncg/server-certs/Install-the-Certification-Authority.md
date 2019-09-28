---
title: Instalar la entidad de certificación
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 024fc73c4ed089d81808cf44d7cfe8b01bfffaa0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406325"
---
# <a name="install-the-certification-authority"></a>Instalar la entidad de certificación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar Active Directory servicios de Certificate Server (AD CS) para poder inscribir un certificado de servidor en servidores que ejecutan el servidor de directivas de redes (NPS), el servicio de enrutamiento y acceso remoto (RRAS), o ambos.  
  
> [!IMPORTANT]  
> -   Antes de instalar Active Directory servicios de Certificate Server, debe asignar un nombre al equipo, configurar el equipo con una dirección IP estática y unir el equipo al dominio. Para obtener más información sobre cómo realizar estas tareas, vea la [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)de Windows Server 2016.  
> -   Para llevar a cabo este procedimiento, el equipo en el que va a instalar AD CS debe estar unido a un dominio en el que esté instalado Active Directory Domain Services (AD DS).  
  
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.  
  
> [!NOTE]  
> Para llevar a cabo este procedimiento con Windows PowerShell, abra Windows PowerShell y escriba el siguiente comando y, a continuación, presione Entrar.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Una vez instalado AD CS, escriba el siguiente comando y presione Entrar.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Para instalar los Servicios de certificados de Active Directory  

> [!TIP]
> Si desea usar Windows PowerShell para instalar Active Directory servicios de Certificate Server, consulte [install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) para los cmdlets y parámetros opcionales.
  
1.  Inicie sesión como miembro del grupo Administradores de empresas y del grupo Admins. del dominio del dominio raíz.  
  
2.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.  
  
3.  En **Antes de comenzar**, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.  
  
4.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.  
  
5.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.  
  
6.  En **Seleccionar roles de servidor**, en **roles**, seleccione **Active Directory servicios de Certificate**Server. Cuando se le pida que agregue las características necesarias, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.  
  
7.  En **seleccionar características**, haga clic en **siguiente**.  
  
8.  En **Active Directory servicios de Certificate Server**, lea la información proporcionada y, a continuación, haga clic en **siguiente**.  
  
9. En **Confirmar selecciones de instalación**, haga clic en **Instalar**. No cierre el asistente durante el proceso de instalación. Una vez finalizada la instalación, haga clic en **configurar servicios de Certificate server Active Directory en el servidor de destino**. Se abre el Asistente para configuración de AD CS. Lea la información de las credenciales y, si es necesario, proporcione las credenciales de una cuenta que sea miembro del grupo administradores de empresas. Haz clic en **Siguiente**.  
  
10. En **servicios de rol**, haga clic en **entidad de certificación**y, a continuación, en **siguiente**.  
  
11. En la página **tipo de instalación** , compruebe que la opción **CA empresarial** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
12. En la página **especificar el tipo de la entidad de certificación** , compruebe que la opción **CA raíz** esté seleccionada y, a continuación, haga clic en **siguiente**.  
  
13. En la página **especificar el tipo de la clave privada** , compruebe que la opción **crear una nueva clave privada** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
14. En la página **Criptografía para CA** , mantenga la configuración predeterminada de CSP (**RSA # proveedor de almacenamiento de claves de software de Microsoft**) y el algoritmo hash (**sha2**) y determine la mejor longitud de caracteres de clave para la implementación. Las longitudes de caracteres de clave grande proporcionan una seguridad óptima. sin embargo, pueden afectar al rendimiento del servidor y puede que no sean compatibles con las aplicaciones heredadas. Se recomienda que mantenga el valor predeterminado de 2048. Haz clic en **Siguiente**.  
  
15. En la página **nombre de CA** , conserve el nombre común sugerido para la CA o cambie el nombre de acuerdo con sus requisitos. Asegúrese de que el nombre de la entidad de certificación es compatible con las convenciones de nomenclatura y los propósitos, ya que no se puede cambiar el nombre de la entidad de certificación después de haber instalado AD CS. Haz clic en **Siguiente**.  
  
16. En la página **período de validez** , en **especifique el período de validez**, escriba el número y seleccione un valor de tiempo (años, meses, semanas o días). Se recomienda el valor predeterminado de cinco años. Haz clic en **Siguiente**.  
  
17. En la página **base de datos de CA** , en **Especifique las ubicaciones de base de datos**, especifique la ubicación de la carpeta para la base de datos de certificados y el registro de base de datos de certificados. Si especifica ubicaciones distintas de las ubicaciones predeterminadas, asegúrese de que las carpetas estén protegidas mediante listas de control de acceso (ACL) que impidan que usuarios o equipos no autorizados tengan acceso a los archivos de registro y la base de datos de la entidad de certificación. Haz clic en **Siguiente**.  
  
18. En **confirmación**, haga clic en **configurar** para aplicar las selecciones y, a continuación, haga clic en **cerrar**.  
  


