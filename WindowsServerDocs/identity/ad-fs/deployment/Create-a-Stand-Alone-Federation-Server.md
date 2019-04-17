---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: "Crear un servidor de federación independiente"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>Crear un servidor de federación independiente

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de instalar el servicio de rol de servicios de federación y configurar los certificados necesarios en un equipo, estás listo para configurar el equipo para convertirse en un servidor de federación. Puedes usar el siguiente procedimiento para configurar el equipo para convertirte en un servidor de federación stand\ independiente. La acción de crear un servidor de federación stand\ solo también crea un nuevo servicio de federación. Crear un servidor de federación con el Asistente para configuración del servidor de federación de AD FS.  
  
> [!NOTE]  
> Para el diseño federados Web Single\-Sign\-On \(SSO\), debes tener al menos un servidor de federación de la organización de partner de la cuenta y al menos un servidor de federación de la organización de partner de recurso. Para obtener más información, consulta [dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para crear un servidor de federación stand\ solo  
  
1.  Hay dos formas de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar al asistente, realiza una de las siguientes acciones:  
  
    -   Una vez completada la instalación del servicio de rol de servicios de federación, abre la administración de AD FS en snap\ y haz clic en el **Asistente de configuración del servidor de AD FS federación** vincular en la **Introducción** página o en la **acciones** panel.  
  
    -   En cualquier momento después de que el Asistente para la instalación se completa, abre el Explorador de Windows, navega hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, clic double\ **FsConfigWizard.exe**.  
  
2.  En la **bienvenida** página, comprueba que **crear un nuevo servicio de federación** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  En la **selecciona Stand\-por sí solo o la implementación de granja** página, haz clic en **Stand\ solo servidor de federación**y, a continuación, haz clic en **siguiente**.  
  
    > [!IMPORTANT]  
    > Cuando seleccionas la opción de servidor de federación Stand\ solo en el Asistente para configuración del servidor de federación de AD FS, la cuenta de servicio asociada con este servicio de federación se asigna a la cuenta de servicio de red. Con el servicio de red como la cuenta de servicio solo se recomienda en situaciones donde están evaluando AD FS en un entorno de laboratorio de prueba. Si tienes previsto usar la opción de servidor de federación Stand\ solo para implementar un servidor de federación en un entorno de producción, es importante que cambiar esta cuenta de servicio a una cuenta de servicio más adecuada que se puede dedicar a atender solicitudes para este nuevo servicio de federación. Cambiar la cuenta de servicio a una cuenta distinta de servicio de red se solucionará los vectores de ataque posibles que realizaría el servidor de federación vulnerable a ataques malintencionados.  
  
4.  En la **especifique el nombre de servicio de federación** página, comprueba que la **certificado SSL** que se muestra es correcta. Si no es así, selecciona el certificado apropiado de la **certificado SSL** lista.  
  
    Este certificado se genera desde la configuración de la capa de Sockets seguros \(SSL\) del sitio Web predeterminado. Si el sitio Web predeterminado tiene un único certificado SSL configurado, ese certificado se presenta y se selecciona automáticamente para su uso. Si se configuran varios certificados SSL para el sitio Web de forma predeterminada, todos los certificados se enumeran aquí y debe seleccionar entre ellos. Si no hay sin ninguna configuración de SSL para el sitio Web de forma predeterminada, se genera la lista de los certificados que están disponibles en el almacén de certificados personal en el equipo local.  
  
    > [!NOTE]  
    > El asistente no permitirá que invalidar el certificado si se configura un certificado SSL para IIS. Esto garantiza que cualquier se conserva la configuración anterior de IIS para los certificados SSL. Para evitar esta restricción, puedes quitar el certificado o reconfigurarlo manualmente con la consola de administración de IIS.  
  
5.  Si la base de datos de AD FS que seleccionaste ya existe, la **detecta de base de datos de configuración de existente AD FS** aparecerá la página. Si esto sucede, haz clic en **base de datos de eliminar**y, a continuación, haz clic en **siguiente**.  
  
    > [!CAUTION]  
    > Selecciona esta opción solo cuando está seguro de que los datos de esta base de datos de AD FS no están importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
6.  En la **listo para aplicar configuración** página, revisa los detalles. Si aparece la configuración sea correcta, haz clic en **siguiente** para empezar a configurar AD FS con estas opciones de configuración.  
  
7.  En la **resultados de la configuración** página, revisar los resultados. Cuando haya terminado de todos los pasos de configuración, haz clic en **cerrar** para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

