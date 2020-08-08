---
title: Instalar la entidad de certificación
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1aa5e75c9de54a2e6acd52982f0a2f640df108cd
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995520"
---
# <a name="install-the-certification-authority"></a>Instalar la entidad de certificación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar Active Directory servicios de Certificate Server (AD CS) para poder inscribir un certificado de servidor en servidores que ejecutan el servidor de directivas de redes (NPS), el servicio de enrutamiento y acceso remoto (RRAS), o ambos.

> [!IMPORTANT]
> -   Antes de instalar Active Directory servicios de Certificate Server, debe asignar un nombre al equipo, configurar el equipo con una dirección IP estática y unir el equipo al dominio. Para obtener más información sobre cómo realizar estas tareas, vea la [Guía de red principal](../../core-network-guide.md)de Windows Server 2016.
> -   Para realizar este procedimiento, el equipo en el que se va a instalar AD CS debe unirse a un dominio donde están instalados los Servicios de dominio de Active Directory (AD DS).

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
> Si desea usar Windows PowerShell para instalar Active Directory servicios de Certificate Server, consulte [install-AdcsCertificationAuthority](/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) para los cmdlets y parámetros opcionales.

1.  Inicie sesión como miembro del grupo Administradores de empresas y del grupo Admins. del dominio del dominio raíz.

2.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

3.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

4.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

5.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haga clic en **Next**.

6.  En **Seleccionar roles de servidor**, en **roles**, seleccione **Active Directory servicios de Certificate**Server. Cuando se le pida que agregue las características necesarias, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.

7.  En **seleccionar características**, haga clic en **siguiente**.

8.  En **Active Directory servicios de Certificate Server**, lea la información proporcionada y, a continuación, haga clic en **siguiente**.

9. En **Confirmar selecciones de instalación**, haga clic en **Instalar**. No cierre el asistente durante el proceso de instalación. Una vez finalizada la instalación, haga clic en **configurar servicios de Certificate server Active Directory en el servidor de destino**. Se abre el Asistente para configuración de AD CS. Lea la información de las credenciales y, si es necesario, proporcione las credenciales de una cuenta que sea miembro del grupo administradores de empresas. Haga clic en **Next**.

10. En **servicios de rol**, haga clic en **entidad de certificación**y, a continuación, en **siguiente**.

11. En la página **tipo de instalación** , compruebe que la opción **CA empresarial** está seleccionada y, a continuación, haga clic en **siguiente**.

12. En la página **especificar el tipo de la entidad de certificación** , compruebe que la opción **CA raíz** esté seleccionada y, a continuación, haga clic en **siguiente**.

13. En la página **especificar el tipo de la clave privada** , compruebe que la opción **crear una nueva clave privada** está seleccionada y, a continuación, haga clic en **siguiente**.

14. En la página **Criptografía para CA** , mantenga la configuración predeterminada de CSP (**RSA # proveedor de almacenamiento de claves de software de Microsoft**) y el algoritmo hash (**sha2**) y determine la mejor longitud de caracteres de clave para la implementación. Las longitudes de caracteres de clave grande proporcionan una seguridad óptima. sin embargo, pueden afectar al rendimiento del servidor y puede que no sean compatibles con las aplicaciones heredadas. Se recomienda que mantenga el valor predeterminado de 2048. Haga clic en **Next**.

15. En la página **nombre de CA** , conserve el nombre común sugerido para la CA o cambie el nombre de acuerdo con sus requisitos. Asegúrese de que el nombre de la entidad de certificación es compatible con las convenciones de nomenclatura y los propósitos, ya que no se puede cambiar el nombre de la entidad de certificación después de haber instalado AD CS. Haga clic en **Next**.

16. En la página **período de validez** , en **especifique el período de validez**, escriba el número y seleccione un valor de tiempo (años, meses, semanas o días). Se recomienda el valor predeterminado de cinco años. Haga clic en **Next**.

17. En la página **base de datos de CA** , en **Especifique las ubicaciones de base de datos**, especifique la ubicación de la carpeta para la base de datos de certificados y el registro de base de datos de certificados. Si especifica ubicaciones distintas de las ubicaciones predeterminadas, asegúrese de que las carpetas estén protegidas mediante listas de control de acceso (ACL) que impidan que usuarios o equipos no autorizados tengan acceso a los archivos de registro y la base de datos de la entidad de certificación. Haga clic en **Next**.

18. En **confirmación**, haga clic en **configurar** para aplicar las selecciones y, a continuación, haga clic en **cerrar**.