---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: "Crear el primer servidor de federación en una granja de servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Crear el primer servidor de federación en una granja de servidores de federación

 >Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de instalar el servicio de rol de servicios de federación y configurar los certificados necesarios en un equipo, estás listo para configurar el equipo para convertirse en un servidor de federación. Puedes usar el siguiente procedimiento para configurar el equipo para convertirse en el primer servidor de federación de un nuevo conjunto de servidor de federación mediante el Asistente para configuración del servidor de federación de AD FS.  
  
La acción de crear el primer servidor de federación de un conjunto de también crea un nuevo servicio de federación y hace que este equipo en el servidor de federación principal. Esto significa que este equipo se configurará con una copia de la base de datos de configuración de AD FS read\ y escritura. Todos los otros servidores de federación de este conjunto deben replicar los cambios realizados en el servidor de federación principal solo read\ copias de la base de datos de configuración de AD FS que se almacenan localmente. Para obtener más información sobre este proceso de replicación, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para el diseño federados Web Single\-Sign\-On \(SSO\), debes tener al menos un servidor de federación de la organización de partner de la cuenta y al menos un servidor de federación de la organización de partner de recurso. Para obtener más información, consulta [dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pertenencia al grupo Administradores de dominio o una cuenta de dominio delegado que se ha concedido acceso de escritura en el contenedor de datos de programa en Active Directory, es lo mínimo necesario para completar este procedimiento.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Para crear el primer servidor de federación en una granja de servidores de federación  
  
1.  Hay dos formas de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar al asistente, realiza una de las siguientes acciones:  
  
    -   Una vez completada la instalación del servicio de rol de servicios de federación, abre la administración de AD FS en snap\ y haz clic en el **Asistente de configuración del servidor de AD FS federación** vincular en la **Introducción** página o en la **acciones** panel.  
  
    -   Cualquier momento después de que el Asistente para la instalación se completa, abre el Explorador de Windows, navega hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, clic double\ **FsConfigWizard.exe**.  
  
2.  En la **bienvenida** página, comprueba que **crear un nuevo servicio de federación** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  En la **selecciona Stand\-por sí solo o la implementación de granja** página, haz clic en **nueva granja de servidores de federación**y, a continuación, haz clic en **siguiente**.  
  
4.  En la **especifique el nombre de servicio de federación** página, comprueba que la **certificado SSL** que se muestra es correcta. Si esto no es el certificado correcto, selecciona el certificado apropiado de la **certificado SSL** lista.  
  
    Este certificado se genera desde la configuración de la capa de Sockets seguros \(SSL\) del sitio Web predeterminado. Si el sitio Web predeterminado tiene un único certificado SSL configurado, ese certificado se presenta y se selecciona automáticamente para su uso. Si se configuran varios certificados SSL para el sitio Web de forma predeterminada, todos los certificados se enumeran aquí y debe seleccionar entre ellos. Si no hay sin ninguna configuración de SSL para el sitio Web de forma predeterminada, se genera la lista de los certificados que están disponibles en el almacén de certificados personal en el equipo local.  
  
    > [!NOTE]  
    > El asistente no permitirá que invalidar el certificado si se configura un certificado SSL para IIS. Esto garantiza que cualquier se conserva la configuración anterior de IIS para los certificados SSL. Para evitar esta restricción, puedes quitar el certificado o volver a configurarla manualmente con la consola de administración de IIS.  
  
5.  Si la base de datos de AD FS que seleccionaste ya existe, la **detecta de base de datos de configuración de existente AD FS** aparecerá la página. Si aparece esa página, haz clic en **base de datos de eliminar**y, a continuación, haz clic en **siguiente**.  
  
    > [!CAUTION]  
    > Selecciona esta opción solo cuando está seguro de que los datos de esta base de datos de AD FS no están importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
6.  En la **especificar una cuenta de servicio** página, haz clic en **examinar**. En la **examinar** cuadro de diálogo, busque la cuenta de dominio que se usará como la cuenta del servicio de este conjunto de servidor de federación nueva y, a continuación, haz clic en **Aceptar**. Escribe la contraseña para esta cuenta, confirmarla y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]  
    > Consulta [configurar manualmente una cuenta de servicio para una granja de servidores de federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de federación. Cada servidor de federación de la granja de servidores de federación debe especificar la misma cuenta de servicio para el conjunto en funcionamiento. Por ejemplo, si la cuenta de servicio que se creó era contoso\\ADFS2SVC, cada equipo que se configura para el rol de servidor de federación y que participarán en la misma granja debe especificar contoso\\ADFS2SVC en este paso en el Asistente de configuración de servidor de federación de la granja de servidores en funcionamiento.  
  
7.  En la **listo para aplicar configuración** página, revisa los detalles. Si aparece la configuración sea correcta, haz clic en **siguiente** para empezar a configurar AD FS con estas opciones de configuración.  
  
8.  En la **resultados de la configuración** página, revisar los resultados. Cuando haya terminado de todos los pasos de configuración, haz clic en **cerrar** para salir del asistente.  
  
    > [!IMPORTANT]  
    > Para fines de implementación segura, detección de resolución y respuesta artefacto están deshabilitadas cuando usas el Asistente para configuración del servidor de federación de AD FS para configurar una granja de servidores de federación. Este asistente configura automáticamente la base de datos interna de Windows para almacenar datos de configuración del servicio. Es posible que, sin embargo, por error deshacer este cambio habilitando el extremo de resolución artefacto ya sea con la **extremos** nodo en la administración de AD FS en snap\ o el cmdlet Enable\ ADFSEndpoint en Windows PowerShell. Ten cuidado de no volver a configurar el valor predeterminado para que este extremo permanecerá deshabilitado cuando uses una granja de servidores de federación y la base de datos interna de Windows entre sí.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

