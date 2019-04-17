---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: "Implementar notificaciones entre bosques (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implementar notificaciones entre bosques (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema, vamos a explicar un escenario básico que se explica cómo configurar notificaciones transformaciones entre bosques de confianza y que confían. Aprenderás cómo crear objetos de directiva de transformación de notificaciones y vinculados a la confianza en el bosque de confianza y en el bosque de confianza. A continuación, se validará el escenario.  
  
## <a name="scenario-overview"></a>Información general del escenario  
Corporation Adatum proporciona servicios financieros a Contoso, Ltd. Cada trimestre, Adatum contadores copia las hojas de cálculo de la cuenta en una carpeta en un servidor de archivos que se encuentra en Contoso, Ltd. Hay una confianza bidireccional configure de Contoso a Adatum. Contoso, Ltd. quiere proteger el recurso compartido para que solo Adatum los empleados puedan acceder el recurso compartido remoto.  
  
En este escenario:  
  
1.  [Configurar los requisitos previos y el entorno de prueba](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configurar la transformación de solicitudes en el bosque de confianza (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configurar la transformación de solicitudes en el bosque de confianza (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Validar el escenario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurar los requisitos previos y el entorno de prueba  
La configuración de prueba implica configurar dos bosques: Adatum Corporation y Contoso, Ltd. y tener una confianza bidireccional entre Contoso y Adatum. "adatum.com" es el bosque de confianza y "contoso.com" es el bosque de confianza.  
  
El escenario de transformación de notificaciones muestra la transformación de una notificación en el bosque de confianza a una notificación en el bosque de confianza. Para ello, debes configurar un bosque nuevo llamado adatum.com y rellenar del bosque con un usuario de prueba con un valor de la empresa de 'Adatum'. A continuación, tienes que configurar una confianza bidireccional entre contoso.com y adatum.com.  
  
> [!IMPORTANT]  
> Al configurar los bosques de Contoso y Adatum, debes asegurarte de que ambos dominios raíz se encuentran en Windows Server 2012 nivel funcional del dominio de transformación de notificaciones para que funcione.  
  
Debes configurar los siguientes datos de la práctica. Estos procedimientos se explican en detalle en [Apéndice B: configuración la prueba entorno](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
Debes implementar los siguientes procedimientos para configurar el laboratorio para este escenario:  
  
1.  [Establecer Adatum como bosque de confianza para Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Crear el tipo de notificación 'Company' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Habilitar la propiedad de recursos 'Company' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Crear la regla de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Crear la directiva de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Publicar la nueva directiva mediante la directiva de grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Crear la carpeta de ganancias en el servidor de archivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Establecer clasificación y aplica la directiva de acceso central en la carpeta nueva](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Usa la siguiente información para completar este escenario:  
  
|Objetos|Detalles|  
|-----------|-----------|  
|Usuarios|Jeff bajo, Contoso|  
|Notificaciones de usuario en Adatum y Contoso|Identificador: anuncio: / / ext/empresa: ContosoAdatum,<br /><br />Atributo de origen: empresa<br /><br />Valores sugeridos: Contoso, Adatum **importante:** debe establecer el Id. de la empresa' ' tipo de notificación en Contoso y Adatum a ser el mismo para la transformación de notificaciones para que funcione.|  
|Regla de acceso central en Contoso|AdatumEmployeeAccessRule|  
|Directiva de acceso central en Contoso|Directiva de acceso solo adatum|  
|Directivas de la transformación de reclamaciones en Adatum y Contoso|DenyAllExcept empresa|  
|Carpeta de archivos en Contoso|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>Configurar la transformación de solicitudes en el bosque de confianza (Adatum)  
En este paso se crea una directiva de la transformación en Adatum denegar todas las reclamaciones excepto 'Company' para pasar a Contoso.  
  
El módulo de Active Directory para Windows PowerShell proporciona la **DenyAllExcept** argumento, que elimina todo excepto las notificaciones especificadas en la directiva de transformación.  
  
Para configurar una transformación de notificaciones, deberás crear una directiva de transformación de notificaciones y vincularla entre los bosques de confianza y de confianza.  
  
### <a name="BKMK_2.2"></a>Crear una directiva de transformación de reclamaciones en Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para crear una directiva de transformación Adatum denegar todas las reclamaciones excepto 'Company'  
  
1.  Inicia sesión en el controlador de dominio, adatum.com como administrador con la contraseña **pass@word1**.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y escribe lo siguiente:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Establecer un vínculo de transformación de notificaciones en el objeto de dominio de confianza de Adatum  
En este paso, aplicar la directiva de transformación reclamaciones recién creado en el objeto de dominio de confianza de Adatum de Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar la directiva de transformación de notificaciones  
  
1.  Inicia sesión en el controlador de dominio, adatum.com como administrador con la contraseña **pass@word1**.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y escribe lo siguiente:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configurar la transformación de solicitudes en el bosque de confianza (Contoso)  
En este paso crear una directiva de transformación reclamaciones en Contoso (el bosque de confianza) para denegar todas las reclamaciones excepto 'Company'. Debes crear una directiva de transformación de notificaciones y vincularla con la confianza de bosque.  
  
### <a name="BKMK_4.1"></a>Crear una directiva de transformación de reclamaciones de Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para crear una directiva de transformación Adatum denegar todo excepto 'Company'  
  
1.  Inicia sesión en el controlador de dominio, contoso.com como administrador con la contraseña **pass@word1**.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y escribe lo siguiente:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Establecer un vínculo de transformación de notificaciones en el objeto de dominio de confianza de Contoso  
En este paso, aplicas recién creado directiva de transformación de notificaciones en el objeto de dominio de confianza contoso.com para Adatum permitir que "Empresa" se pasa a contoso.com. El objeto de dominio de confianza se denomina adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Para establecer las notificaciones de la directiva de transformación  
  
1.  Inicia sesión en el controlador de dominio, contoso.com como administrador con la contraseña **pass@word1**.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y escribe lo siguiente:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Validar el escenario  
En este paso, intenta obtener acceso a la carpeta D:\EARNINGS que se haya configurado en el servidor de archivos archivo1 para validar que el usuario tiene acceso a la carpeta compartida.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para asegurarse de que el usuario Adatum puede tener acceso a la carpeta compartida  
  
1.  Inicia sesión en el equipo cliente, CLIENTE1 como Jeff Low con la contraseña **pass@word1**.  
  
2.  Busca la carpeta \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low debe poder acceder a la carpeta.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Escenarios adicionales para las directivas de transformación de notificaciones  
Siguiente es una lista de los casos comunes adicionales en la transformación de solicitudes.  
  
|Escenario|Directiva|  
|------------|----------|  
|Permitir todas las reclamaciones que provienen de Adatum para llevar a cabo para Contoso Adatum|Código: <br />New-ADClaimTransformPolicy \'<br /> -Descripción: "Directiva de transformación para permitir todas las reclamaciones que dice" \'<br />-Nombre: "AllowAllClaimsPolicy" \'<br />-PermitirTodas \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br />-Identidad: "adatum.com" \'<br />-Policy: "AllowAllClaimsPolicy" \'<br />-TrustRole: confiar en \'<br />-Server: "contoso.com" '|  
|Denegar todas las reclamaciones que provienen de Adatum para llevar a cabo para Contoso Adatum|Código: <br />New-ADClaimTransformPolicy \'<br />-Descripción: "Directiva de transformación para denegar todas las reclamaciones que dice" \'<br />-Nombre: "DenyAllClaimsPolicy" \'<br /> -DenyAll \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br />-Identidad: "adatum.com" \'<br />-Policy: "DenyAllClaimsPolicy" \'<br />-TrustRole: confiar en \'<br />-Server: "contoso.com" '|  
|Permitir todas las reclamaciones que provienen de Adatum excepto "Compañía" y "Departamento" para llevar a cabo para Contoso Adatum|Código <br />-New-ADClaimTransformationPolicy \'<br />-Descripción: "Directiva de transformación para permitir todas las reclamaciones excepto la compañía y el departamento que dice" \'<br /> -Nombre: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \'<br />-AllowAllExcept: empresa, departamento \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br /> -Identidad: "adatum.com" \'<br />-Policy: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \'<br /> -TrustRole: confiar en \'<br />-Server: "contoso.com" '|  
  
## <a name="BKMK_Links"></a>Consulta también  
  
-   Para obtener una lista de todos los cmdlets de Windows PowerShell que están disponibles para la transformación de notificaciones, consulta [referencia de Cmdlet de PowerShell de Active Directory ](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   Para tareas avanzadas que implican de exportación e importación de información de configuración de DAC entre dos bosques, usa el [referencia de PowerShell de Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Implementar notificaciones entre bosques](Deploy-Claims-Across-Forests.md)  
  
-   [Lenguaje de las reglas de transformación de notificaciones](Claims-Transformation-Rules-Language.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

