---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Crear un servidor de federación independiente
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 498b2f1c87181ee2675bf7a312c73ba90d0b0421
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962969"
---
# <a name="create-a-stand-alone-federation-server"></a>Crear un servidor de federación independiente

Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede usar el siguiente procedimiento para configurar el equipo para que se convierta en un \- servidor de Federación independiente. La acción de crear un \- servidor de Federación independiente también crea un nuevo servicio de Federación. Cree un servidor de Federación con el Asistente para la configuración del servidor de Federación de AD FS.

> [!NOTE]
> En el caso del \- diseño SSO de inicio de sesión único Web federado \- \( \) , debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-create-a-stand-alone-federation-server"></a>Para crear un \- servidor de Federación independiente

1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:

    -   Una vez finalizada la instalación del servicio de rol de Servicio de federación, abra el complemento Administración \- de AD FS y haga clic en el vínculo del **Asistente para configuración de servidor de federación de AD FS** en la página **información general** o en el panel **acciones** .

    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C: \\ Windows \\ ADFS** y, a continuación, \- haga doble clic en **FsConfigWizard.exe**.

2.  En la **Página principal**, comprueba que la opción **Crear un nuevo servicio de federación** esté seleccionada y, después, haz clic en **Siguiente**.

3.  En la página **seleccionar \- implementación independiente o de granja** , haga clic en servidor de ** \- Federación**independiente y, a continuación, haga clic en **siguiente**.

    > [!IMPORTANT]
    > Al seleccionar la \- opción servidor de Federación independiente en el Asistente para la configuración del servidor de Federación de AD FS, la cuenta de servicio asociada a este servicio de Federación se asignará automáticamente a la cuenta servicio de red. El uso de NETWORK SERVICE como cuenta de servicio solo se recomienda en situaciones en las que se está evaluando AD FS en un entorno de laboratorio de pruebas. Si piensa utilizar la \- opción de servidor de Federación independiente para implementar un servidor de Federación en un entorno de producción, es importante que cambie esta cuenta de servicio a una cuenta de servicio más adecuada que pueda estar dedicada a atender las solicitudes de esta nueva servicio de Federación. El cambio de la cuenta de servicio a una cuenta que no sea servicio de red mitigará posibles vectores de ataque que, de otro modo, harían que el servidor de Federación fuera vulnerable a ataques malintencionados.

4.  En la página **Especificar el nombre del Servicio de federación**, comprueba que el **Certificado SSL** que se muestra es correcto. Si no es así, seleccione el certificado adecuado en la lista **certificado SSL** .

    Este certificado se genera a partir de la configuración de Capa de sockets seguros \( SSL \) para el sitio web predeterminado. Si el sitio web predeterminado tiene un solo certificado SSL configurado, dicho certificado se mostrará y se seleccionará automáticamente para su uso. Si hay varios certificados SSL configurados para el sitio web predeterminado, se mostrarán todos los certificados en una lista y tendrás que elegir entre ellos. Si no hay ningún ajuste SSL configurado para el sitio web predeterminado, se generará una lista a partir de los certificados disponibles en el almacén de certificados personales del equipo local.

    > [!NOTE]
    > El asistente no te permitirá invalidar el certificado si hay un certificado SSL configurado para IIS. Esto garantiza que se conserve cualquier configuración IIS anterior prevista para los certificados SSL. Para evitar esta restricción, puede quitar el certificado o volver a configurarlo manualmente con la consola de administración de IIS.

5.  Si la base de datos de AD FS que seleccionó ya existe, aparece la página **Se detectó una base de datos de configuración de AD FS existente**. Si ocurre eso, haga clic en **Eliminar base de datos** y luego en **Siguiente**.

    > [!CAUTION]
    > Seleccione esta opción solo cuando esté seguro de que los datos en esta base de datos de AD FS no son importantes o no se usan en una granja de servidores de federación de producción.

6.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar AD FS con estos valores.

7.  En la página **Resultados de la configuración**, comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)

