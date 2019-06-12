---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creación de una relación de confianza para usuario autenticado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444360"
---
# <a name="create-a-relying-party-trust"></a>Creación de una relación de confianza para usuario autenticado


El siguiente documento proporciona información sobre cómo crear una relación de confianza para usuario autenticado manualmente y con metadatos de federación.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para crear un notificaciones compatible con usuarios de confianza de confianza manualmente 

Para agregar una nueva entidad de confianza mediante el complemento Administración de AD FS\-en y manualmente la configuración, lleve a cabo el siguiente procedimiento en un servidor de federación.  

El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En **acciones**, haga clic en **agregar confianza**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  En el **bienvenida** página, elija **notificaciones** y haga clic en **iniciar**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Escribir manualmente los datos acerca del usuario de confianza** y, a continuación, en **Siguiente**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  En el **especificar nombre para mostrar** página, escriba un nombre en **nombre para mostrar**, en **notas** escriba una descripción para esta relación de confianza y, a continuación, haga clic en **siguiente** .  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. En el **configurar certificado** página, si tiene un certificado de cifrado de tokens opcional, haga clic en **examinar** para buscar un archivo de certificado y, a continuación, haga clic en **siguiente**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  En el **configurar dirección URL de** página, realice una o ambas de las siguientes acciones, haga clic en **siguiente**y, a continuación, vaya al paso 8:  
  
    -   Seleccione el **habilitar la compatibilidad con WS\-protocolo pasivo de federación** casilla de verificación. En **para usuario autenticado entidad WS\-dirección URL de protocolo pasivo de federación**, escriba la dirección URL para esta relación de confianza y, a continuación, haga clic en **siguiente**.  
  
    -   Active la casilla **Habilitar compatibilidad con el protocolo SAML 2.0 WebSSO**. En **para usuario autenticado entidad dirección URL del servicio SSO de SAML 2.0**, escriba el lenguaje de marcado de aserción de seguridad \(SAML\) dirección URL del punto de conexión para esta relación de confianza del servicio y, a continuación, haga clic en **siguiente**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  En el **elegir directiva de Control de acceso** seleccione una directiva y haga clic en **siguiente**.  Para obtener más información acerca de las directivas de Control de acceso, consulte [las directivas de Control de acceso en AD FS](Access-Control-Policies-in-AD-FS.md). 
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. En la página **Listo para agregar confianza** , revisa la configuración y haz clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.  
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. En la página **Finalizar**, haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para crear un notificaciones compatible con usuarios de confianza de confianza con metadatos de federación

Para agregar una nueva entidad de confianza, utilizando el complemento de administración de AD FS, importando automáticamente los datos de configuración del asociado de metadatos de federación que el asociado publicó en una red local o a Internet, realice el procedimiento siguiente en un servidor de federación de la organización del asociado de cuenta.

>[!NOTE]
>Aunque ha sido una práctica común usar certificados con nombres de host sin calificar como https://myserver, estos certificados no tienen ningún valor de seguridad y puede permitir que un atacante suplante un servicio de federación que está publicando metadatos de federación. Por lo tanto, al consultar los metadatos de federación, debe sólo usar un nombre de dominio completo, como https://myserver.contoso.com.

El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).


1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2. En **acciones**, haga clic en **agregar confianza**.  
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. En el **bienvenida** página, elija **notificaciones** y haga clic en **iniciar**.  
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. En el **Seleccionar origen de datos** página, haga clic en <strong>importar datos acerca del usuario de confianza publicados en línea o en una red local *. En ** dirección de metadatos de federación (nombre de host o dirección URL)</strong>, escriba el nombre de host o dirección URL de metadatos de federación para el socio y, a continuación, haga clic en **siguiente**.  
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. En la página Especificar nombre para mostrar, escriba un nombre en **nombre para mostrar**en notas escriba una descripción para esta relación de confianza y, a continuación, haga clic en **siguiente**.

6. En la página Elegir reglas de autorización de emisión, seleccione **permitir todos los usuarios tener acceso a este usuario autenticado** o **denegar todos los usuarios el acceso a este usuario autenticado**y, a continuación, haga clic en **siguiente**.

7. En la página Listo para agregar confianza, revise la configuración y, a continuación, haga clic en **siguiente** para guardar su confianza información de confianza.

8. En la página de finalización, haga clic en **cerrar**. Esta acción muestra automáticamente el cuadro de diálogo Editar reglas de notificación. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta relación de confianza para usuario autenticado, consulta Referencias adicionales.




## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
