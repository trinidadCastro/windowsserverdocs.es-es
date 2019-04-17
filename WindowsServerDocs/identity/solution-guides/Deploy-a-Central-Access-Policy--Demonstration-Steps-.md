---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: "Implementar una directiva de acceso Central (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Implementar una directiva de acceso Central (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este escenario, las operaciones de seguridad del departamento de finanzas está trabajando con la seguridad de la información central para especificar la necesidad de una directiva de acceso central para que se puede proteger información de finanzas archivada almacenada en servidores de archivos. El archivado finanzas de cada país puede tener acceso a información como de solo lectura empleados Finanzas desde el mismo país. Un grupo de administración de finanzas central puede acceder a la información financiera de todos los países.  
  
Implementar una directiva de acceso central incluye las siguientes fases:  
  
|Fase|Descripción  
|---------|---------------  
|[Plan: Identificar la necesidad de directiva y la configuración necesaria para la implementación](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identificar la necesidad de una directiva y la configuración necesaria para la implementación. 
|[Implementar: Configurar los componentes y la directiva](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configurar los componentes y la directiva.  
|[Implementar la directiva de acceso central](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Implementar la directiva.  
|[Mantener: Cambiar y organizar la directiva](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Cambios en la directiva y almacenamiento provisional. 
  
## <a name="BKMK_1.1"></a>Configurar un entorno de prueba  
Antes de comenzar, debes configurar el laboratorio para probar este escenario. Los pasos para configurar el laboratorio se explican en detalle en [Apéndice B: configuración la prueba entorno ](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Plan: Identificar la necesidad de directiva y la configuración necesaria para la implementación  
Esta sección proporciona la serie de pasos que ayudan a la fase de planificación de la implementación de alto nivel.  
  
||Paso|Ejemplo|  
|-|--------|-----------|  
|1.1|Empresas determina que se necesita una directiva de acceso central|Para proteger la información financiera que se almacena en servidores de archivos, las operaciones de seguridad del departamento de finanzas está trabajando con la seguridad de la información central para especificar la necesidad de una directiva de acceso central.|  
|1.2|Express la directiva de acceso|Documentos de finanzas solo deben ser leídas por miembros del departamento de finanzas. Los miembros del departamento de finanzas solo deben acceder a los documentos en su propio país. Solo los administradores de finanzas deben tener acceso de escritura. Para los miembros del grupo FinanceException se permitirá una excepción. Este grupo tendrá acceso de lectura.|  
|1.3|Construcciones de la directiva de acceso en Windows Server 2012 Express|Selección de destino:<br /><br />-Resource.Department contiene Finanzas<br /><br />Reglas de acceso:<br /><br />-Permitir leído User.Country=Resource.Country y User.department = Resource.Department<br />-Permitir control total User.MemberOf(FinanceAdmin)<br /><br />Excepción:<br /><br />Permitir la lectura memberOf(FinanceException)|  
|1.4|Determinar las propiedades de archivo necesarias para la directiva|Etiqueta los archivos con:<br /><br />-Departamento<br />-País|  
|1.5|Determinar los tipos de notificación y grupos necesarios para la directiva|Los tipos de notificación:<br /><br />-País<br />-Departamento<br /><br />Grupos de usuarios:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Determinar los servidores en el que se aplica esta directiva.|Aplica la directiva en todos los servidores de archivos de finanzas.|  
  
## <a name="BKMK_1.3"></a>Implementar: Configurar los componentes y la directiva  
Esta sección presenta un ejemplo que implementa una directiva de acceso central para documentos de finanzas.  
  
|No|Paso|Ejemplo|  
|------|--------|-----------|  
|2.1|Crear los tipos de notificación|Crear los siguientes tipos de notificación:<br /><br />-Departamento<br />-País|  
|2.2|Crear propiedades de recurso|Crear y habilitar las siguientes propiedades de recurso:<br /><br />-Departamento<br />-País|  
|2.3|Configurar una regla de acceso central|Crear una regla de finanzas documentos que incluye la directiva especificada en la sección anterior.|  
|2.4|Configurar una directiva de acceso central (PAC)|Crear un extremo de directiva de finanzas y agregar la regla de documentos de finanzas a ese límite.|  
|2.5|Directiva de acceso central de destino para los servidores de archivos|Publicar el límite de la directiva de Finanzas en los servidores de archivos.|  
|2.6|Habilitar compatibilidad KDC para notificaciones, autenticación compuesta y protección Kerberos.|Habilitar compatibilidad KDC para notificaciones, autenticación compuesta y protección de Kerberos para contoso.com.|  
  
En el siguiente procedimiento, puedes crear dos tipos de notificación: país y departamento.  
  
#### <a name="to-create-claim-types"></a>Para crear los tipos de notificación  
  
1.  Abrir DC1 del servidor en el Administrador de Hyper-V y el registro en como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Abre el centro de administración de Active Directory.  
  
3.  Haz clic en el **icono de la vista de árbol**, expanda **Control de acceso dinámico**y, a continuación, selecciona **los tipos de notificación **.  
  
    Haz clic en **los tipos de notificación**, haz clic en **nueva**y, a continuación, haz clic en **tipo de notificación **.  
  
    > [!TIP]  
    > También puedes abrir un **crear el tipo de notificación:** ventana desde la **tareas** panel. En la **tareas** panel, haz clic en **nueva**y, a continuación, haz clic en **tipo de notificación **.  
  
4.  En la **atributo Source** lista, desplázate hacia abajo de la lista de atributos y haz clic en **departamento **. Esto debería rellenar la **nombre para mostrar** campo con **departamento **. Haz clic en **Aceptar**.  
  
5.  En **tareas** panel, haz clic en **nueva**y, a continuación, haz clic en **tipo de notificación**.  
  
6.  En la **atributo Source** lista, desplázate hacia abajo de la lista de atributos y, a continuación, haz clic en el **c** atributo (nombre del país). En la **nombre para mostrar**, escriba **país**.  
  
7.  En la **valores sugeridos** sección, selecciona **se sugieren los siguientes valores:**y, a continuación, haz clic en **agregar**.  
  
8.  En la **valor** y **nombre para mostrar** campos, escribe **US**y, a continuación, haz clic en **Aceptar**.  
  
9. Repite el paso anterior. En la **agrega un valor de sugerir** cuadro de diálogo, escribe **JP** en la **valor** y **nombre para mostrar** campos y, a continuación, haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> Puedes usar el Visor de historial de Windows PowerShell en el centro de administración de Active Directory para buscar los cmdlets de Windows PowerShell para cada procedimiento que realizar en el centro de administración de Active Directory. Para obtener más información, consulta [Visor de historial de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
El siguiente paso es crear las propiedades de recurso. En el siguiente procedimiento se crea una propiedad de recursos que se agrega automáticamente a la lista de propiedades Global de recursos en el controlador de dominio, por lo que está disponible para el servidor de archivos.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Para crear y habilitar las propiedades de recurso creados previamente  
  
1.  En el panel izquierdo del centro de administración de Active Directory, haga clic en **vista de árbol**. Expande **Control de acceso dinámico**y, a continuación, selecciona **propiedades de recurso**.  
  
2.  Haz clic en **propiedades de recurso**, haz clic en **nueva**y, a continuación, haz clic en **propiedad de recursos de referencia**.  
  
    > [!TIP]  
    > También puedes elegir la propiedad de un recurso desde el **tareas** panel. Haz clic en **nueva** y, a continuación, haz clic en **propiedad de recursos de referencia**.  
  
3.  En **selecciona un tipo de notificación que se comparta se sugiere valores lista**, haz clic en **país**.  
  
4.  En la **nombre para mostrar**, escriba **país**y, a continuación, haz clic en **Aceptar**.  
  
5.  Haz doble clic en el **propiedades de recurso** la lista, desplázate hacia abajo hasta la **departamento** propiedad del recurso. Haz clic en y, a continuación, haz clic en **habilitar**. Esto te permitirá integrado **departamento** propiedad del recurso.  
  
6.  En la **propiedades de recurso** lista en el panel de navegación del centro de administración de Active Directory, ahora tiene dos propiedades de recurso habilitado:  
  
    -   País  
  
    -   Departamento  
  
![guías de solución](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
El siguiente paso es crear reglas de acceso central que definan que pueden acceder a recursos. En este escenario son las reglas de negocio:  
  
-   Finanzas documentos pueden ser leídas solo por miembros del departamento de finanzas.  
  
-   Los miembros del departamento de Finanzas pueden acceder a solo los documentos en su propio país.  
  
-   Solo los administradores de Finanzas pueden tener acceso de escritura.  
  
-   Para los miembros del grupo FinanceException te permitirá una excepción. Este grupo tendrá acceso de lectura.  
  
-   El administrador y el propietario del documento seguirán teniendo acceso completo.  
  
O para expresar las reglas con construcciones de Windows Server 2012:  
  
Selección de destino: Resource.Department contiene Finanzas  
  
Reglas de acceso:  
  
-   Permitir la lectura User.Country=Resource.Country y User.department = Resource.Department  
  
-   Permitir el control total User.MemberOf(FinanceAdmin)  
  
-   Permitir User.MemberOf(FinanceException) de lectura  
  
#### <a name="to-create-a-central-access-rule"></a>Para crear una regla de acceso central  
  
1.  En el panel izquierdo del centro de administración de Active Directory, haga clic en **vista de árbol**, selecciona **Control de acceso dinámico**y, a continuación, haz clic en **reglas de acceso Central**.  
  
2.  Haz clic en **reglas de acceso Central**, haz clic en **nueva**y, a continuación, haz clic en **reglas de acceso Central**.  
  
3.  En la **nombre**, escriba **Finanzas documentos regla**.  
  
4.  En la **recursos de destino** sección, haz clic en **editar**y en la **reglas de acceso Central** cuadro de diálogo, haz clic en **agregar una condición**. Agrega la condición siguiente:   
    [**Recursos**] [**Departamento**] [**Es igual a**] [**Value**] [**Finanzas**] y, a continuación, haz clic en **Aceptar**.  
  
5.  En la **permisos** sección, selecciona **usa los siguientes permisos como permisos actuales**, haz clic en **editar**y en la **la configuración de seguridad avanzada para permisos** haz clic en el cuadro de diálogo **agregar**.  
  
    > [!NOTE]  
    > **Usar los siguientes permisos como permisos propuestos** opción te permite crear la directiva de almacenamiento provisional. Para obtener más información sobre cómo hacerlo, consulte la mantener: cambios y la fase de la sección de directiva en este tema.  
  
6.  En la **entrada de permiso para permisos** cuadro de diálogo, haz clic en **seleccionar una entidad de seguridad**, tipo **usuarios autenticados**y, a continuación, haz clic en **Aceptar**.  
  
7.  En la **entrada de permiso para permisos** cuadro de diálogo, haz clic en **agregar una condición**y agrega las siguientes condiciones:   
    [**User**] [**país**] [**Any of**] [**Recursos**] [**país**]   
     Haz clic en **agregar una condición**.   
     [**And**]   
    Haz clic en [**usuario**] [**departamento**] [**cualquiera de**] [**recursos**] [**departamento**]. Establecer el **permisos** a **lectura**.  
  
8.  Haz clic en **Aceptar**y, a continuación, haz clic en **agregar**. Haz clic en **seleccionar una entidad de seguridad**, tipo **FinanceAdmin**y, a continuación, haz clic en **Aceptar**.  
  
9. Selecciona el **modificar, leer y ejecutar, leer y escribir** permisos y, a continuación, haz clic en **Aceptar**.  
  
10. Haz clic en **agregar**, haz clic en **seleccionar una entidad de seguridad**, tipo **FinanceException**y, a continuación, haz clic en **Aceptar**. Selecciona los permisos para ser **lectura** y **leer y ejecutar**.  
  
11. Haz clic en **Aceptar** tres veces para finalizar y volver al centro de administración de Active Directory.  
  
    ![guías de solución](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
    El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
 
    $countryClaimType = Get-ADClaimType país  
    $departmentClaimType = Get-ADClaimType departamento  
    $countryResourceProperty = Get-ADResourceProperty país  
    $departmentResourceProperty = Get-ADResourceProperty departamento  
    $currentAcl = "O:SYG:SYD:AR(A;; FA;; O W) (A; FA;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; FA;; SY) (XA; 0 x1200a9;; AU;((@USER." + $countryClaimType.Name + "Any_of @RESOURCE." + $countryResourceProperty.Name + ") & & (@USER." + $departmentClaimType.Name + "Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + "Contains {`"Finance`"}) "  
    New-ADCentralAccessRule "Finanzas a documentos regla" - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> En el ejemplo del cmdlet anterior, los identificadores de seguridad (SID) para el grupo FinanceAdmin y los usuarios se determinan en tiempo de creación y será diferentes en el ejemplo. Por ejemplo, el SID valor proporcionado (S-1-5-21-1787166779-1215870801-2157059049-1113) para las necesidades de FinanceAdmins reemplazarse con el SID para el grupo de FinanceAdmin que necesitas para crear en la implementación real. Puedes usar Windows PowerShell para buscar el valor del SID de este grupo, ese valor se asigna a una variable y, a continuación, usar la variable aquí. Para obtener más información, consulta [sugerencia de Windows PowerShell: trabajar con el SID](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
Ahora debería tener una regla de acceso central que permite a los usuarios tener acceso a documentos desde el mismo país y el departamento de la mismo. La regla permite que el grupo FinanceAdmin editar documentos y permite que el grupo FinanceException leer los documentos. Esta regla está destinada únicamente los documentos que se clasifica como Finanzas.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Para agregar una regla de acceso central a una directiva de acceso central  
  
1.  En el panel izquierdo del centro de administración de Active Directory, haga clic en **Control de acceso dinámico**y, a continuación, haz clic en **directivas de acceso Central**.  
  
2.  En la **tareas** panel, haz clic en **nueva**y, a continuación, haz clic en **directiva de acceso Central**.  
  
3.  En **crear la directiva de acceso Central:**, tipo **Finanzas directiva** en la **nombre** cuadro.  
  
4.  En **reglas de acceso central miembro**, haz clic en **agregar**.  
  
5.  Haz doble clic en el **Finanzas documentos regla** para agregar a la **agrega las siguientes reglas de acceso central** lista y, a continuación, haz clic en **Aceptar**.  
  
6.  Haz clic en **Aceptar** a fin. Ahora debería tener una directiva de acceso central denominada directiva de finanzas.  
  
    ![guías de solución](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
    El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Para aplicar la directiva de acceso central a través de los servidores de archivos mediante la directiva de grupo  
  
1.  En la **inicio** de pantalla, en la **búsqueda**, escriba **Group Policy Management **. Haz doble clic en **administración de directivas de grupo **.  
  
    > [!TIP]  
    > Si la **herramientas administrativas mostrar** configuración está deshabilitada, la **herramientas administrativas** carpeta y su contenido no aparecerá en la **configuración** resultados.  
  
    > [!TIP]  
    > En el entorno de producción, debes crear una unidad organizativa de servidor de archivos (OU) y agregar todos los servidores de archivos a esta unidad organizativa, al que quieres aplicar esta directiva. A continuación, puedes crear una directiva de grupo y agregar esta unidad organizativa a esa directiva.  
  
2.  En este paso, edición el objeto de directiva de grupo que creaste en [crear el controlador de dominio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) sección en el entorno de prueba para incluir la directiva de acceso central que creaste. En el Editor de administración de directiva de grupo, desplázate hasta y selecciona la unidad organizativa en el dominio (contoso.com en este ejemplo): **Group Policy Management**, **bosque: contoso.com**, **dominios**, **contoso.com**, **Contoso**, **FileServerOU **.  
  
3.  Haz clic en **FlexibleAccessGPO**y, a continuación, haz clic en **editar **.  
  
4.  En la ventana del Editor de administración de directivas de grupo, ve a **configuración del equipo**, expanda **directivas**, expanda **configuración de Windows**y haz clic en **la configuración de seguridad **.  
  
5.  Expande **sistema de archivos**, haz clic en **directiva de acceso Central**y, a continuación, haz clic en **directivas de acceso Central de administrar**.  
  
6.  En la **configuración de directivas de acceso Central** diálogo cuadro, agregar **Finanzas directiva**y, a continuación, haz clic en **Aceptar **.  
  
7.  Desplázate hacia abajo hasta **configuración de directiva de auditoría avanzada**y se expande.  
  
8.  Expande **las directivas de auditoría**y selecciona **acceso a objetos **.  
  
9. Haz doble clic en **auditar almacenamiento provisional de directiva de acceso Central **. Selecciona todas las casillas de verificación y, a continuación, haz clic en **Aceptar **. Este paso permite que el sistema recibir eventos de auditoría relacionados con directivas de la prueba de acceso Central.  
  
10. Haz doble clic en **propiedades del sistema de archivos de auditoría **. Selecciona todas las casillas de verificación, a continuación, haz clic en **Aceptar **.  
  
11. Cierra el Editor de administración de directivas de grupo. Ahora ha incluido la directiva de acceso central para la directiva de grupo.  
  
Para que los controladores de dominio de un dominio proporcionar notificaciones o datos de autorización del dispositivo, los controladores de dominio deben configurarse para admitir el control de acceso dinámico.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Para habilitar el soporte técnico para reclamaciones y la autenticación compuesta contoso.com  
  
1.  Abre Administración de directivas de grupo, haz clic en **contoso.com**y, a continuación, haz clic en **controladores de dominio **.  
  
2.  Haz clic en **directiva predeterminada de controladores de dominio**y, a continuación, haz clic en **editar**.  
  
3.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **configuración del equipo**, haz doble clic en **directivas**, haz doble clic en **plantillas administrativas**, haz doble clic en **sistema**y, a continuación, haz doble clic en **KDC**.  
  
4.  Haz doble clic en **compuestos de compatibilidad KDC para notificaciones, autenticación y protección de Kerberos **. En la **compuestos de compatibilidad KDC para notificaciones, autenticación y protección de Kerberos** cuadro de diálogo, haz clic en **habilitado** y selecciona **Supported** desde el **opciones** lista desplegable. (Debes habilitar esta configuración para usar notificaciones de usuario en las directivas de acceso central).  
  
5.  Cerrar **administración de directivas de grupo**.  
  
6.  Abre un símbolo del sistema y escribe `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Implementar la directiva de acceso central  
  
||Paso|Ejemplo|  
|-|--------|-----------|  
|3.1|Asignar el límite de a las carpetas compartidas apropiadas en el servidor de archivos.|Asignar la directiva de acceso central a la carpeta compartida correspondiente en el servidor de archivos.|  
|3.2|Comprueba que acceso está correctamente configurado.|Comprobar el acceso a los usuarios de diferentes países y departamentos.|  
  
En este paso se asigna la directiva de acceso central a un servidor de archivos. Se inicia sesión en un servidor de archivos que esté recibiendo la directiva de acceso central que creaste los pasos anteriores y asignar la directiva a una carpeta compartida.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Para asignar una directiva de acceso central a un servidor de archivos  
  
1.  En el Administrador de Hyper-V, conecta al servidor archivo1. Iniciar sesión en el servidor mediante Contoso\Administrador con la contraseña:**pass@word1**.  
  
2.  Abre un símbolo del sistema con privilegios elevados y escribe: **comando gpupdate/force**. Esto garantiza que los cambios de directiva de grupo surtan efecto en el servidor.  
  
3.  También deberás actualizar las propiedades del recurso Global de Active Directory. Abre una ventana de Windows PowerShell con privilegios elevados y escribe `Update-FSRMClassificationpropertyDefinition`. Haz clic en entrar y, a continuación, cierra Windows PowerShell.  
  
    > [!TIP]  
    > También puede actualizar las propiedades del recurso Global, iniciando sesión en el servidor de archivos. Para actualizar las propiedades del recurso Global desde el servidor de archivos, haz lo siguiente  
    >   
    > 1.  Inicio de sesión para archivo1 de servidor de archivos como Contoso\Administrador, usando la contraseña **pass@word1**.  
    > 2.  Abre el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos, haz clic en **inicio**, tipo **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
    > 3.  En el Administrador de recursos de servidor de archivos, haz clic en **administración de archivos de clasificación**, haz clic en **propiedades de la clasificación** y, a continuación, haz clic en **actualizar **.  
  
4.  Abre el Explorador de Windows y en el panel izquierdo, haz clic en la unidad D. Haz clic en el **Finanzas documentos** carpeta y haz clic en **propiedades **.  
  
5.  Haz clic en el **clasificación**, haga clic **país**y, a continuación, selecciona **EE. UU.** en la **valor** campo.  
  
6.  Haz clic en **departamento**, a continuación, selecciona **finanzas** en la **valor** campo y, a continuación, haz clic en **aplicar **.  
  
    > [!NOTE]  
    > Recuerda que se configuró la directiva de acceso central para archivos de destino para el departamento de finanzas. Los pasos anteriores marca todos los documentos en la carpeta con los atributos de país y departamento.  
  
7.  Haz clic en el **seguridad** pestaña y, a continuación, haz clic en **avanzadas**. Haz clic en el **directiva Central** pestaña.  
  
8.  Haz clic en **cambio**, selecciona **Finanzas directiva** desde el menú desplegable y, a continuación, haz clic en **aplicar **. Puedes ver el **Finanzas documentos regla** incluido en la directiva. Expande el elemento para ver todos los permisos que se establecen al crear la regla en Active Directory.  
  
9. Haz clic en **Aceptar** para volver a Windows Explorer.  
  
En el paso siguiente, se garantiza que acceso está correctamente configurado.  Cuentas de usuario necesitan tener establecido el atributo de departamento apropiado (se establece con el centro de administración de Active Directory). La forma más sencilla para ver los resultados de la nueva directiva eficaces es usar la **acceso eficaz** ficha en el Explorador de Windows. La **acceso eficaz** pestaña muestra los derechos de acceso de un usuario determinado.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Para examinar el acceso para varios usuarios  
  
1.  En el Administrador de Hyper-V, conecta al servidor archivo1. Iniciar sesión en el servidor mediante contoso\administrator. Navegar a D:\ en el Explorador de Windows. Haz clic en el **Finanzas documentos** carpeta y, a continuación, haz clic en **propiedades **.  
  
2.  Haz clic en el **seguridad**, haga clic **avanzadas**y, a continuación, haz clic en el **acceso eficaz** pestaña.  
  
3.  Para examinar los permisos de un usuario, haz clic en **seleccionar un usuario**, escribe el nombre de usuario y, a continuación, haz clic en **ver acceso eficaz** para ver los derechos de acceso eficaz. Por ejemplo:  
  
    -   Myriam Delesalle (MDelesalle) es el departamento de finanzas y debe tener acceso de lectura a la carpeta.  
  
    -   Miles Reid (MReid) es un miembro del grupo FinanceAdmin y debe tener acceso de modificación a la carpeta.  
  
    -   Esther Valle (EValle) no está en el departamento de finanzas; Sin embargo, lo es un miembro del grupo FinanceException y debe tener acceso de lectura.  
  
    -   Maira Wenzel (MWenzel) no está en el departamento de finanzas y no es miembro de uno de ellos el grupo FinanceAdmin o FinanceException. No debe tener acceso a la carpeta.  
  
    Ten en cuenta que la última columna denominada **acceso limitado por** en la ventana de acceso eficaz. Esta columna indica qué gates se presente, junto con los permisos de la persona. En este caso, el recurso compartido y los permisos NTFS permiten a los usuarios control total. Sin embargo, la directiva de acceso central restringe el acceso según las reglas que has configurado anteriormente.  
  
## <a name="BKMK_1.5"></a>Mantener: Cambiar y organizar la directiva  
  
||||  
|-|-|-|  
|Número|Paso|Ejemplo|  
|4.1|Configurar notificaciones de dispositivo para que los clientes|Establecer la configuración de directiva de grupo para habilitar las notificaciones de dispositivo|  
|4.2|Habilitar una notificación para dispositivos.|Habilitar el tipo de notificación del país para dispositivos.|  
|4.3|Agregar una directiva de copia intermedia a la regla existente de acceso central que quieres modificar.|Modificar la regla de documentos de finanzas para agregar una directiva provisional.|  
|4.4|Ver los resultados de la directiva provisional.|Comprueba los permisos de éster Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Para configurar la directiva de grupo configuración para habilitar las reclamaciones por dispositivos  
  
1.  Iniciar sesión en DC1, abre Administración de directivas de grupo, haz clic en **contoso.com**, haz clic en **directiva de dominio predeterminada**, haz clic derecho y selecciona **editar **.  
  
2.  En la ventana del Editor de administración de directivas de grupo, ve a **configuración del equipo**, **directivas**, **plantillas administrativas**, **sistema**, **Kerberos **.  
  
3.  Selecciona **compatibilidad del cliente Kerberos con reclamaciones, autenticación compuesta y protección de Kerberos** y haz clic en **habilitar **.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Para habilitar una notificación para dispositivos  
  
1.  Abrir DC1 del servidor en el Administrador de Hyper-V y el registro en como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Desde el **herramientas** menú, abre centro de administración de Active Directory.  
  
3.  Haz clic en **vista de árbol**, expanda **Control de acceso dinámico**, haz doble clic en **tipos de notificación**y haz doble clic en el **país** reclamar.  
  
4.  En **reclamaciones de este tipo se pueden emitir para las siguientes clases**, selecciona el **equipo** casilla de verificación. Haz clic en **Aceptar**.   
    Ambos **usuario** y **equipo** ahora debe estar seleccionadas casillas de verificación. La reclamación país ahora puede usarse con dispositivos además de los usuarios.  
  
El siguiente paso es crear una regla provisional de directiva. Directivas de ensayo pueden usarse para supervisar los efectos de una nueva entrada de directiva para habilitarlo. En el paso siguiente, se crea una entrada provisional de directiva y supervisar el efecto en la carpeta compartida.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Para crear una regla provisional de directiva y agregarlo a la directiva de acceso central  
  
1.  Abrir DC1 del servidor en el Administrador de Hyper-V y el registro en como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Abre el centro de administración de Active Directory.  
  
3.  Haz clic en **vista de árbol**, expanda **Control de acceso dinámico**y selecciona **reglas de acceso Central **.  
  
4.  Haz clic en **Finanzas documentos regla**y, a continuación, haz clic en **propiedades **.  
  
5.  En la **propuesta permisos** sección, selecciona el **permiso Habilitar configuración de ensayo** casilla de verificación, haz clic en **editar**y, a continuación, haz clic en **agregar **. En la **entrada de permiso para permisos propuesta** ventana, haz clic en el **seleccionar una entidad de seguridad** vincular, escriba **usuarios autenticados**y, a continuación, haz clic en **Aceptar **.  
  
6.  Haz clic en el **agregar una condición** vincular y agrega la condición siguiente:   
     [**User**] [**país**] [**Any of**] [**Recursos**] [**País**].  
  
7.  Haz clic en **agregar una condición** de nuevo y agrega la condición siguiente:  
    [**And**]   
     [**Dispositivo**] [**país**] [**Any of**] [**Recursos**] [**País**]  
  
8.  Haz clic en **agregar una condición** de nuevo y agrega la condición siguiente.  
    [Y]   
     [**User**] [**Group**] [**Miembro de cualquier**] [**Valor**] \ (**FinanceException**)  
  
9. Para establecer el FinanceException, grupo, haz clic en **agregar elementos** y, en la **Seleccionar usuarios, equipo, cuenta de servicio o grupo** ventana, escribe **FinanceException **.  
  
10. Haz clic en **permisos**, selecciona **Control total**y haz clic en **Aceptar **.  
  
11. En la configuración de seguridad de avance de ventana propuesta permisos, selecciona **FinanceException** y haz clic en **quitar **.  
  
12. Haz clic en **Aceptar** dos veces a fin.  
  
![guías de solución](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> En el ejemplo del cmdlet anterior, el valor de servidor refleja el servidor en el entorno de laboratorio de prueba. Puedes usar el Visor de historial de Windows PowerShell para buscar los cmdlets de Windows PowerShell para cada procedimiento que realizar en el centro de administración de Active Directory. Para obtener más información, consulta [Visor de historial de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
En este conjunto de permisos propuesto, los miembros del grupo FinanceException tienen acceso completo a archivos desde su propio país cuando tienen acceso a ellos a través de un dispositivo desde el mismo país que el documento. Las entradas de auditoría están disponibles en los servidores de archivos que se intenta obtener acceso a archivos de registro de seguridad cuando alguien del departamento de finanzas. Sin embargo, la configuración de seguridad no se aplica hasta que se promueve la directiva de almacenamiento provisional.  
  
En el siguiente procedimiento, se comprueban los resultados de la directiva provisional. Obtener acceso a la carpeta compartida con un nombre de usuario que tiene permisos en función de la regla actual. Esther Valle (EValle) es un miembro de FinanceException y actualmente tiene derechos de lectura. Según nuestra directiva provisional, EValle no debe tener los derechos.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Para comprobar los resultados de la directiva provisional  
  
1.  Conectarse a la archivo1 de servidor de archivo en el Administrador de Hyper-V y registro de como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Abre una ventana de símbolo del sistema y escriba **comando gpupdate/force **. Esto garantiza que los cambios de directiva de grupo surtirán efecto en el servidor.  
  
3.  En el Administrador de Hyper-V, conecta al servidor CLIENTE1. Cerrar sesión del usuario que ha iniciado sesión. Reinicia la máquina virtual, CLIENTE1. A continuación, inicie sesión en el equipo mediante contoso\EValle pass@word1.  
  
4.  Haz doble clic en el acceso directo del escritorio a \\\FILE1\Finance documentos. EValle aún deben tener acceso a los archivos. Cambiar a archivo1.  
  
5.  Abre **Visor de eventos** desde el acceso directo en el escritorio. Expande **registros de Windows**y, a continuación, selecciona **seguridad **. Abre las entradas con **4818 de identificador de evento**bajo la **almacenamiento provisional de directiva de acceso Central** categoría de la tarea. Verás que EValle se ha permitido el acceso; Sin embargo, según la directiva de ensayo, el usuario hubiera ha denegado el acceso.  
  
## <a name="next-steps"></a>Pasos siguientes  
Si tienes un sistema de administración de servidor central como System Center Operations Manager, puedes configurar también la supervisión de eventos. Esto permite a los administradores supervisar los efectos de directivas de acceso central antes de aplicarlas.  
  

