---
title: Instalar la entidad de certificación
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 84e4b2fe0b59820b9e51229335f3539bcbeeec90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860746"
---
# <a name="install-the-certification-authority"></a>Instalar la entidad de certificación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar servicios de certificados de Active Directory (AD CS) para que puedan inscribir un certificado de servidor para los servidores que ejecutan el servidor de directivas de redes (NPS), enrutamiento y acceso remoto (RRAS) o ambos.  
  
> [!IMPORTANT]  
> -   Antes de instalar servicios de certificados de Active Directory, debe el nombre del equipo, configure el equipo con una dirección IP estática y unir el equipo al dominio. Para obtener más información sobre cómo realizar estas tareas, vea Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Para llevar a cabo este procedimiento, el equipo en el que va a instalar AD CS debe estar unido a un dominio donde está instalado servicios de dominio de Active Directory (AD DS).  
  
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.  
  
> [!NOTE]  
> Para llevar a cabo este procedimiento con Windows PowerShell, abra Windows PowerShell y escriba el siguiente comando y, a continuación, presione ENTRAR.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Después de instalar AD CS, escriba el siguiente comando y presione ENTRAR.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Para instalar los Servicios de certificados de Active Directory  

>[!TIP]
>Si desea usar Windows PowerShell para instalar servicios de certificados de Active Directory, consulte [Install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) cmdlets y parámetros opcionales.
  
1.  Inicie sesión como miembro del grupo Administradores de empresas y del grupo Admins. del dominio del dominio raíz.  
  
2.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.  
  
3.  En **Antes de comenzar**, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.  
  
4.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.  
  
5.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.  
  
6.  En **seleccionar Roles de servidor**, en **Roles**, seleccione **Active Directory Certificate Services**. Cuando se le pida que agregue las características necesarias, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.  
  
7.  En **seleccionar características**, haga clic en **siguiente**.  
  
8.  En **Active Directory Certificate Services**, lea la información proporcionada y, a continuación, haga clic en **siguiente**.  
  
9. En **Confirmar selecciones de instalación**, haga clic en **Instalar**. No cierre al asistente durante el proceso de instalación. Cuando se complete la instalación, haga clic en **configurar Active Directory Certificate Services en el servidor de destino**. Abre el Asistente de configuración de AD CS. Lea la información de credenciales y, si es necesario, proporcione las credenciales de una cuenta que sea miembro del grupo Administradores de empresa. Haz clic en **Siguiente**.  
  
10. En **servicios de rol**, haga clic en **entidad de certificación**y, a continuación, haga clic en **siguiente**.  
  
11. En el **el tipo de instalación** , comprueba que **CA empresarial** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
12. En el **especificar el tipo de la entidad de certificación** , comprueba que **CA raíz** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
13. En el **especificar el tipo de la clave privada** , comprueba que **crear una nueva clave privada** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
14. En el **criptografía para CA** página, mantenga la configuración predeterminada para CSP (**RSA #Microsoft Software Key Storage Provider**) y el algoritmo hash (**SHA2**) y determinar la mejor longitud de caracteres de la clave para la implementación. Clave extensa ofrece una seguridad óptima; Sin embargo, se puede afectar al rendimiento del servidor y no pueden ser compatibles con las aplicaciones heredadas. Se recomienda que mantenga la configuración predeterminada de 2048. Haz clic en **Siguiente**.  
  
15. En el **nombre de CA** página, mantenga el nombre común sugerido para la CA o cambie el nombre según sus requisitos. Asegúrese de que está seguro de que el nombre de entidad emisora de certificados es compatible con sus fines y las convenciones de nomenclatura porque no se puede cambiar el nombre de entidad emisora de certificados después de haber instalado AD CS. Haz clic en **Siguiente**.  
  
16. En el **período de validez** página **especificar el período de validez**, escriba el número y seleccione un valor de tiempo (años, meses, semanas o días). Se recomienda el valor predeterminado de cinco años. Haz clic en **Siguiente**.  
  
17. En el **base de datos de CA** página **especifique las ubicaciones de la base de datos**, especifique la ubicación de carpeta para la base de datos de certificado y el registro de base de datos de certificado. Si especifica ubicaciones distintas de las ubicaciones predeterminadas, asegúrese de que las carpetas estén protegidas mediante listas de control de acceso (ACL) que impidan que usuarios o equipos no autorizados tengan acceso a los archivos de registro y la base de datos de la entidad de certificación. Haz clic en **Siguiente**.  
  
18. En **confirmación**, haga clic en **configurar** para aplicar sus selecciones y, a continuación, haga clic en **cerrar**.  
  


