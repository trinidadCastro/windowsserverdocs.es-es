---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Creación de una confianza de proveedor de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4539e8abd1af1eca7bacb51971e6d355bb0aab28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407657"
---
# <a name="create-a-claims-provider-trust"></a>Creación de una confianza de proveedor de notificaciones

Para agregar una nueva confianza de proveedor de notificaciones mediante el no__t de administración de AD FS @ 0in y configurar manualmente los valores, realice el siguiente procedimiento en un servidor de Federación de asociado de recurso en la organización del asociado de recurso.  
  
La pertenencia al grupo **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para crear una confianza de proveedor de notificaciones manualmente  
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En **acciones**, haga clic en **Agregar confianza de proveedor de notificaciones**.  
![claims proveedor de confianza @ no__t-1   
  
3.  En la **página principal**, haga clic en **Iniciar**. 
![claims proveedor de confianza @ no__t-1    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Escribir los datos de la confianza del proveedor de notificaciones manualmente** y, a continuación, en **Siguiente**.  
![claims proveedor de confianza @ no__t-1     

5.  En la página **Especificar nombre para mostrar**, escriba un **Nombre para mostrar**, en **Notas** escriba una descripción para esta confianza de proveedor de notificaciones y, después, haga clic en **Siguiente**.  
![claims proveedor de confianza @ no__t-1     

6.  En la página **configurar dirección URL** , especifique la **dirección URL pasiva de WS-Federation** , si procede, y haga clic en **siguiente**.
![claims proveedor de confianza @ no__t-1     

8. En la página **Configurar identificador**, en **Identificador de confianza del proveedor de notificaciones**, escriba el identificador apropiado y haga clic en **Siguiente**.  
![claims proveedor de confianza @ no__t-1    

9. En la página **Configurar certificados**, haga clic en **Agregar** para buscar un archivo de certificado y agregarlo a la lista de certificados y, después, haga clic en **Siguiente**.  
![claims proveedor de confianza @ no__t-1    

10. En la página **Listo para agregar confianza**, haga clic en **Siguiente** para guardar la información de la confianza de proveedor de notificaciones.  
![claims proveedor de confianza @ no__t-1    

11. En la página **Finalizar**, haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta confianza de proveedor de notificaciones, vea las siguientes referencias adicionales.  
![claims proveedor de confianza @ no__t-1

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para crear una confianza de proveedor de notificaciones mediante metadatos de Federación
Para agregar una nueva confianza de proveedor de notificaciones mediante el complemento de administración de AD FS, importando automáticamente los datos de configuración sobre el socio desde los metadatos de Federación que el asociado ha publicado en una red local o en Internet, realice el siguiente procedimiento en un servidor de Federación en la organización del asociado de recurso.

>[!NOTE]
>Aunque la práctica habitual es usar certificados con nombres de host sin calificar, como https: \//-Server, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplante un Servicio de federación que está publicando Federación repositorio. Por lo tanto, al consultar los metadatos de Federación, solo debe usar un nombre de dominio completo como `https://myserver.contoso.com`.

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En **acciones**, haga clic en **Agregar confianza de proveedor de notificaciones**.  
![claims proveedor de confianza @ no__t-1   
  
3.  En la **página principal**, haga clic en **Iniciar**. 
![claims proveedor de confianza @ no__t-1    
  
4.  En la página **Seleccionar origen de datos**, haga clic en **Importar los datos acerca del proveedor de notificaciones publicados en línea o en una red local**. En dirección de metadatos de Federación (nombre de host o dirección URL), escriba la **dirección URL de metadatos de Federación** o el nombre de host del asociado y, a continuación, haga clic en **siguiente**.
![claims proveedor de confianza @ no__t-1    

5.  En la página especificar nombre para mostrar, escriba un **nombre para mostrar**, en notas escriba una descripción para esta confianza del proveedor de notificaciones y, a continuación, haga clic en **siguiente**.

6.  En la página listo para agregar confianza, haga clic en **siguiente** para guardar la información de confianza del proveedor de notificaciones.

7.  En la página finalizar, haga clic en **cerrar**. Se mostrará automáticamente el cuadro de diálogo editar reglas de notificaciones. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta confianza de proveedor de notificaciones, consulte la sección referencias adicionales a continuación.



    
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configuración de la organización del asociado de recurso @ no__t-0  
  
[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
