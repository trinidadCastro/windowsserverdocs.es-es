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
ms.openlocfilehash: 0fe5c3160f661357536ef3bd60762873063c8ed0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855478"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Crear el primer servidor de federación en una granja de servidores de federación

Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede usar el siguiente procedimiento para configurar el equipo para que se convierta en el primer servidor de Federación de una nueva granja de servidores de Federación mediante el Asistente para configuración de servidor de Federación de AD FS.  
  
Al crear el primer servidor de federación de una granja, también se crea un nuevo servicio de federación y el equipo se convierte en el servidor de federación principal. Esto significa que este equipo se configurará con una copia de lectura\/escritura de la base de datos de configuración de AD FS. Todos los demás servidores de Federación de esta granja deben replicar los cambios realizados en el servidor de Federación principal en sus\-de lectura solo copias de la base de datos de configuración AD FS que almacenan de forma local. Para obtener más información sobre el proceso de replicación, consulta [El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para el inicio de sesión único Web federado\-\-en \(diseño de\) SSO, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
Para completar este procedimiento, lo mínimo que se necesita es pertenecer a Administradores del dominio, o a una cuenta de dominio delegada que disponga de acceso de escritura al contenedor Datos de programas en Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Cómo crear el primer servidor de federación en una granja de servidores de federación  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, siga uno de estos procedimientos:  
  
    -   Una vez finalizada la instalación del servicio de rol de Servicio de federación, abra el\-de administración de AD FS en y haga clic en el vínculo **AD FS del Asistente para configuración de servidor de Federación** en la página **información general** o en el panel **acciones** .  
  
    -   Cuando haya finalizado el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C:\\Windows\\ADFS** y, a continuación, haga doble\-haga clic en **FsConfigWizard. exe**.  
  
2.  En la página **Bienvenida**, compruebe que la opción **Crear un nuevo servicio de federación** esté seleccionada y luego haga clic en **Siguiente**.  
  
3.  En la página **seleccionar\-independiente o implementación de la granja de servidores** , haga clic en **nueva granja**de servidores de Federación y, a continuación, en **siguiente**.  
  
4.  En la página **Especificar el nombre de servicio de federación**, compruebe que el **Certificado SSL** sea el correcto. Si no es el certificado correcto, seleccione el certificado apropiado en la lista **Certificado SSL**.  
  
    Este certificado se genera a partir de la configuración Capa de sockets seguros \(\) SSL para el sitio web predeterminado. Si el sitio web predeterminado solo tiene configurado un certificado SSL, se presenta ese certificado y se selecciona automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, aparecen aquí todos esos certificados y debe seleccionar entre ellos. Si no hay ningún certificado SSL configurado para el sitio web predeterminado, la lista se genera a partir de los certificados que están disponibles en los certificados personales almacenados en el equipo local.  
  
    > [!NOTE]  
    > El asistente no permitirá reemplazar el certificado si un certificado SSL está configurado para IIS. Esto garantiza que la configuración de IIS anterior pretendida para los certificados SSL se mantiene. Para solucionar esta restricción, puede quitar el certificado o reconfigurarlo manualmente con la Consola de administración de IIS.  
  
5.  Si la base de datos de AD FS que seleccionó ya existe, aparece la página **Se detectó una base de datos de configuración de AD FS existente**. Si aparece dicha página, haga clic en **Eliminar base de datos** y luego haga clic en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos en esta base de datos de AD FS no son importantes o no se usan en una granja de servidores de federación de producción.  
  
6.  En la página **Especificar cuenta de servicio**, haga clic en **Examinar**. En el cuadro de diálogo **Examinar**, busque la cuenta de dominio que se usará como cuenta de servicio en esta nueva granja de servidores de federación y luego haga clic en **Aceptar**. Escriba la contraseña para esta cuenta, confírmela y luego haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Consulte [configurar manualmente una cuenta de servicio para una granja de servidores de Federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de Federación. Cada servidor de Federación de la granja de servidores de Federación debe especificar la misma cuenta de servicio para que la granja esté operativa. Por ejemplo, si la cuenta de servicio que se creó era contoso\\ADFS2SVC, cada equipo que configure para el rol de servidor de Federación y que participará en la misma granja debe especificar contoso\\ADFS2SVC en este paso en el Asistente para configuración de servidor de Federación para que la granja esté operativa.  
  
7.  En la página **Listo para aplicar configuración**, revise los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar AD FS con estos valores.  
  
8.  En la página **Resultados de configuración**, revise los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
    > [!IMPORTANT]  
    > A los efectos de una implementación segura, la resolución de artefactos y la detección de respuestas están deshabilitadas cuando usa el Asistente para la configuración del servidor de federación de AD FS para configurar una granja de servidores de federación. Este asistente configura automáticamente Windows Internal Database para almacenar los datos de configuración de servicios. Sin embargo, es posible que deshaga por error este cambio habilitando el extremo de resolución de artefactos mediante el nodo **puntos de conexión** en el\-de AD FS de administración de en o el cmdlet enable\-ADFSEndpoint en Windows PowerShell. Tenga cuidado de no reconfigurar los valores predeterminados de modo que este extremo permanezca deshabilitado cuando use una granja de servidores de federación y Windows Internal Database juntos.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

