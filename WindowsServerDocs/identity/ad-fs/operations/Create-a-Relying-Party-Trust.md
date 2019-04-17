---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Crear un usuario de confianza
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>Crear un usuario de confianza

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

El siguiente documento proporciona información sobre cómo crear una confianza de terceros de confianza manualmente y uso de metadatos de federación.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para crear un reclamaciones con reconocimiento de terceros Relying confianza manualmente 

Para agregar una nueva confianza de terceros de confianza mediante la administración de AD FS en snap\ y configurar manualmente las opciones, realizar lo siguiente en un servidor de federación.  

Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En **acciones**, haz clic en **agregar confiar terceros de confianza **.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  En la **bienvenida** página, elige **reclamaciones cuenta** y haz clic en **inicio **.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  En la **Seleccionar origen de datos** página, haz clic en **escribir datos sobre el usuario de confianza manualmente**y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  En la **especificar nombre para mostrar**, escriba un nombre en **nombre para mostrar**, en **notas** escribe una descripción para esta confianza de terceros de confianza y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. En la **configurar certificado** página, si tienes un certificado de cifrado de token opcional, haz clic en **examinar** para buscar un archivo de certificado y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  En la **configurar URL** página, realiza una o ambas de las siguientes acciones, haz clic en **siguiente**y, a continuación, ve al paso 8:  
  
    -   Selecciona el **habilitar la compatibilidad con el protocolo WS\ federación pasivo** casilla de verificación. En **Relying terceros WS\ federación pasivo Protocolo URL**, escribe la dirección URL para esta confianza de terceros de confianza y, a continuación, haz clic en **siguiente**.  
  
    -   Selecciona el **habilitar la compatibilidad con el protocolo SAML 2.0 WebSSO** casilla de verificación. En **Relying terceros dirección URL del servicio SAML 2.0 SSO**, escribe la URL de extremo de lenguaje de marcado de seguridad aserción \(SAML\) servicio para esta confianza de terceros de confianza y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. En la **configurar identificadores** página, especifica uno o más identificadores para este usuario de confianza, haz clic en **agregar** para agregarlos a la lista y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  En la **elegir la directiva de Control de acceso** selecciona una directiva y haz clic en **siguiente**.  Para obtener más información acerca de las directivas de Control de acceso, consulta [directivas de Control de acceso de AD FS](Access-Control-Policies-in-AD-FS.md). 
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. En la **listo para agregar confianza** página, revisa la configuración y, a continuación, haz clic en **siguiente** para el usuario de confianza de guardar información de confianza.  
   ![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. En la **finalizar** página, haz clic en **cerrar**. Esta acción se muestra automáticamente el **editar reglas de notificación** cuadro de diálogo.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para crear un reclamaciones con reconocimiento de terceros Relying confiar en uso de metadatos de federación

Para agregar una nueva confianza confianza parte, mediante el complemento de administración de AD FS, importar automáticamente los datos de configuración sobre el socio de metadatos de federación el partner publicado en una red local o a Internet, realiza el procedimiento siguiente en un servidor de federación de la organización de partner de la cuenta.

>[!NOTE]
>Aunque durante cuánto tiempo ha pasado una práctica habitual usar certificados con nombres de host no calificado como https://myserver, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplantar a un servicio de federación que publica federación metadatos. Por lo tanto, al consultar los metadatos de federación, solo debes usar un nombre de dominio completo como https://myserver.contoso.com.

Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).


1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En **acciones**, haz clic en **agregar confiar terceros de confianza **.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  En la **bienvenida** página, elige **reclamaciones cuenta** y haz clic en **inicio **.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  En la **Seleccionar origen de datos** página, haz clic en **importar datos sobre el usuario de confianza publicación en línea o en una red local*. En **dirección de metadatos de federación (nombre de host o dirección URL)**, escribe el nombre de host o dirección URL de metadatos de federación para los partners y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  En la página Especificar nombre para mostrar, escribe un nombre en **nombre para mostrar**, bajo notas escribe una descripción para esta confianza de terceros de confianza y, a continuación, haz clic en **siguiente**.

6.  En la página de elegir las reglas de autorización de emisión, seleccione **permitir que todos los usuarios para acceder a este usuario de confianza** o **denegar todos los usuarios el acceso a este usuario de confianza**y, a continuación, haz clic en **siguiente**.

7.  En la página Listo para agregar confianza, revisa la configuración y, a continuación, haz clic en **siguiente** para el usuario de confianza de guardar información de confianza.

8.  En la página de finalización, haz clic en **cerrar**. Esta acción muestra automáticamente el cuadro de diálogo Editar reglas de notificación. Para obtener más información sobre cómo proceder con la adición de reglas de notificación para esta confianza de terceros de confianza, consulta las referencias adicionales.




## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 
