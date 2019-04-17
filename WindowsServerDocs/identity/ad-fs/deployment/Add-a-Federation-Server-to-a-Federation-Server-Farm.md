---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: "Agregar un servidor de federación a una granja de servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Agregar un servidor de federación a una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de instalar el servicio de rol de servicios de federación y configurar los certificados necesarios en un equipo, estás listo para configurar el equipo para convertirse en un servidor de federación. Puedes usar el siguiente procedimiento para unir un equipo a un nuevo conjunto de servidor de federación.  
  
Unir un equipo a un conjunto con el Asistente para configuración del servidor de federación de AD FS. Al usar este asistente para unir un equipo a un conjunto existente, el equipo está configurado con una copia de la base de datos de configuración de AD FS sólo read\ y que debe recibir actualizaciones desde un servidor de federación principal.  
  
> [!NOTE]  
> Para el diseño federados Web Single\-Sign\-On \(SSO\), debes tener al menos un servidor de federación de la organización de partner de la cuenta y al menos un servidor de federación de la organización de partner de recurso. Para obtener más información, consulta [dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para agregar un servidor de federación a una granja de servidores de federación  
  
1.  Hay dos formas de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar al asistente, realiza una de las siguientes acciones:  
  
    -   Una vez completada la instalación del servicio de rol de servicios de federación, abre la administración de AD FS en snap\ y haz clic en el **Asistente de configuración del servidor de AD FS federación** vincular en la **Introducción** página o en la **acciones** panel.  
  
    -   En cualquier momento después de que el Asistente para la instalación se completa, abre el Explorador de Windows, navega hasta la **C:\\Windows\\ADFS** carpeta y double\ clic **FsConfigWizard.exe**.  
  
2.  En la **bienvenida** página, comprueba que **agregar un servidor de federación a un servicio de federación existente** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  Si la base de datos de AD FS que seleccionaste ya existe, la **detecta de base de datos de configuración de existente AD FS** aparecerá la página. Si esto sucede, haz clic en **base de datos de eliminar**y, a continuación, haz clic en **siguiente**.  
  
    > [!CAUTION]  
    > Selecciona esta opción solo cuando está seguro de que los datos de esta base de datos de AD FS no están importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
4.  En la **especificar el servidor de federación principal y la cuenta de servicio** página, debajo **el nombre del servidor de federación principal**, escribe el nombre del equipo del servidor de federación principal de la batería y, a continuación, haz clic en **examinar**. En la **examinar** cuadro de diálogo, busque la cuenta de dominio que se usa como la cuenta de servicio todos los otros servidores de federación de la granja de servidores de federación existente y, a continuación, haz clic en **Aceptar**. Escribe la contraseña y confirmarla y, a continuación, haz clic en **siguiente**:  
  
    > [!NOTE]  
    > Para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de federación, consulta [configurar manualmente una cuenta de servicio para una granja de servidores de federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de federación de la granja de servidores de federación debe especificar la misma cuenta de servicio para el conjunto en funcionamiento. Por ejemplo, si la cuenta de servicio que se creó era contoso\\ADFS2SVC, cada equipo se configura para el rol de servidor de federación y que participarán en la misma granja debe especificar contoso\\ADFS2SVC en este paso en el Asistente de configuración de servidor de federación de la granja de servidores en funcionamiento.  
  
5.  En la **listo para aplicar configuración** página, revisa los detalles. Si aparece la configuración sea correcta, haz clic en **siguiente** para empezar a configurar AD FS con estas opciones de configuración.  
  
6.  En la **resultados de la configuración** página, revisar los resultados. Cuando haya terminado de todos los pasos de configuración, haz clic en **cerrar** para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

