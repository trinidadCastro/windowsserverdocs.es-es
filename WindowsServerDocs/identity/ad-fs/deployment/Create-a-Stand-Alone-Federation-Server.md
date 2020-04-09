---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Crear un servidor de federación independiente
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 603786d8553cce20f0b559ba8a91dfc29f760488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855488"
---
# <a name="create-a-stand-alone-federation-server"></a>Crear un servidor de federación independiente

Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede usar el siguiente procedimiento para configurar el equipo para que se convierta en un servidor de Federación\-independiente. La acción de crear un stand\-servidor de Federación independiente también crea un nuevo Servicio de federación. Cree un servidor de Federación con el Asistente para la configuración del servidor de Federación de AD FS.  
  
> [!NOTE]  
> Para el inicio de sesión único Web federado\-\-en \(diseño de\) SSO, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para crear un servidor de Federación\-independiente  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, siga uno de estos procedimientos:  
  
    -   Una vez finalizada la instalación del servicio de rol de Servicio de federación, abra el\-de administración de AD FS en y haga clic en el vínculo **AD FS del Asistente para configuración de servidor de Federación** en la página **información general** o en el panel **acciones** .  
  
    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C:\\Windows\\ADFS** y, a continuación, haga doble\-haga clic en **FsConfigWizard. exe**.  
  
2.  En la página **Bienvenida**, compruebe que la opción **Crear un nuevo servicio de federación** esté seleccionada y luego haga clic en **Siguiente**.  
  
3.  En la página **seleccionar\-independiente o implementación de la granja de servidores** , haga clic en **\-servidor de Federación independiente**y, a continuación, haga clic en **siguiente**.  
  
    > [!IMPORTANT]  
    > Al seleccionar la opción de servidor de Federación independiente\-del Asistente para la configuración del servidor de Federación de AD FS, la cuenta de servicio asociada a este Servicio de federación se asignará automáticamente a la cuenta servicio de red. El uso de NETWORK SERVICE como cuenta de servicio solo se recomienda en situaciones en las que se está evaluando AD FS en un entorno de laboratorio de pruebas. Si piensa usar la opción de servidor de Federación independiente\-para implementar un servidor de Federación en un entorno de producción, es importante que cambie esta cuenta de servicio a una cuenta de servicio más adecuada que se pueda usar para atender las solicitudes de esta nueva Servicio de federación. El cambio de la cuenta de servicio a una cuenta que no sea servicio de red mitigará posibles vectores de ataque que, de otro modo, harían que el servidor de Federación fuera vulnerable a ataques malintencionados.  
  
4.  En la página **Especificar el nombre de servicio de federación**, compruebe que el **Certificado SSL** sea el correcto. Si no es así, seleccione el certificado adecuado en la lista **certificado SSL** .  
  
    Este certificado se genera a partir de la configuración Capa de sockets seguros \(\) SSL para el sitio web predeterminado. Si el sitio web predeterminado solo tiene configurado un certificado SSL, se presenta ese certificado y se selecciona automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, aparecen aquí todos esos certificados y debe seleccionar entre ellos. Si no hay ningún certificado SSL configurado para el sitio web predeterminado, la lista se genera a partir de los certificados que están disponibles en los certificados personales almacenados en el equipo local.  
  
    > [!NOTE]  
    > El asistente no permitirá reemplazar el certificado si un certificado SSL está configurado para IIS. Esto garantiza que la configuración de IIS anterior pretendida para los certificados SSL se mantiene. Para evitar esta restricción, puede quitar el certificado o volver a configurarlo manualmente con la consola de administración de IIS.  
  
5.  Si la base de datos de AD FS que seleccionó ya existe, aparece la página **Se detectó una base de datos de configuración de AD FS existente**. Si ocurre eso, haga clic en **Eliminar base de datos** y luego en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos en esta base de datos de AD FS no son importantes o no se usan en una granja de servidores de federación de producción.  
  
6.  En la página **Listo para aplicar configuración**, revise los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar AD FS con estos valores.  
  
7.  En la página **Resultados de configuración**, revise los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

