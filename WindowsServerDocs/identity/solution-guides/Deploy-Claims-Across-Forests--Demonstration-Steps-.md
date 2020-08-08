---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Implementación de notificaciones entre bosques (pasos de demostración)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 828b7256854f9d2fd6d58567c773d4abc288cd0a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952877"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implementación de notificaciones entre bosques (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema, trataremos un escenario básico en el que se explica cómo configurar transformaciones de notificaciones entre bosques de confianza y de confianza. Aprenderá cómo se pueden crear y vincular objetos de directiva de transformación de notificaciones a la confianza en el bosque que confía y el bosque de confianza. A continuación, se validará el escenario.

## <a name="scenario-overview"></a>Información general de escenario
Adatum Corporation proporciona servicios financieros a Contoso, Ltd. Cada trimestre, los contables de Adatum copian sus hojas de cálculo de cuentas en una carpeta de un servidor de archivos ubicado en Contoso, Ltd. Hay una relación de confianza bidireccional establecida entre contoso y adatum. Contoso, Ltd. desea proteger el recurso compartido para que solo los empleados de Adatum puedan tener acceso al recurso compartido remoto.

En este escenario:

1.  [Configurar los requisitos previos y el entorno de prueba](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)

2.  [Configuración de la transformación de notificaciones en un bosque de confianza (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)

3.  [Configuración de la transformación de notificaciones en el bosque que confía (contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)

4.  [Validar el escenario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>Configurar los requisitos previos y el entorno de prueba
La configuración de prueba implica la configuración de dos bosques: Adatum Corporation y Contoso, Ltd, y que tienen una confianza bidireccional entre contoso y adatum. "adatum.com" es el bosque de confianza y "contoso.com" es el bosque que confía.

El escenario de transformación de notificaciones muestra la transformación de una notificación en el bosque de confianza a una notificación en el bosque que confía. Para ello, debe configurar un nuevo bosque llamado adatum.com y rellenar el bosque con un usuario de prueba con un valor de empresa de ' Adatum '. A continuación, debe configurar una relación de confianza bidireccional entre contoso.com y adatum.com.

> [!IMPORTANT]
> Al configurar los bosques contoso y Adatum, debe asegurarse de que ambos dominios raíz estén en el nivel funcional del dominio de Windows Server 2012 para que funcione la transformación de notificaciones.

Debe configurar lo siguiente para el laboratorio. Estos procedimientos se explican en detalle en [el Apéndice B: configurar el entorno de prueba.](Appendix-B--Setting-Up-the-Test-Environment.md)

Debe implementar los siguientes procedimientos para configurar el laboratorio para este escenario:

1.  [Establecer Adatum como bosque de confianza en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)

2.  [Crear el tipo de notificaciones ' Company ' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)

3.  [Habilitar la propiedad de recurso ' Company ' en Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)

4.  [Crear la regla de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)

5.  [Crear la directiva de acceso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)

6.  [Publicar la nueva directiva mediante directiva de grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)

7.  [Crear la carpeta Earnings en el servidor de archivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)

8.  [Establecer la clasificación y aplicar la Directiva de acceso central en la nueva carpeta](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)

Use la siguiente información para completar este escenario:

|Objetos|Detalles|
|-----------|-----------|
|Usuarios|Jeff Low, contoso|
|Notificaciones de usuario en adatum y contoso|ID: ad://ext/Company:ContosoAdatum,<p>Atributo de origen: compañía<p>Valores sugeridos: Contoso, Adatum **importante:** debe establecer el identificador en el tipo de notificación ' compañía ' en Contoso y Adatum para que sea el mismo para que funcione la transformación de notificaciones.|
|Regla de acceso central en Contoso|AdatumEmployeeAccessRule|
|Directiva de acceso central en Contoso|Directiva de acceso de solo Adatum|
|Directivas de transformación de notificaciones en adatum y contoso|Empresa DenyAllExcept|
|Carpeta de archivos de Contoso|D:\EARNINGS|

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>Configuración de la transformación de notificaciones en un bosque de confianza (Adatum)
En este paso, creará una directiva de transformación en adatum para denegar todas las notificaciones excepto ' Company ' para que pasen a contoso.

El módulo de Active Directory para Windows PowerShell proporciona el argumento **DenyAllExcept** , que quita todo excepto las notificaciones especificadas en la Directiva de transformación.

Para configurar una transformación de notificaciones, debe crear una directiva de transformación de notificaciones y vincularla entre los bosques de confianza y de confianza.

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>Creación de una directiva de transformación de notificaciones en adatum

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para crear una directiva de transformación Adatum para denegar todas las notificaciones excepto ' Company '

1. Inicie sesión en el controlador de dominio, adatum.com como administrador con la contraseña <strong>pass@word1</strong> .

2. Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except Company"`
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"adatum.com" `

   ```

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>Establecer un vínculo de transformación de notificaciones en el objeto de dominio de confianza de Adatum
En este paso, se aplica la Directiva de transformación de notificaciones recién creada en el objeto de dominio de confianza de Adatum para contoso.

##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar la Directiva de transformación de notificaciones

1. Inicie sesión en el controlador de dominio, adatum.com como administrador con la contraseña <strong>pass@word1</strong> .

2. Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:

   ```

     Set-ADClaimTransformLink `
   -Identity:"contoso.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   '"TrustRole:Trusted `

   ```

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>Configuración de la transformación de notificaciones en el bosque que confía (contoso)
En este paso creará una directiva de transformación de notificaciones en Contoso (el bosque que confía) para denegar todas las notificaciones excepto ' Company '. Debe crear una directiva de transformación de notificaciones y vincularla a la confianza de bosque.

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>Creación de una directiva de transformación de notificaciones en Contoso

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para crear una directiva de transformación Adatum para denegar todo excepto ' Company '

1. Inicie sesión en el controlador de dominio, contoso.com como administrador con la contraseña <strong>pass@word1</strong> .

2. Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except company" `
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"contoso.com" `

   ```

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>Establecer un vínculo de transformación de notificaciones en el objeto de dominio de confianza de Contoso
En este paso, se aplica la Directiva de transformación de notificaciones recién creada en el objeto de dominio de confianza contoso.com para Adatum para permitir que "Company" pase a contoso.com. El objeto de dominio de confianza se denomina adatum.com.

##### <a name="to-set-the-claims-transformation-policy"></a>Para establecer la Directiva de transformación de notificaciones

1. Inicie sesión en el controlador de dominio, contoso.com como administrador con la contraseña <strong>pass@word1</strong> .

2. Abra un símbolo del sistema con privilegios elevados en Windows PowerShell y escriba lo siguiente:

   ```

     Set-ADClaimTransformLink
   -Identity:"adatum.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   -TrustRole:Trusting `

   ```

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>Validar el escenario
En este paso, intentará obtener acceso a la carpeta D:\EARNINGS que se configuró en el ARCHIVO1 del servidor de archivos para validar que el usuario tiene acceso a la carpeta compartida.

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para asegurarse de que el usuario de Adatum puede tener acceso a la carpeta compartida

1. Inicie sesión en el equipo cliente, CLIENT1 como Juan bajo con la contraseña <strong>pass@word1</strong> .

2. Vaya a la carpeta \\ \FILE1.contoso.com\Earnings.

3. Jeff Low debe poder tener acceso a la carpeta.

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Escenarios adicionales para las directivas de transformación de notificaciones
A continuación se muestra una lista de casos comunes adicionales en la transformación de notificaciones.


|                                                 Escenario                                                 |                                                                                                                                                                                                                                           Directiva                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Permitir que todas las notificaciones que proceden de Adatum pasen a contoso Adatum                  |                                                          Codifica <br />New-ADClaimTransformPolicy\`<br /> -Description: "Directiva de transformación de notificaciones para permitir todas las notificaciones"\`<br />-Name: "AllowAllClaimsPolicy"\`<br />-AllowAll\`<br />-Server:"contoso. com"\`<br />Set-ADClaimTransformLink\`<br />-Identity:"adatum. com"\`<br />-Policy: "AllowAllClaimsPolicy"\`<br />-TrustRole: confiar en\`<br />-Server:"contoso. com"\`                                                          |
|                  Denegar todas las notificaciones que proceden de Adatum a pasar a contoso Adatum                   |                                                            Codifica <br />New-ADClaimTransformPolicy\`<br />-Description: "Directiva de transformación de notificaciones para denegar todas las notificaciones"\`<br />-Name: "DenyAllClaimsPolicy"\`<br /> -DenyAll\`<br />-Server:"contoso. com"\`<br />Set-ADClaimTransformLink\`<br />-Identity:"adatum. com"\`<br />-Policy: "DenyAllClaimsPolicy"\`<br />-TrustRole: confiar en\`<br />-Server:"contoso. com"\`                                                             |
| Permitir todas las notificaciones que proceden de Adatum, excepto "compañía" y "Departamento", para pasar a contoso Adatum | Código <br />-New-ADClaimTransformationPolicy\`<br />-Description: "Directiva de transformación de notificaciones para permitir todas las notificaciones excepto la empresa y el Departamento"\`<br /> -Name: "AllowAllClaimsExceptCompanyAndDepartmentPolicy"\`<br />-AllowAllExcept: compañía, Departamento\`<br />-Server:"contoso. com"\`<br />Set-ADClaimTransformLink\`<br /> -Identity:"adatum. com"\`<br />-Policy: "AllowAllClaimsExceptCompanyAndDepartmentPolicy"\`<br /> -TrustRole: confiar en\`<br />-Server:"contoso. com"\` |

## <a name="see-also"></a><a name="BKMK_Links"></a>Vea también

-   Para obtener una lista de todos los cmdlets de Windows PowerShell que están disponibles para la transformación de notificaciones, vea [Active Directory referencia de cmdlets de PowerShell](https://go.microsoft.com/fwlink/?LinkId=243150).

-   Para las tareas avanzadas que implican la exportación e importación de información de configuración de DAC entre dos bosques, use la [referencia de PowerShell de Access Control dinámicas](https://go.microsoft.com/fwlink/?LinkId=243150) .

-   [Implementación de notificaciones en bosques](Deploy-Claims-Across-Forests.md)

-   [Lenguaje de reglas de transformación de notificaciones](Claims-Transformation-Rules-Language.md)

-   [Control de acceso dinámico: Información general sobre el escenario](Dynamic-Access-Control--Scenario-Overview.md)


