---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Creación de una confianza de proveedor de notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 07c5e642ca0198d48b4427c38d7c7bafa7cde62f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966847"
---
# <a name="create-a-claims-provider-trust"></a>Creación de una confianza de proveedor de notificaciones

Para agregar una nueva confianza de proveedor de notificaciones mediante el complemento \- de administración de AD FS en y configurar manualmente las opciones, realice el siguiente procedimiento en un servidor de Federación de asociado de recurso en la organización del asociado de recurso.  
  
La pertenencia al grupo **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para crear una confianza de proveedor de notificaciones manualmente  
  
1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.  
  
2.  En **acciones**, haga clic en **Agregar confianza de proveedor de notificaciones**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **página principal**, haz clic en **Iniciar**. 
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Escribir los datos de la confianza del proveedor de notificaciones manualmente** y, a continuación, en **Siguiente**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  En la página **Especificar nombre para mostrar**, escriba un **Nombre para mostrar**, en **Notas** escriba una descripción para esta confianza de proveedor de notificaciones y, después, haga clic en **Siguiente**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  En la página **configurar dirección URL** , especifique la **dirección URL pasiva de WS-Federation** , si procede, y haga clic en **siguiente**.
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. En la página **Configurar identificador**, en **Identificador de confianza del proveedor de notificaciones**, escriba el identificador apropiado y haga clic en **Siguiente**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. En la página **Configurar certificados**, haga clic en **Agregar** para buscar un archivo de certificado y agregarlo a la lista de certificados y, después, haga clic en **Siguiente**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. En la página **Listo para agregar confianza**, haga clic en **Siguiente** para guardar la información de la confianza de proveedor de notificaciones.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. En la página **Finalizar**, haz clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta confianza de proveedor de notificaciones, vea las siguientes referencias adicionales.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para crear una confianza de proveedor de notificaciones mediante metadatos de Federación
Para agregar una nueva confianza de proveedor de notificaciones mediante el complemento de administración de AD FS, importando automáticamente los datos de configuración sobre el socio desde los metadatos de Federación que el asociado ha publicado en una red local o en Internet, realice el siguiente procedimiento en un servidor de Federación de la organización del asociado de recurso.

>[!NOTE]
>Aunque la práctica habitual es usar certificados con nombres de host no calificados como https: \/ /MyServer, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplante una servicio de Federación que está publicando metadatos de Federación. Por lo tanto, al consultar los metadatos de Federación, solo debe usar un nombre de dominio completo como `https://myserver.contoso.com` .

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.  
  
2.  En **acciones**, haga clic en **Agregar confianza de proveedor de notificaciones**.  
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  En la **página principal**, haz clic en **Iniciar**. 
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Importar los datos acerca del proveedor de notificaciones publicados en línea o en una red local**. En dirección de metadatos de Federación (nombre de host o dirección URL), escriba la **dirección URL de metadatos de Federación** o el nombre de host del asociado y, a continuación, haga clic en **siguiente**.
![confianza de proveedor de notificaciones](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  En la página especificar nombre para mostrar, escriba un **nombre para mostrar**, en notas escriba una descripción para esta confianza del proveedor de notificaciones y, a continuación, haga clic en **siguiente**.

6.  En la página Listo para agregar confianza, haga clic en **Siguiente** para guardar la información de la confianza de proveedor de notificaciones.

7.  En la página Finalizar , haga clic en **Cerrar**. Se mostrará automáticamente el cuadro de diálogo editar reglas de notificaciones. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta confianza de proveedor de notificaciones, consulte la sección referencias adicionales a continuación.



    
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de la organización del asociado de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Consulte también  
[Operaciones de AD FS](../ad-fs-operations.md) 
  
