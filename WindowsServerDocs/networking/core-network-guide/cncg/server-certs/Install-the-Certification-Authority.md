---
title: Instalar la entidad de certificación
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>Instalar la entidad de certificación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para instalar servicios de certificados de Active Directory (AD CS) para que se puede inscribir un certificado de servidor a servidores que ejecutan el servidor de directivas de redes (NPS), enrutamiento y acceso remoto (RRAS) o ambos.  
  
> [!IMPORTANT]  
> -   Antes de instalar servicios de certificados de Active Directory, debes nombre al equipo, configurar el equipo con una dirección IP estática y unir el equipo al dominio. Para obtener más información sobre cómo realizar estas tareas, consulta el Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Para realizar este procedimiento, el equipo en el que vas a instalar AD CS debe estar unido a un dominio donde está instalado servicios de dominio de Active Directory (AD DS).  
  
Pertenencia a ambos **administradores** y el dominio raíz **administradores de dominio** grupo es lo mínimo necesario para completar este procedimiento.  
  
> [!NOTE]  
> Para realizar este procedimiento mediante Windows PowerShell, abre Windows PowerShell y escribe el siguiente comando y, a continuación, presione ENTRAR.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Después de instalar AD CS, escribe el siguiente comando y presione ENTRAR.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Para instalar servicios de certificados de Active Directory  
  
1.  Inicia sesión como miembro del grupo Administradores de empresa y grupo de administradores de dominio del dominio raíz.  
  
2.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.  
  
3.  En **antes de comenzar**, haz clic en **siguiente**.  
  
    > [!NOTE]  
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.  
  
4.  En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
5.  En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.  
  
6.  En **seleccionar Roles de servidor**, en **Roles**, selecciona **servicios de certificados de Active Directory**. Cuando se te pida para agregar características necesarias, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
7.  En **Select features**, haz clic en **siguiente**.  
  
8.  En **servicios de certificados de Active Directory**, leer la información proporcionada y, a continuación, haz clic en **siguiente**.  
  
9. En **Confirmar selecciones de instalación**, haz clic en **instalar**. No se cerrará al asistente durante el proceso de instalación. Una vez finalizada la instalación, haz clic en **configurar certificado de servicios de Active Directory en el servidor de destino**. Abre el Asistente para configuración de AD CS. Leer la información de credenciales y, si es necesario, proporciona las credenciales de una cuenta que es un miembro del grupo Administradores de empresa. Haz clic en **siguiente**.  
  
10. En **servicios de rol**, haz clic en **entidad de certificación**y, a continuación, haz clic en **siguiente**.  
  
11. En la **tipo de instalación** página, comprueba que **CA de empresa** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
12. En la **especificar el tipo de la CA** página, comprueba que **CA de raíz** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
13. En la **especificar el tipo de la clave privada** página, comprueba que **crear una nueva clave privada** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
14. En la **criptografía para CA** página, mantén la configuración predeterminada de CSP (**proveedor de almacenamiento de claves de RSA #Microsoft Software**) y algoritmo hash (**SHA1**) y determinar la longitud de clave de carácter recomendada para la implementación. Longitudes de caracteres clave grandes proporcionan seguridad óptima; Sin embargo, se puede afectar al rendimiento del servidor y podrían no ser compatibles con aplicaciones heredadas. Se recomienda que mantenga la configuración predeterminada de 2048. Haz clic en **siguiente**.  
  
15. En la **nombre de entidad emisora** página, mantener el nombre común sugerido para la entidad de certificación o cambiar el nombre de acuerdo con los requisitos. Asegúrate de que estás seguro del que nombre de entidad emisora de certificados es compatible con tu convenciones de nomenclatura y efectos, porque no puedes cambiar el nombre de entidad emisora de certificados después de haber instalado AD CS. Haz clic en **siguiente**.  
  
16. En la **período de validez** página, en **especificar el período de validez**, escribe el número y selecciona un valor de hora (años, meses, días o semanas). Se recomienda el valor predeterminado de cinco años. Haz clic en **siguiente**.  
  
17. En la **base de datos de CA** página, en **especificar las ubicaciones de la base de datos**, especifica la ubicación de carpeta para la base de datos de certificado y el registro de la base de datos de certificados. Si especificas ubicaciones distintas de las ubicaciones predeterminadas, asegúrate de que las carpetas están protegidas con listas de control de acceso (ACL) que impiden que los usuarios no autorizados o equipos tengan acceso a los archivos de base de datos y el registro de la CA. Haz clic en **siguiente**.  
  
18. En **confirmación**, haz clic en **configurar** para aplicar las opciones seleccionadas y, a continuación, haz clic en **cerrar**.  
  


