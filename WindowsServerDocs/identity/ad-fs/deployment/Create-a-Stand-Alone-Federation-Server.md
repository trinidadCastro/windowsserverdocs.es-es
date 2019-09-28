---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Crear un servidor de federación independiente
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359657"
---
# <a name="create-a-stand-alone-federation-server"></a>Crear un servidor de federación independiente

Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede usar el siguiente procedimiento para configurar el equipo para que se convierta en un servidor de Federación de no__t-0alone. La acción de crear un servidor de Federación @ no__t-0alone también crea un nuevo Servicio de federación. Cree un servidor de Federación con el Asistente para la configuración del servidor de Federación de AD FS.  
  
> [!NOTE]  
> En el caso del diseño único Web no__t-0Sign @ no__t-1On \(SSO @ no__t-3, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en \( [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para crear un servidor de Federación @ no__t-0alone  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de Federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez completada la instalación del servicio de rol de Servicio de federación, abra el AD FS Management Snap @ no__t-0in y haga clic en el vínculo del **Asistente para configuración de servidor de Federación de AD FS** en la página **información general** o en el panel **acciones** .  
  
    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, navegue hasta la carpeta **C: \\Windows @ no__t-2ADFS** y, a continuación, haga doble @ no__t-3click **FsConfigWizard. exe**.  
  
2.  En la **Página principal**, comprueba que la opción **Crear un nuevo servicio de federación** esté seleccionada y, después, haz clic en **Siguiente**.  
  
3.  En la página **Seleccione el stand @ no__t-1Alone o la implementación de la granja de servidores** , haga clic en **stand @ no__t-3alone Federation Server**y, a continuación, haga clic en **siguiente**.  
  
    > [!IMPORTANT]  
    > Al seleccionar la opción de servidor de Federación stand @ no__t-0alone en el Asistente para configuración de servidor de Federación de AD FS, la cuenta de servicio asociada a este Servicio de federación se asignará automáticamente a la cuenta servicio de red. El uso de NETWORK SERVICE como cuenta de servicio solo se recomienda en situaciones en las que se está evaluando AD FS en un entorno de laboratorio de pruebas. Si piensa usar la opción de servidor de Federación stand @ no__t-0alone para implementar un servidor de Federación en un entorno de producción, es importante que cambie esta cuenta de servicio a una cuenta de servicio más adecuada que se pueda dedicar a atender solicitudes para Esta nueva Servicio de federación. El cambio de la cuenta de servicio a una cuenta que no sea servicio de red mitigará posibles vectores de ataque que, de otro modo, harían que el servidor de Federación fuera vulnerable a ataques malintencionados.  
  
4.  En la página **Especificar el nombre del Servicio de federación**, comprueba que el **Certificado SSL** que se muestra es correcto. Si no es así, seleccione el certificado adecuado en la lista **certificado SSL** .  
  
    Este certificado se genera a partir de la configuración Capa de sockets seguros \(SSL @ no__t-1 para el sitio web predeterminado. Si el sitio web predeterminado tiene un solo certificado SSL configurado, dicho certificado se mostrará y se seleccionará automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, se mostrarán todos los certificados en una lista y tendrás que elegir entre ellos. Si no hay ningún ajuste SSL configurado para el sitio web predeterminado, se generará una lista a partir de los certificados disponibles en el almacén de certificados personales del equipo local.  
  
    > [!NOTE]  
    > El asistente no te permitirá invalidar el certificado si hay un certificado SSL configurado para IIS. Esto garantiza que se conserve cualquier configuración IIS anterior prevista para los certificados SSL. Para evitar esta restricción, puede quitar el certificado o volver a configurarlo manualmente con la consola de administración de IIS.  
  
5.  Si el AD FS base de datos que seleccionó ya existe, aparecerá la página **base de datos de configuración de AD FS existente detectada** . Si aparece, haga clic en **Eliminar base de datos** y, a continuación, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos de esta AD FS base de datos no son importantes o no se usan en una granja de servidores de Federación de producción.  
  
6.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **siguiente** para empezar a configurar AD FS con esta configuración.  
  
7.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

