---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Creación de una confianza de proveedor de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1c47986cda3f091033274aa2c59a656ec861a98f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189715"
---
# <a name="create-a-claims-provider-trust"></a>Creación de una confianza de proveedor de notificaciones

Para agregar una nueva confianza de proveedor de notificaciones utilizando el complemento Administración de AD FS\-en y manualmente la configuración, lleve a cabo el siguiente procedimiento en un servidor de federación del asociado de recurso en la organización del asociado de recurso.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para crear una confianza de proveedor de notificaciones manualmente  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En **acciones**, haga clic en **agregar confianza del proveedor de notificaciones**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **página principal**, haga clic en **Iniciar**. 
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Escribir los datos de la confianza del proveedor de notificaciones manualmente** y, a continuación, en **Siguiente**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  En la página **Especificar nombre para mostrar**, escriba un **Nombre para mostrar**, en **Notas** escriba una descripción para esta confianza de proveedor de notificaciones y, después, haga clic en **Siguiente**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  En el **configurar dirección URL de** , especifique el **URL WS-Federation Passive** si es aplicable y haga clic en **siguiente**.
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. En la página **Configurar identificador**, en **Identificador de confianza del proveedor de notificaciones**, escriba el identificador apropiado y haga clic en **Siguiente**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. En la página **Configurar certificados**, haga clic en **Agregar** para buscar un archivo de certificado y agregarlo a la lista de certificados y, después, haga clic en **Siguiente**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. En la página **Listo para agregar confianza**, haga clic en **Siguiente** para guardar la información de la confianza de proveedor de notificaciones.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. En la página **Finalizar**, haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**. Para obtener más información acerca de cómo continuar agregando reglas de notificaciones para esta relación de confianza de proveedor de notificaciones, consulte las siguientes referencias adicionales.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para crear una confianza de proveedor de notificaciones mediante metadatos de federación
Para agregar una nueva confianza de proveedor de notificaciones, mediante el complemento de administración de AD FS, importando automáticamente los datos de configuración del asociado de metadatos de federación que el asociado ha publicado en una red local o a Internet, lleve a cabo el procedimiento siguiente en un servidor de federación de la organización del asociado de recurso.

>[!NOTE]
>Aunque ha sido una práctica común usar certificados con nombres de host sin calificar como https://myserver, estos certificados no tienen ningún valor de seguridad y puede permitir que un atacante suplante un servicio de federación que está publicando metadatos de federación. Por lo tanto, al consultar los metadatos de federación, debe sólo usar un nombre de dominio completo, como https://myserver.contoso.com.

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En **acciones**, haga clic en **agregar confianza del proveedor de notificaciones**.  
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **página principal**, haga clic en **Iniciar**. 
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Importar los datos acerca del proveedor de notificaciones publicados en línea o en una red local**. En la federación, dirección de metadatos (nombre de host o dirección URL), escriba el **dirección URL de metadatos de federación** o host que asigne un nombre para el socio y, a continuación, haga clic en **siguiente**.
![Confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Tipo de página en el nombre para mostrar especificar un **nombre para mostrar**en notas, escriba una descripción para esta confianza de proveedor de notificaciones y, a continuación, haga clic en **siguiente**.

6.  En la página Listo para agregar confianza, haga clic en **siguiente** para guardar información del proveedor de confianza de sus notificaciones.

7.  En la página de finalización, haga clic en **cerrar**. Esto mostrará automáticamente el cuadro de diálogo Editar reglas de notificación. Para obtener más información acerca de cómo continuar agregando reglas de notificaciones para esta relación de confianza de proveedor de notificaciones, consulte la sección referencias adicionales a continuación.



    
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configuración de la organización del asociado de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
