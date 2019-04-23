---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Implementación de notificaciones entre bosques (pasos de demostración)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830356"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implementación de notificaciones entre bosques (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema, hablaremos sobre un escenario básico que se explica cómo configurar transformaciones de notificaciones entre bosques que confían y de confianza. Obtendrá información sobre cómo se pueden crear objetos de directiva de transformación de notificaciones y vinculados a la relación de confianza en el bosque que confía y el bosque de confianza. A continuación, validará el escenario.  
  
## <a name="scenario-overview"></a>Introducción a los escenarios  
Adatum Corporation proporciona servicios financieros a Contoso, Ltd. Cada trimestre, Adatum contables copiar sus hojas de cálculo de la cuenta en una carpeta en un servidor de archivos ubicado en Contoso, Ltd. Hay una confianza bidireccional configurado desde Contoso y Adatum. Contoso, Ltd. desea proteger el recurso compartido para que solo los empleados de Adatum pueden tener acceso el recurso compartido remoto.  
  
En este escenario:  
  
1.  [Configurar los requisitos previos y el entorno de prueba](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configuración de transformación de notificaciones en el bosque de confianza (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configuración de transformación de notificaciones en el bosque que confía (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Validar el escenario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurar los requisitos previos y el entorno de prueba  
La configuración de pruebas implica configurar dos bosques: Adatum Corporation y Contoso, Ltd y tener una confianza bidireccional entre Contoso y Adatum. "adatum.com" es el bosque de confianza y "contoso.com" es el bosque que confía.  
  
El escenario de transformación de notificaciones muestra la transformación de una notificación en el bosque de confianza a una notificación en el bosque que confía. Para ello, deberá configurar un nuevo bosque llamado adatum.com y rellenar el bosque con un usuario de prueba con un valor de la empresa de «Adatum». A continuación, debe establecer una confianza bidireccional entre contoso.com y adatum.com.  
  
> [!IMPORTANT]  
> Al configurar los bosques de Contoso y Adatum, debe asegurarse de que los dominios raíz están en Windows Server 2012 nivel funcional del dominio para la transformación de notificaciones para que funcione.  
  
Debe configurar lo siguiente para el laboratorio. Estos procedimientos se explican con detalle en [Apéndice B: Configurar el entorno de prueba](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
Deberá implementar los siguientes procedimientos para configurar el laboratorio para este escenario:  
  
1.  [Establecer Adatum como bosque de confianza a Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Crear el tipo de notificación 'Company' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Habilitar la propiedad 'Company' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Crear la regla de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Crear la directiva de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Publicar la nueva directiva mediante Directiva de grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Cree la carpeta Earnings en el servidor de archivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Establecer la clasificación y aplicar la directiva de acceso central en la nueva carpeta](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Use la siguiente información para completar este escenario:  
  
|Objects|Detalles|  
|-----------|-----------|  
|Usuarios|Jeff Low, Contoso|  
|Notificaciones de usuario de Contoso y Adatum|Id.: ad: / / ext/ContosoAdatum: empresa,<br /><br />Atributo de origen: empresa<br /><br />Valores sugeridos: Contoso, Adatum **importantes:** Debe establecer el identificador de la sección 'Company' tipo de notificación en Contoso y Adatum sea el mismo para la transformación de notificaciones para que funcione.|  
|Regla de acceso central en Contoso|AdatumEmployeeAccessRule|  
|Directiva de acceso central de Contoso|Directiva de acceso única de Adatum|  
|Directivas de transformación de notificaciones en Contoso y Adatum|Empresa DenyAllExcept|  
|Carpeta de archivos en Contoso|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>Configuración de transformación de notificaciones en el bosque de confianza (Adatum)  
En este paso creará una directiva de transformación en Adatum para denegar todas las notificaciones excepto 'Company' para pasar a Contoso.  
  
El módulo de Active Directory para Windows PowerShell proporciona el **DenyAllExcept** argumento, que quita todo, excepto las notificaciones especificadas en la directiva de transformación.  
  
Para configurar una transformación de notificaciones, deberá crear una directiva de transformación de notificaciones y vincularlo entre los bosques de confianza y que confían.  
  
### <a name="BKMK_2.2"></a>Crear una directiva de transformación de notificaciones en Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para crear una directiva de transformación Adatum para denegar todas las notificaciones excepto 'Company'  
  
1.  Inicie sesión en el controlador de dominio adatum.com como administrador con la contraseña **pass@word1**.  
  
2.  Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Establecer un vínculo de transformación de notificaciones en el objeto de dominio de confianza de Adatum  
En este paso, aplicar la directiva de transformación de notificaciones recién creado en el objeto de dominio de confianza de Adatum para Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar la directiva de transformación de notificaciones  
  
1.  Inicie sesión en el controlador de dominio adatum.com como administrador con la contraseña **pass@word1**.  
  
2.  Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configuración de transformación de notificaciones en el bosque que confía (Contoso)  
En este paso se crea una directiva de transformación de notificaciones en Contoso (el bosque que confía) para denegar todas las notificaciones excepto 'Company'. Deberá crear una directiva de transformación de notificaciones y vincularla a la confianza de bosque.  
  
### <a name="BKMK_4.1"></a>Crear una directiva de transformación de notificaciones en Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para crear una directiva de transformación Adatum para denegar todo excepto 'Company'  
  
1.  Inicie sesión en el controlador de dominio, contoso.com como administrador con la contraseña **pass@word1**.  
  
2.  Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Establecer un vínculo de transformación de notificaciones en el objeto de confianza de dominio de Contoso  
En este paso, aplicar el recién creado directivas de transformación de notificaciones en el objeto de dominio de contoso.com confianza para Adatum permitir "Company" debe pasarse a contoso.com. El objeto de dominio de confianza se denomina adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Para establecer directivas de transformación de las notificaciones  
  
1.  Inicie sesión en el controlador de dominio, contoso.com como administrador con la contraseña **pass@word1**.  
  
2.  Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Validar el escenario  
En este paso intenta tener acceso a la carpeta D:\EARNINGS que se haya configurado en el servidor de archivos FILE1 para validar que el usuario tiene acceso a la carpeta compartida.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para asegurarse de que el usuario Adatum puede tener acceso a la carpeta compartida  
  
1.  Inicie sesión en el cliente de equipo, CLIENT1 como Jeff Low con la contraseña **pass@word1**.  
  
2.  Vaya a la carpeta \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low debería poder acceder a la carpeta.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Escenarios adicionales para directivas de transformación de notificaciones  
Siguiente es una lista de los casos comunes adicionales en la transformación de notificaciones.  
  
|Escenario|Directiva|  
|------------|----------|  
|Permitir todas las notificaciones que proceden de Adatum recorrer a Contoso Adatum|Código: <br />Nuevo ADClaimTransformPolicy \`<br /> -Description: "Directiva de notificaciones de transformación para permitir que todas las notificaciones" \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole: confiar en \`<br />-Server:"contoso.com" `|  
|Denegar todas las notificaciones que proceden de Adatum recorrer a Contoso Adatum|Código: <br />Nuevo ADClaimTransformPolicy \`<br />-Description: "Directiva de notificaciones de transformación para denegar todas las notificaciones" \`<br />-Name: "DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy: "DenyAllClaimsPolicy" \`<br />-TrustRole: confiar en \`<br />-Server:"contoso.com"`|  
|Permitir todas las notificaciones que proceden de Adatum excepto "Compañía" y "Department" para ir a través a Contoso Adatum|Código <br />Nueva-ADClaimTransformationPolicy \`<br />-Description: "Directiva de notificaciones de transformación para permitir que todas las notificaciones, excepto la compañía y el departamento" \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept: organización, departamento \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole: confiar en \`<br />-Server:"contoso.com" `|  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   Para obtener una lista de todos los cmdlets de Windows PowerShell que están disponibles para la transformación de notificaciones, consulte [referencia de Cmdlet de PowerShell de Active Directory](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   Para tareas avanzadas que implican la exportación e importación DAC de información de configuración entre dos bosques, use el [referencia de PowerShell de Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Implementar notificaciones en bosques](Deploy-Claims-Across-Forests.md)  
  
-   [Lenguaje de reglas de transformación de notificaciones](Claims-Transformation-Rules-Language.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

