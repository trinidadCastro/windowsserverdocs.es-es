---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Crear un servidor de federación independiente
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4b70b0b048f66f9a8ba19cd7990dde57e0655ae4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192229"
---
# <a name="create-a-stand-alone-federation-server"></a>Crear un servidor de federación independiente

Después de instalar el servicio de rol Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para convertirse en un servidor de federación. Puede usar el procedimiento siguiente para configurar el equipo para convertirse en un soporte\-servidor de federación por sí solo. La acción de crear un soporte\-servidor de federación independiente también crea un nuevo servicio de federación. Crear un servidor de federación con el Asistente para configuración del servidor de federación de AD FS.  
  
> [!NOTE]  
> Para Federated Web único\-sesión\-en \(SSO\) diseño, debe tener al menos un servidor de federación en la organización del asociado de cuenta y al menos un servidor de federación en la organización del asociado de recurso . Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para crear un soporte\-servidor de federación independiente  
  
1.  Hay dos maneras de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez completada la instalación del servicio de rol Servicio de federación, abra el complemento Administración de AD FS\-en y haga clic en el **Asistente para configuración de AD FS Federation Server** vincular en el **Introducción** página o en el **acciones** panel.  
  
    -   En cualquier momento después de que el Asistente para la instalación está completa, abra el Explorador de Windows, navegue hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, doble\-haga clic en **FsConfigWizard.exe**.  
  
2.  En la **Página principal**, comprueba que la opción **Crear un nuevo servicio de federación** esté seleccionada y, después, haz clic en **Siguiente**.  
  
3.  En el **seleccione soporte\-Alone o implementación de granja de servidores** página, haga clic en **soporte\-servidor de federación independiente**y, a continuación, haga clic en **siguiente**.  
  
    > [!IMPORTANT]  
    > Cuando se selecciona el soporte\-opción de servidor de federación independiente en el Asistente para configuración del servidor de federación de AD FS, la cuenta de servicio asociada con este servicio de federación se asignarán automáticamente a la cuenta de servicio de red. Usando NETWORK SERVICE como cuenta de servicio solo se recomienda en situaciones donde va a evaluar AD FS en un entorno de laboratorio de pruebas. Si va a usar el soporte\-opción de servidor de federación independiente para implementar un servidor de federación en un entorno de producción, es importante que cambie esta cuenta de servicio a una cuenta de servicio más adecuada que pueda estar dedicada al servicio solicitudes de este nuevo servicio de federación. Cambiar la cuenta de servicio a una cuenta distinta de servicio de red mitigar los vectores de ataque posibles que realizaría el servidor de federación sea vulnerable a ataques malintencionados.  
  
4.  En la página **Especificar el nombre del Servicio de federación**, comprueba que el **Certificado SSL** que se muestra es correcto. Si no aparece, seleccione el certificado adecuado en el **certificado SSL** lista.  
  
    Este certificado se genera a partir de la capa de Sockets seguros \(SSL\) configuración para el sitio Web predeterminado. Si el sitio web predeterminado tiene un solo certificado SSL configurado, dicho certificado se mostrará y se seleccionará automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, se mostrarán todos los certificados en una lista y tendrás que elegir entre ellos. Si no hay ningún ajuste SSL configurado para el sitio web predeterminado, se generará una lista a partir de los certificados disponibles en el almacén de certificados personales del equipo local.  
  
    > [!NOTE]  
    > El asistente no te permitirá invalidar el certificado si hay un certificado SSL configurado para IIS. Esto garantiza que se conserve cualquier configuración IIS anterior prevista para los certificados SSL. Para evitar esta restricción, puede quitar el certificado o reconfigurarlo manualmente con la consola de administración de IIS.  
  
5.  Si la base de datos de AD FS que seleccionó ya existe, el **existente de AD FS Configuration Database detectada** aparece la página. Si aparece, haga clic en **Eliminar base de datos** y, a continuación, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos de esta base de datos de AD FS no son importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
6.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **siguiente** para comenzar a configurar AD FS con esta configuración.  
  
7.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

