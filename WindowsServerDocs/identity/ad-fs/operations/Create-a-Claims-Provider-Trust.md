---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Crear una confianza de proveedor de notificaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>Crear una confianza de proveedor de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para agregar una nueva confianza proveedor de notificaciones mediante la administración de AD FS en snap\ y configurar manualmente las opciones, realiza el procedimiento siguiente en un servidor de federación de recursos asociado en la organización de partner de recurso.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para crear una confianza de proveedor de reclamaciones manualmente  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En **acciones**, haz clic en **agregar confiar en proveedor reclamaciones **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **bienvenida** página, haz clic en **inicio **. 
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la **Seleccionar origen de datos** página, haz clic en **entrar datos de confianza de proveedor de notificaciones manualmente**y, a continuación, haz clic en **siguiente **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  En la **especificar nombre para mostrar**, escriba un **nombre para mostrar**, en **notas**, escribe una descripción para esto dice confianza de proveedor y, a continuación, haz clic en **siguiente **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  En la **configurar URL** página, especifica la **dirección URL de federación de WS pasivo** si corresponde y haz clic en **siguiente **.
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. En la **configurar identificador** página, debajo **identificador de confianza de proveedor de reclamaciones**, escribe el identificador adecuado y, a continuación, haz clic en **siguiente **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. En la **configurar certificados** página, haz clic en **agregar** para buscar un archivo de certificado y agregarlo a la lista de certificados y, a continuación, haz clic en **siguiente **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. En la **listo para agregar confianza** página, haz clic en **siguiente** para guardar las reclamaciones de información del proveedor de confianza.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. En la **finalizar** página, haz clic en **cerrar**. Esta acción se muestra automáticamente el **editar reglas de notificación** cuadro de diálogo. Para obtener más información sobre cómo proceder con la adición de reglas para esta confianza de proveedor de reclamaciones de notificación, consulte las siguientes referencias adicionales.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para crear una confianza de proveedor de reclamaciones mediante metadatos de federación
Para agregar un nuevo proveedor de reclamaciones confianza, mediante el complemento de administración de AD FS, al importar automáticamente los datos de configuración sobre el socio de metadatos de federación que ha publicado el asociado a una red local o a Internet, realiza el procedimiento siguiente en un servidor de federación de la organización de partner de recurso.

>[!NOTE]
>Aunque durante cuánto tiempo ha pasado una práctica habitual usar certificados con nombres de host no calificado como https://myserver, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplantar a un servicio de federación que publica federación metadatos. Por lo tanto, al consultar los metadatos de federación, solo debes usar un nombre de dominio completo como https://myserver.contoso.com.

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En **acciones**, haz clic en **agregar confiar terceros de confianza **.  
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **bienvenida** página, haz clic en **inicio **. 
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la **Seleccionar origen de datos** página, haz clic en **importar datos sobre el proveedor de notificaciones publicación en línea o en una red local **. En federación escribe la dirección de los metadatos (nombre de host o dirección URL), el **dirección URL de metadatos de federación** o de host de nombre para el socio y, a continuación, haz clic en **siguiente **.
![Notificaciones de confianza de proveedor](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  En el nombre para mostrar especificar el tipo de página un **nombre para mostrar**, bajo notas escribe una descripción para esto dice confianza de proveedor y, a continuación, haz clic en **siguiente **.

6.  En la página Listo para agregar confianza, haz clic en **siguiente** para guardar las reclamaciones de información del proveedor de confianza.

7.  En la página de finalización, haz clic en **cerrar**. Esto mostrará automáticamente el cuadro de diálogo Editar reglas de notificación. Para obtener más información sobre cómo proceder con la adición de reclamar reglas para esta confianza de proveedor de notificaciones, consulta la sección de referencias adicionales más adelante.



    
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configuración de la organización de Partner de recursos](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 
  
