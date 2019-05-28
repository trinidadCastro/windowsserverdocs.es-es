---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Crear el primer servidor de federación en una granja de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e16289142ea2e53adba52a4ed8f6c01a929a530d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192223"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Crear el primer servidor de federación en una granja de servidores de federación

Después de instalar el servicio de rol Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para convertirse en un servidor de federación. Puede usar el procedimiento siguiente para configurar el equipo para convertirse en el primer servidor de federación en una nueva granja de servidores de federación mediante el Asistente para configuración del servidor de federación de AD FS.  
  
Al crear el primer servidor de federación de una granja, también se crea un nuevo servicio de federación y el equipo se convierte en el servidor de federación principal. Esto significa que este equipo se configurará con una lectura\/escribir la copia de la base de datos de configuración de AD FS. Todos los demás servidores de federación de la granja deben replicar los cambios realizados en el servidor de federación principal para su lectura\-solo copias de la base de datos de configuración de AD FS que almacenen localmente. Para obtener más información sobre el proceso de replicación, consulta [El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para Federated Web único\-sesión\-en \(SSO\) diseño, debe tener al menos un servidor de federación en la organización del asociado de cuenta y al menos un servidor de federación en la organización del asociado de recurso . Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
Para completar este procedimiento, lo mínimo que se necesita es pertenecer a Administradores del dominio, o a una cuenta de dominio delegada que disponga de acceso de escritura al contenedor Datos de programas en Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Cómo crear el primer servidor de federación en una granja de servidores de federación  
  
1.  Hay dos maneras de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez completada la instalación del servicio de rol Servicio de federación, abra el complemento Administración de AD FS\-en y haga clic en el **Asistente para configuración de AD FS Federation Server** vincular en el **Introducción** página o en el **acciones** panel.  
  
    -   Cuando el Asistente para la instalación haya finalizado, abre el Explorador de Windows, navegue hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, doble\-haga clic en **FsConfigWizard.exe**.  
  
2.  En la **Página principal**, comprueba que la opción **Crear un nuevo servicio de federación** esté seleccionada y, después, haz clic en **Siguiente**.  
  
3.  En el **seleccione soporte\-Alone o implementación de granja de servidores** página, haga clic en **nueva granja de servidores de federación**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Especificar el nombre del Servicio de federación**, comprueba que el **Certificado SSL** que se muestra es correcto. Si no es el certificado correcto, selecciona el adecuado en la lista **Certificado SSL**.  
  
    Este certificado se genera a partir de la capa de Sockets seguros \(SSL\) configuración para el sitio Web predeterminado. Si el sitio web predeterminado tiene un solo certificado SSL configurado, dicho certificado se mostrará y se seleccionará automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, se mostrarán todos los certificados en una lista y tendrás que elegir entre ellos. Si no hay ningún ajuste SSL configurado para el sitio web predeterminado, se generará una lista a partir de los certificados disponibles en el almacén de certificados personales del equipo local.  
  
    > [!NOTE]  
    > El asistente no te permitirá invalidar el certificado si hay un certificado SSL configurado para IIS. Esto garantiza que se conserve cualquier configuración IIS anterior prevista para los certificados SSL. Para solucionar esta restricción, puedes quitar el certificado o reconfigurarlo manualmente con la consola de administración IIS.  
  
5.  Si la base de datos de AD FS que seleccionó ya existe, el **existente de AD FS Configuration Database detectada** aparece la página. Si aparece dicha página, haz clic en **Eliminar base de datos**y, después, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos de esta base de datos de AD FS no son importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
6.  En la página **Especificar una cuenta de servicio** , haz clic en **Examinar**. En el cuadro de diálogo **Examinar** , localiza la cuenta de dominio que se utilizará como cuenta de servicio en esta nueva granja de servidores de federación y haz clic en **Aceptar**. Escribe la contraseña de la cuenta, confírmala y haz clic en **Siguiente**.  
  
    > [!NOTE]  
    > Consulte [configurar manualmente una cuenta de servicio para una granja de servidores de federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de federación. Cada servidor de federación en la granja de servidores de federación debe especificar la misma cuenta de servicio para la granja de servidores sea operativa. Por ejemplo, si la cuenta de servicio que se creó es contoso\\ADFS2SVC, cada equipo que configura para el rol de servidor de federación y que participará en la misma granja debe especificar contoso\\ADFS2SVC en este paso el Asistente de configuración de servidor de federación para la granja de servidores sea operativa.  
  
7.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **siguiente** para comenzar a configurar AD FS con esta configuración.  
  
8.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
    > [!IMPORTANT]  
    > Para que la implementación sea segura, la resolución de artefactos y la detección de respuestas se deshabilitan cuando utilizas el asistente para la configuración del servidor de federación de AD FS a fin de configurar una granja de servidores de federación. Este asistente configura automáticamente Windows Internal Database para almacenar los datos de configuración del servicio. Es posible que, sin embargo, podrías deshacer este cambio si habilitas el extremo de la resolución de artefactos utilizando el **extremos** nodo en el complemento Administración de AD FS\-en o permitir\-cmdlet ADFSEndpoint en Windows PowerShell. Procura no reconfigurar la configuración predeterminada para que este extremo permanezca deshabilitado cuando utilices conjuntamente una granja de servidores de federación y Windows Internal Database.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

