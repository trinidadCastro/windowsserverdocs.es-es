---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Crear el primer servidor de federación en una granja de servidores de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2ed7302a0712c50837d985c6d55ec133ffdcd5fa
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965987"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Crear el primer servidor de federación en una granja de servidores de federación

Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede usar el siguiente procedimiento para configurar el equipo para que se convierta en el primer servidor de Federación de una nueva granja de servidores de Federación mediante el Asistente para configuración de servidor de Federación de AD FS.  
  
Al crear el primer servidor de federación de una granja, también se crea un nuevo servicio de federación y el equipo se convierte en el servidor de federación principal. Esto significa que este equipo se configurará con una \/ copia de lectura y escritura de la base de datos de configuración de AD FS. Todos los demás servidores de Federación de esta granja deben replicar los cambios realizados en el servidor de Federación principal en sus \- copias de solo lectura de la base de datos de configuración de AD FS que almacenan de forma local. Para obtener más información sobre el proceso de replicación, consulta [El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> En el caso del \- diseño SSO de inicio de sesión único Web federado \- \( \) , debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).  
  
Para completar este procedimiento, lo mínimo que se necesita es pertenecer a Administradores del dominio, o a una cuenta de dominio delegada que disponga de acceso de escritura al contenedor Datos de programas en Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Cómo crear el primer servidor de federación en una granja de servidores de federación  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez finalizada la instalación del servicio de rol de Servicio de federación, abra el complemento Administración \- de AD FS y haga clic en el vínculo del **Asistente para configuración de servidor de federación de AD FS** en la página **información general** o en el panel **acciones** .  
  
    -   Cuando haya finalizado el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C: \\ Windows \\ ADFS** y, a continuación, \- haga doble clic en **FsConfigWizard.exe**.  
  
2.  En la **Página principal**, comprueba que la opción **Crear un nuevo servicio de federación** esté seleccionada y, después, haz clic en **Siguiente**.  
  
3.  En la página **seleccionar \- implementación independiente o de granja** , haga clic en **nueva granja**de servidores de Federación y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Especificar el nombre del Servicio de federación**, comprueba que el **Certificado SSL** que se muestra es correcto. Si no es el certificado correcto, selecciona el adecuado en la lista **Certificado SSL**.  
  
    Este certificado se genera a partir de la configuración de Capa de sockets seguros \( SSL \) para el sitio web predeterminado. Si el sitio web predeterminado tiene un solo certificado SSL configurado, dicho certificado se mostrará y se seleccionará automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, se mostrarán todos los certificados en una lista y tendrás que elegir entre ellos. Si no hay ningún ajuste SSL configurado para el sitio web predeterminado, se generará una lista a partir de los certificados disponibles en el almacén de certificados personales del equipo local.  
  
    > [!NOTE]  
    > El asistente no te permitirá invalidar el certificado si hay un certificado SSL configurado para IIS. Esto garantiza que se conserve cualquier configuración IIS anterior prevista para los certificados SSL. Para solucionar esta restricción, puedes quitar el certificado o reconfigurarlo manualmente con la consola de administración IIS.  
  
5.  Si la base de datos de AD FS que seleccionó ya existe, aparece la página **Se detectó una base de datos de configuración de AD FS existente**. Si aparece dicha página, haz clic en **Eliminar base de datos** y, después, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos en esta base de datos de AD FS no son importantes o no se usan en una granja de servidores de federación de producción.  
  
6.  En la página **Especificar una cuenta de servicio**, haz clic en **Examinar**. En el cuadro de diálogo **Examinar**, localiza la cuenta de dominio que se utilizará como cuenta de servicio en esta nueva granja de servidores de federación y haz clic en **Aceptar**. Escribe la contraseña de la cuenta, confírmala y haz clic en **Siguiente**.  
  
    > [!NOTE]  
    > Consulte [configurar manualmente una cuenta de servicio para una granja de servidores de Federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de Federación. Cada servidor de Federación de la granja de servidores de Federación debe especificar la misma cuenta de servicio para que la granja esté operativa. Por ejemplo, si la cuenta de servicio que se creó era contoso \\ ADFS2SVC, cada equipo que configure para el rol de servidor de Federación y que participará en la misma granja debe especificar contoso \\ ADFS2SVC en este paso del Asistente para la configuración del servidor de Federación para que la granja esté operativa.  
  
7.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar AD FS con estos valores.  
  
8.  En la página **Resultados de la configuración**, comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
    > [!IMPORTANT]  
    > Para que la implementación sea segura, la resolución de artefactos y la detección de respuestas se deshabilitan cuando utilizas el asistente para la configuración del servidor de federación de AD FS a fin de configurar una granja de servidores de federación. Este asistente configura automáticamente Windows Internal Database para almacenar los datos de configuración del servicio. Sin embargo, es posible que deshaga por error este cambio habilitando el extremo de resolución de artefactos mediante el nodo **extremos** del complemento administración \- de AD FS en o el cmdlet enable \- ADFSEndpoint en Windows PowerShell. Procura no reconfigurar la configuración predeterminada para que este extremo permanezca deshabilitado cuando utilices conjuntamente una granja de servidores de federación y Windows Internal Database.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
