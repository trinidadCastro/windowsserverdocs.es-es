---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: Implementar una directiva de acceso central (pasos de demostración)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817146"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Implementar una directiva de acceso central (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este escenario, las operaciones de seguridad del Departamento Financiero trabajan con la seguridad de la información central para especificar la necesidad de una directiva de acceso central para proteger información financiera archivada que esté almacenada en servidores de archivos. La información financiera archivada de cada país solo pueden consultarla (con permisos de solo lectura) los empleados del Departamento Financiero del dicho país. El grupo de administradores de finanzas central puede acceder a la información financiera de todos los países.  
  
Para implementar una directiva de acceso central es necesario completar varias fases:  
  
|Fase|Descripción  
|---------|---------------  
|[Plan: Identificar la necesidad de directiva y la configuración necesaria para la implementación](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identifica la necesidad de una directiva y la configuración necesaria para la implementación. 
|[Implementar: Configurar los componentes y la directiva](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configura los componentes y la directiva.  
|[Implementar la directiva de acceso central](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Implementa la directiva.  
|[Mantener: Cambiar y almacenar provisionalmente la directiva](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Los cambios de directiva y almacenamiento provisional. 
  
## <a name="BKMK_1.1"></a>Configurar un entorno de prueba  
Antes de comenzar, deberás preparar un entorno de laboratorio para este escenario. Los pasos para configurar el laboratorio se explican con detalle en [Apéndice B: Configurar el entorno de prueba](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Plan: Identifica la necesidad de una directiva y la configuración necesaria para la implementación  
En esta sección encontrarás series de pasos de alto nivel que te ayudarán a planificar la implementación.  
  
||Paso|Ejemplo|  
|-|--------|-----------|  
|1.1|Una empresa determina que necesita una directiva de acceso central|Para proteger la información financiera almacenada en los servidores de archivos, las operaciones de seguridad del Departamento Financiero trabajan con seguridad de la información central para especificar la necesidad de una directiva de acceso central.|  
|1.2|Expresión de la directiva de acceso|Los documentos financieros solo deben leerlos los miembros del Departamento Financiero. Los miembros del Departamento Financiero solo deben tener acceso a los documentos de su propio país. Solo los administradores de finanzas deben tener acceso de escritura. Se permite una excepción para los miembros del grupo FinanceException. Este grupo tendrá acceso de lectura.|  
|1.3|Expresar la directiva de acceso en construcciones de Windows Server 2012|Destinatarios:<br /><br />-Resource.Department contiene Finanzas<br /><br />Reglas de acceso:<br /><br />-Permitir lectura User.Country=Resource.Country y User.department = Resource.Department<br />-Permitir control total User.MemberOf(FinanceAdmin)<br /><br />Excepción:<br /><br />Allow read memberOf(FinanceException)|  
|1.4|Determinar las propiedades del archivo necesarias para la directiva|Etiquetar archivos con:<br /><br />: Departamento<br />: País|  
|1.5|Determinar los tipos de notificaciones y los grupos necesarios para la directiva|Tipos de notificaciones:<br /><br />: País<br />: Departamento<br /><br />Grupos de usuarios:<br /><br />-   FinanceAdmin<br />-FinanceException|  
|1.6|Determinar los servidores donde se aplicará esta directiva|Aplica la directiva en todos los servidores de archivos de finanzas.|  
  
## <a name="BKMK_1.3"></a>Implementar: Configura los componentes y la directiva  
En esta sección encontrarás un ejemplo en el que implementa una directiva de acceso central para documentos financieros.  
  
|No|Paso|Ejemplo|  
|------|--------|-----------|  
|2.1|Crear tipos de notificaciones|Crea los siguientes tipos de notificaciones:<br /><br />: Departamento<br />: País|  
|2.2|Crear propiedades de recursos|Crea y habilita las siguientes propiedades de recursos:<br /><br />: Departamento<br />: País|  
|2.3|Configurar una regla de acceso central|Crea una regla llamada Documentos financieros que incluya una directiva determinada en la sección anterior.|  
|2.4|Configurar una directiva de acceso central (CAP)|Crea una CAP llamada Directiva de finanzas y agrega la regla Documentos financieros a esta.|  
|2.5|Dirigir la directiva de acceso central a los servidores de archivos|Publica la CAP Directiva de finanzas en los servidores de archivos.|  
|2.6|Habilita Compatibilidad de KDC para notificaciones, autenticación compuesta y protección de Kerberos.|Habilita Compatibilidad de KDC para notificaciones, autenticación compuesta y protección de Kerberos para contoso.com.|  
  
En el procedimiento siguiente puedes crear dos tipos de notificaciones: País y Departamento.  
  
#### <a name="to-create-claim-types"></a>Para crear tipos de notificaciones  
  
1.  Abre servidor DC1 en el Administrador de Hyper-V e inicie sesión como Contoso\Administrador, con la contraseña **pass@word1**.  
  
2.  Abre el Centro de administración de Active Directory.  
  
3.  Haz clic en el icono **Vista de árbol**, expande **Control de acceso dinámico** y selecciona **Tipos de notificaciones**.  
  
    Haz clic con el botón secundario en **Tipos de notificaciones**, en **Nuevo** y después en **Tipo de notificación**.  
  
    > [!TIP]  
    > También puedes abrir la ventana **Crear tipo de notificación** en el panel **Tareas**. En el panel **Tareas**, haz clic en **Nueva** y después en **Tipo de notificación**.  
  
4.  En la lista **Atributo de origen**, desplázate por la lista de atributos y haz clic en **departamento**. Esto rellenará el campo **Nombre para mostrar** con **departamento**. Haga clic en **Aceptar**.  
  
5.  En el panel **Tareas**, haz clic en **Nueva** y después en **Tipo de notificación**.  
  
6.  En la lista **Atributo de origen**, desplázate por los atributos y haz clic en el atributo **c** (nombre de país). En el campo **Nombre para mostrar**, escribe **país**.  
  
7.  En la sección **Valores sugeridos**, selecciona **Se sugieren los valores siguientes** y haz clic en **Agregar**.  
  
8.  En los campos **Valor** y **Nombre para mostrar**, escribe **US** y haz clic en **Aceptar**.  
  
9. Repite el paso anterior. En el cuadro de diálogo **Agregar un valor sugerido**, escribe **JP** en los campos **Valor** y **Nombre para mostrar**, y haz clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> Puedes usar el Visor del historial de Windows PowerShell en el Centro de administración de Active Directory para buscar los cmdlets de Windows PowerShell para cada procedimiento que realices en el Centro de administración de Active Directory. Para obtener más información, consulte [Visor del historial de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
El paso siguiente es crear propiedades de recursos. En el procedimiento siguiente vas a crear una propiedad de recurso que se agregará automáticamente a la lista Propiedades de recursos globales en el controlador de dominio para que esté disponible en el servidor de archivos.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Para crear y habilitar propiedades de recursos predefinidas  
  
1.  En el panel izquierdo del Centro de administración de Active Directory, haz clic en **Vista de árbol**. Expande **Control de acceso dinámico** y selecciona **Propiedades de recursos**.  
  
2.  Haz clic con el botón secundario en **Propiedades de recursos**, haz clic en **Nueva** y, después, en **Propiedad de recurso de referencia**.  
  
    > [!TIP]  
    > También puedes elegir una propiedad de recurso del panel de **Tareas**. Haz clic en **Nuevo** y, después, en **Propiedad de recurso de referencia**.  
  
3.  En **Seleccionar un tipo de notificación para compartir su lista de valores sugeridos**, haz clic en **país**.  
  
4.  En el campo **Nombre para mostrar**, escribe **país** y haz clic en **Aceptar**.  
  
5.  Haz doble clic en la lista **Propiedades de recursos** y desplázate hasta la propiedad de recurso **Departamento**. Haz clic con el botón secundario y selecciona **Habilitar**. Esto habilitará la propiedad de recurso predefinida **Departamento**.  
  
6.  En la lista **Propiedades de recursos**, en el panel de navegación del Centro de administración de Active Directory, ahora verás que hay dos propiedades de recursos habilitadas:  
  
    -   País  
  
    -   Departmento  
  
![guías de soluciones](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
El paso siguiente es crear reglas de acceso central que definan quién puede acceder a los recursos. En este escenario, las reglas de negocios son las siguientes:  
  
-   Los documentos financieros solo pueden leerlos los miembros del Departamento Financiero.  
  
-   Los miembros del Departamento Financiero solo pueden acceder a los documentos de su propio país.  
  
-   Solo los administradores de finanzas tienen acceso de escritura.  
  
-   Se permite una excepción para los miembros del grupo FinanceException. Este grupo tendrá acceso de lectura.  
  
-   El administrador y el propietario del documento tendrán acceso total.  
  
O para expresar las reglas con construcciones de Windows Server 2012:  
  
Destinatarios: Resource.Department Contains Finance  
  
Reglas de acceso:  
  
-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department  
  
-   Allow Full control User.MemberOf(FinanceAdmin)  
  
-   Allow Read User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>Para crear una regla de acceso central  
  
1.  En el panel izquierdo del Centro de administración de Active Directory, haz clic en **Vista de árbol**, selecciona **Control de acceso dinámico** y haz clic en **Reglas de acceso central**.  
  
2.  Haz clic con el botón secundario en **Reglas de acceso central**, haz clic en **Nueva** y después en **Regla de acceso central**.  
  
3.  En el campo **Nombre**, escribe **Regla de documentos financieros**.  
  
4.  En la sección **Recursos de destino**, haz clic en **Editar** y, en el cuadro de diálogo **Regla de acceso central**, haz clic en **Agregar una condición**. Agrega la condición siguiente:   
    [**Recurso**] [**Departamento**] [**Igual**] [**Valor**] [**Finanzas**] y haz clic en **Aceptar**.  
  
5.  En la sección **Permisos**, selecciona **Usar los siguientes permisos como permisos actuales**, haz clic en **Editar** y, en el cuadro de diálogo **Configuración de seguridad avanzada para permisos**, haz clic en **Agregar**.  
  
    > [!NOTE]  
    > Con la opción **Usar los siguientes permisos como permisos propuestos** podrás crear una directiva provisional. Para obtener más información sobre cómo crearla, consulta la sección Mantenimiento: Cambiar y almacenar provisionalmente la directiva en este tema.  
  
6.  En el cuadro de diálogo **Entrada de permiso para permisos**, haz clic en **Seleccionar una entidad de seguridad**, escribe **Usuarios autenticados** y haz clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Entrada de permiso para permisos**, haz clic en **Agregar una condición** y agrega la condición siguiente:   
    [**Usuario**] [**país**] [**cualquiera de**] [**recursos**] [**país**]   
     Haz clic en **Agregar una condición**.   
     [**y**]   
    Haga clic en [**usuario**] [**departamento**] [**cualquiera de**] [**recursos**] [**departamento**]. Establece los **permisos** en **lectura**.  
  
8.  Haz clic en **Aceptar** y después en **Agregar**. Haz clic en **Seleccionar una entidad de seguridad**, escribe **FinanceAdmin** y haz clic en **Aceptar**.  
  
9. Selecciona los permisos **Modificar, Leer y ejecutar, Lectura, Escritura** y haz clic en **Aceptar**.  
  
10. Haz clic en **Agregar**, en **Seleccionar una entidad de seguridad**, escribe **FinanceException** y haz clic en **Aceptar**. Selecciona los permisos de **Lectura** y **Leer y ejecutar**.  
  
11. Haz clic en **Aceptar** tres veces para terminar y volver al Centro de administración de Active Directory.  
  
    ![guías de soluciones](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
    Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
 
    $countryClaimType = get-ADClaimType país  
    $departmentClaimType = Get-ADClaimType department  
    $countryResourceProperty = get-ADResourceProperty país  
    $departmentResourceProperty = get-ADResourceProperty departamento  
    $currentAcl = "O:SYG:SYD:AR(A;; FA;; O W) (A; FA;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; FA;; SY) (XA; 0 x1200a9;; AU ;((@USER. " + $countryClaimType.Name + "Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + "Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + "contiene {`"Finance`"}) "  
    Nuevo ADCentralAccessRule "Regla de documentos de finanzas" - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> En el cmdlet del ejemplo anterior, los identificadores de seguridad (SID) para el grupo FinanceAdmin y los usuarios se determinan en el momento de la creación y serán distintos en tu ejemplo. Por ejemplo, es necesario reemplazar el valor de SID proporcionado (S-1-5-21-1787166779-1215870801-2157059049-1113) para los FinanceAdmins con el SID real del grupo FinanceAdmin que has de crear en la implementación. Puede usar Windows PowerShell para buscar el valor del SID de este grupo, asignar dicho valor a una variable y, a continuación, usar la variable aquí. Para obtener más información, consulte [sugerencia de Windows PowerShell: Trabajar con SID](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
Ahora verás una regla de acceso central que permite a los usuarios acceder a documentos del mismo país y del mismo departamento. La regla permite al grupo FinanceAdmin editar los documentos y al grupo FinanceException leerlos. Esta regla solo es válida para documentos clasificados como de Finanzas.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Para agregar una regla de acceso central a una directiva de acceso central  
  
1.  En el panel izquierdo del Centro de administración de Active Directory, haz clic en **Control de acceso dinámico** y, después, en **Directivas de acceso central**.  
  
2.  En el panel **Tareas**, haz clic en **Nueva** y, después, en **Directiva de acceso central**.  
  
3.  En **Directiva de acceso central**, escribe **Directiva de finanzas** en el cuadro **Nombre**.  
  
4.  En **Reglas de acceso central miembros**, haz clic en **Agregar**.  
  
5.  Haz doble clic en la **Regla de documentos financieros** para agregarla a la lista **Agregar las siguientes reglas de acceso central** y, después, haz clic en **Aceptar**.  
  
6.  Haz clic en **Aceptar** para finalizar. Ahora deberías tener una directiva de acceso central llamada Directiva de finanzas.  
  
    ![guías de soluciones](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
    Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Para aplicar la directiva de acceso central en servidores de archivos que usen la directiva de grupo  
  
1.  En la pantalla **Inicio**, en el cuadro **Buscar**, escribe **Administración de directivas de grupo**. Haz doble clic en **Administración de directivas de grupo**.  
  
    > [!TIP]  
    > Si la opción **Mostrar herramientas administrativas** está deshabilitada, no aparecerá la carpeta **Herramientas administrativas** ni su contenido en los resultados de **Configuración**.  
  
    > [!TIP]  
    > En el entorno de producción, crea una unidad de organización de servidores de archivos (OU) y agrega todos los servidores de archivos a esta OU, a la que deberás aplicar esta directiva. Después, puedes crear una directiva de grupo y agregar esta OU a dicha directiva.  
  
2.  En este paso, editarás el objeto de directiva de grupo que creaste en la sección [Creación del controlador de dominio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) del entorno de prueba para incluir la nueva directiva de acceso central. En el Editor de administración de directivas de grupo, navega hasta la unidad organizativa del dominio (en este ejemplo, contoso.com) y selecciónala: **Administración de directivas de grupo**, **Bosque: contoso.com**, **Dominios**, **contoso.com**, **Contoso**, **OUServidorArchivos**.  
  
3.  Haz clic con el botón secundario en **FlexibleAccessGPO** y después en **Editar**.  
  
4.  En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración del equipo**, expande **Directivas**, **Configuración de Windows** y haz clic en **Configuración de seguridad**.  
  
5.  Expande **Sistema de archivos**, haz clic con el botón derecho en **Directiva de acceso central** y, después, haz clic en **Administrar directivas de acceso central**.  
  
6.  En el cuadro de diálogo **Configuración de directivas de acceso central**, agrega **Directiva de finanzas** y haz clic en **Aceptar**.  
  
7.  Desplázate hasta **Configuración de directiva de auditoría avanzada** y expándela.  
  
8.  Expande **Directivas de auditoría** y selecciona **Acceso a objetos**.  
  
9. Haz doble clic en **Auditar almacenamiento provisional de directivas de acceso central**. Selecciona las tres casillas y haz clic en **Aceptar**. Este paso permite al sistema recibir eventos de auditoría relacionados con las directivas de almacenamiento provisional de acceso central.  
  
10. Haz doble clic en **Propiedades del sistema de archivos de auditoría**. Selecciona las tres casillas y haz clic en **Aceptar**.  
  
11. Cierra el Editor de administración de directivas de grupo. Ahora ya has incluido la directiva de acceso central en la directiva de grupo.  
  
Para que los controladores de dominio de un dominio proporcionen notificaciones o datos de autorización de dispositivo, los controladores de dominio deben configurarse para admitir el control de acceso dinámico.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Para habilitar la compatibilidad con notificaciones y autenticación compuesta para contoso.com  
  
1.  Abre Administración de directivas de grupo, haz clic en **contoso.com** y, después, en **Controladores de dominio**.  
  
2.  Haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y, a continuación, haga clic en **Editar**.  
  
3.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **Configuración del equipo**, haz doble clic en **Directivas**, haz doble clic en **Plantillas administrativas**, haz doble clic en **Sistema**y, después, haz doble clic en **KDC**.  
  
4.  Haz doble clic en **Compatibilidad de KDC para notificaciones, autenticación compuesta y protección de Kerberos**. En el cuadro de diálogo **Compatibilidad de KDC para notificaciones, autenticación compuesta y protección de Kerberos**, haz clic en **Habilitada** y selecciona **Compatible** de la lista desplegable **Opciones**. (Tienes que habilitar esta opción para usar notificaciones de usuario en directivas de acceso central).  
  
5.  Cierre **Administración de directivas de grupo**.  
  
6.  Abra un símbolo del sistema y escriba `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Implementar la directiva de acceso central  
  
||Paso|Ejemplo|  
|-|--------|-----------|  
|3.1|Asigna el CAP a las carpetas compartidas correspondientes en el servidor de archivos.|Asigna la directiva de acceso central a la carpeta compartida correspondiente en el servidor de archivos.|  
|3.2|Comprueba que el acceso está configurado correctamente.|Comprueba el acceso para usuarios desde diferentes países y departamentos.|  
  
En este paso asignarás la directiva de acceso central a un servidor de archivos. Inicia sesión en un servidor de archivos que reciba la directiva de acceso central que creaste en el paso anterior y asigna la directiva a una carpeta compartida.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Para asignar una directiva de acceso central a un servidor de archivos  
  
1.  En el Administrador de Hyper-V, conecta con el servidor FILE1. Inicie sesión en el servidor usando Contoso\Administrador con la contraseña: **pass@word1**.  
  
2.  Abra un símbolo del sistema con privilegios elevados y escriba: **gpupdate /force**. Esto garantiza que los cambios que realices en la directiva de grupo se apliquen en el servidor.  
  
3.  También tienes que actualizar las propiedades de recursos globales desde Active Directory. Abra una ventana de Windows PowerShell y escriba `Update-FSRMClassificationpropertyDefinition`. Haz clic en ENTRAR y, después, cierra Windows PowerShell.  
  
    > [!TIP]  
    > También puedes actualizar las propiedades de recursos globales si inicias sesión en el servidor de archivos. Sigue estos pasos para actualizar las propiedades de recursos globales desde el servidor de archivos  
    >   
    > 1.  Inicie sesión en el archivo de servidor FILE1 como Contoso\Administrador, con la contraseña **pass@word1**.  
    > 2.  Abra el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos haz clic en **Inicio**, escribe **administrador de recursos del servidor de archivos**y haz clic en **Administrador de recursos del servidor de archivos**.  
    > 3.  En el Administrador de recursos del servidor de archivos, haz clic en **Administración de clasificación de archivos**, haz clic con el botón secundario en **Propiedades de clasificación** y selecciona **Actualizar**.  
  
4.  Abre el Explorador de Windows y, en el panel izquierdo, haz clic en la unidad D. Haz clic con el botón secundario en la carpeta **Documentos financieros** y selecciona **Propiedades**.  
  
5.  Haz clic en la pestaña **Clasificación**, en **País** y, después, selecciona **US** en el campo **Valor**.  
  
6.  Haz clic en **Departamento**, selecciona **Finanzas** en el campo **Valor** y haz clic en **Aplicar**.  
  
    > [!NOTE]  
    > Recuerda que la directiva de acceso central se configuró para archivos del Departamento de Finanzas. Los pasos anteriores marcan todos los documentos de la carpeta con los atributos País y Departamento.  
  
7.  Haga clic en la pestaña **Seguridad** y, a continuación, en **Opciones avanzadas**. Haz clic en la pestaña **Directiva central**.  
  
8.  Haz clic en **Cambiar**, selecciona **Directiva de finanzas** en el menú desplegable y, después, haz clic en **Aplicar**. Verás que la **Regla Documentos financieros** está incluida en la directiva. Expande el artículo para todos los permisos que estableciste al crear la regla en Active Directory.  
  
9. Haz clic en **Aceptar** para volver al Explorador de Windows.  
  
En el paso siguiente, comprueba que el acceso esté configurado correctamente.  Las cuentas de usuario deben tener el conjunto de atributos de Departamento correctos (establecidas mediante el Centro de administración de Active Directory). La forma más fácil de ver los resultados aplicados de la nueva directiva es usar la pestaña **Acceso efectivo** en el Explorador de Windows. En la pestaña **Acceso efectivo** podrás ver los derechos de acceso de una cuenta de usuario concreta.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Para examinar el acceso de varios usuarios  
  
1.  En el Administrador de Hyper-V, conecta con el servidor FILE1. Inicia sesión en el servidor con la cuenta contoso\administrador. Navega a la unidad D:\ en el Explorador de Windows. Haz clic con el botón secundario en la carpeta **Documentos financieros** y selecciona **Propiedades**.  
  
2.  Haz clic en la pestaña **Seguridad**, haz clic en **Avanzadas** y, después, en la pestaña **Acceso efectivo**.  
  
3.  Para examinar los permisos para un usuario, haga clic en **seleccionar un usuario**, escriba el nombre del usuario y, a continuación, haga clic en **ver acceso efectivo** para ver los derechos de acceso efectivo. Por ejemplo:  
  
    -   Myriam Delesalle (MDelesalle) pertenece al Departamento Financiero y debe tener acceso de lectura a la carpeta.  
  
    -   Miles Reid (MReid) pertenece al grupo FinanceAdmin y debe tener acceso de modificación a la carpeta.  
  
    -   Esther Valle (EValle) no pertenece al Departamento Financiero, pero sí que es miembro del grupo FinanceException y debe tener acceso de lectura.  
  
    -   Maira Wenzel (MWenzel) no pertenece al Departamento Financiero ni es miembro de los grupos FinanceAdmin o FinanceException, por lo que no debe tener acceso a la carpeta.  
  
    La última columna se llama **Acceso limitado por** en la ventana Acceso efectivo. Esta columna indica los límites que afectan los permisos de la persona. En este caso, los permisos de recursos compartidos y NTFS otorgan control total a todos los usuarios. Pero la directiva de acceso central restringe el acceso basándose en las reglas que has configurado anteriormente.  
  
## <a name="BKMK_1.5"></a>Mantener: Cambiar y almacenar provisionalmente la directiva  
  
||||  
|-|-|-|  
|Número|Paso|Ejemplo|  
|4.1|Configurar notificaciones de dispositivo para clientes|Establece la configuración de directiva de grupo para habilitar las notificaciones de dispositivo|  
|4.2|Habilita una notificación para dispositivos.|Habilita el tipo de notificación de país para dispositivos.|  
|4.3|Agrega una directiva de almacenamiento provisional a la regla de acceso central existente que quieras modificar.|Modifica la regla de documentos financieros para agregar una directiva de almacenamiento provisional.|  
|4.4|Visualiza los resultados de la directiva de almacenamiento provisional.|Comprobación de permisos de Ester Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Para habilitar las notificaciones para dispositivos mediante la configuración de directiva de grupo  
  
1.  Inicia sesión en DC1, abre Administración de directivas de grupo, haz clic en **contoso.com**, haz clic con el botón secundario en **Directiva predeterminada de dominio** y selecciona **Editar**.  
  
2.  En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración del equipo**, **Directivas**, **Plantillas administrativas**, **Sistema**, **Kerberos**.  
  
3.  Selecciona **Compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos** y haz clic en **Habilitar**.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Para habilitar una notificación para dispositivos  
  
1.  Abre servidor DC1 en el Administrador de Hyper-V e inicie sesión como Contoso\Administrador, con la contraseña **pass@word1**.  
  
2.  En el menú **Herramientas**, abre el Centro de administración de Active Directory.  
  
3.  Haz clic en **Vista de árbol**, expande **Control de acceso dinámico**, haz doble clic en **Tipos de notificaciones** y, después, haz doble clic en la notificación de **país**.  
  
4.  En **Las notificaciones de este tipo pueden ser emitidas por las siguientes clases**, selecciona la casilla **Equipo**. Haga clic en **Aceptar**.   
    Deben estar seleccionadas las dos casillas, **Usuario** y **Equipo**. Ahora puede usarse la notificación de país con dispositivos, además de los usuarios.  
  
El paso siguiente es crear una regla de directivas de almacenamiento provisional. Las directivas de almacenamiento provisional se pueden usar para supervisar los efectos de una nueva entrada de directiva antes de habilitarla. En el paso siguiente crearás una entrada de directiva de almacenamiento provisional y comprobarás los efectos en la carpeta compartida.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Para crear una regla de directivas de almacenamiento provisional y agregar la a la directiva de acceso central  
  
1.  Abre servidor DC1 en el Administrador de Hyper-V e inicie sesión como Contoso\Administrador, con la contraseña **pass@word1**.  
  
2.  Abre el Centro de administración de Active Directory.  
  
3.  Haz clic en **Vista de árbol**, expande **Control de acceso dinámico** y selecciona **Reglas de acceso central**.  
  
4.  Haz clic con el botón secundario en **Regla de documentos financieros** y selecciona **Propiedades**.  
  
5.  En la sección **Permisos propuestos**, activa la casilla **Habilitar la configuración de almacenamiento provisional de permisos**, haz clic en **Editar** y, después, en **Agregar**. En la ventana **Entrada de permiso para permisos propuestos**, haz clic en el vínculo **Seleccionar una entidad de seguridad**, escribe **Usuarios autenticados** y haz clic en **Aceptar**.  
  
6.  Haz clic en el vínculo **Agregar una condición** y agrega la condición siguiente:   
     [**Usuario**] [**país**] [**Cualquiera de**] [**Recurso**] [**país**].  
  
7.  Vuelve a hacer clic en el vínculo **Agregar una condición** y agrega la condición siguiente:  
    [**y**]   
     [**Dispositivo**] [**país**] [**Cualquiera de**] [**Recurso**] [**País**]  
  
8.  Vuelve a hacer clic en el vínculo **Agregar una condición** y agrega la condición siguiente.  
    [Y]   
     [**Usuario**] [**grupo**] [**miembro de cualquiera**] [**valor**]\(**FinanceException**)  
  
9. Para establecer el grupo FinanceException, haz clic en **Agregar elementos** y, en la ventana **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escribe **FinanceException**.  
  
10. Haz clic en **Permisos**, selecciona **Control total** y haz clic en **Aceptar**.  
  
11. En la ventana Configuración de seguridad avanzada para permisos propuestos, selecciona **FinanceException** y haz clic en **Quitar**.  
  
12. Haz clic en **Aceptar** dos veces para finalizar.  
  
![guías de soluciones](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> En el cmdlet de ejemplo anterior, el valor Servidor refleja el servidor en un entorno de laboratorio de pruebas. Puedes usar el Visor del historial de Windows PowerShell para buscar los cmdlets de Windows PowerShell para cada procedimiento que realices en el Centro de administración de Active Directory. Para obtener más información, consulte [Visor del historial de Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
En este conjunto de permisos propuestos, los miembros del grupo FinanceException tendrán acceso total a los archivos de su propio país cuando accedan a estos a través de un dispositivo ubicado en el mismo país que el documento. Las entradas de auditoría están disponibles en el registro de seguridad Servidores de archivos cuando un usuario del Departamento Financiero intenta acceder a los archivos. Pero la configuración de seguridad no se aplica hasta que la directiva se promociona desde el almacenamiento provisional.  
  
En el procedimiento siguiente verificarás los resultados de la directiva de almacenamiento provisional. Puedes acceder a la carpeta compartida con un nombre de usuario que tenga permisos basados en la regla actual. Esther Valle (EValle) es miembro de FinanceException y actualmente tiene permisos de lectura. Según nuestra directiva de almacenamiento provisional, EValle no debería tener derechos.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Para verificar los resultados de la directiva de almacenamiento provisional  
  
1.  Conectar con el servidor de archivos FILE1 en el Administrador de Hyper-V y de registro en como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Abre una ventana del símbolo del sistema y escribe **gpupdate /force**. Esto garantiza que los cambios que realices en la directiva de grupo se apliquen en el servidor.  
  
3.  En el Administrador de Hyper-V, conéctate al servidor CLIENT1. Cierra la sesión del usuario que haya iniciado sesión actualmente. Reinicia la máquina virtual, CLIENT1. A continuación, inicie sesión en el equipo mediante el uso de contoso\EValle pass@word1.  
  
4.  Haga doble clic en el acceso directo del escritorio a \\\FILE1\Finance documentos. EValle debería seguir teniendo acceso a los archivos. Cambia a FILE1.  
  
5.  Abre el **Visor de eventos** mediante el acceso directo en el escritorio. Expande **Registros de Windows** y selecciona **Seguridad**. Abra las entradas con **Event ID 4818**bajo el **almacenamiento provisional de directivas de acceso Central** categoría de tarea. Verás que EValle tiene acceso permitido; pero, según la directiva de almacenamiento provisional, el usuario no debería tener acceso.  
  
## <a name="next-steps"></a>Pasos siguientes  
Si tienes un sistema de administración del servidor central como System Center Operations Manager, también puedes configurar la supervisión de eventos. Esto permite a los administradores supervisar los efectos de directivas de acceso central antes de aplicarlas.  
  
