---
title: Compilación de complementos con el modelo de evaluación de riesgos 2019 de AD FS
description: 'Más información sobre: compilar complementos con AD FS modelo de evaluación de riesgos de 2019'
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/05/2020
ms.topic: article
ms.openlocfilehash: 2c1d05450869d558d1991da2f95b72bcaeca7462
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047823"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Compilación de complementos con el modelo de evaluación de riesgos 2019 de AD FS

Ahora puede crear sus propios complementos para bloquear o asignar una puntuación de riesgo a las solicitudes de autenticación durante varias fases: solicitud recibida, autenticación previa y posterior a la autenticación. Esto puede realizarse mediante el nuevo modelo de evaluación de riesgos introducido con AD FS 2019.

## <a name="what-is-the-risk-assessment-model"></a>¿Cuál es el modelo de evaluación de riesgos?

El modelo de evaluación de riesgos es un conjunto de interfaces y clases que permiten a los desarrolladores leer los encabezados de solicitud de autenticación e implementar su propia lógica de evaluación de riesgos. El código implementado (complemento) se ejecuta en línea con AD FS proceso de autenticación. Por ejemplo, mediante las interfaces y las clases que se incluyen con el modelo, puede implementar código para bloquear o permitir la solicitud de autenticación en función de la dirección IP del cliente incluida en el encabezado de la solicitud. AD FS ejecutará el código para cada solicitud de autenticación y tomará las medidas adecuadas según la lógica implementada.

El modelo permite el código de complemento en cualquiera de las tres fases de AD FS canalización de autenticación, como se muestra a continuación

![modelo](media/ad-fs-risk-assessment-model/risk1.png)

1.    **Fase de solicitud recibida** : permite compilar complementos para permitir o bloquear solicitudes cuando AD FS recibe la solicitud de autenticación, es decir, antes de que el usuario escriba las credenciales. Puede usar el contexto de la solicitud (por ejemplo, la dirección IP del cliente, el método http, el servidor proxy DNS, etc.) disponible en esta fase para realizar la evaluación de riesgos. Por ejemplo, puede crear un complemento para leer la dirección IP desde el contexto de la solicitud y bloquear la solicitud de autenticación si la dirección IP está en la lista predefinida de direcciones IP de riesgo.

2.    **Fase de autenticación previa** : permite crear complementos para permitir o bloquear solicitudes en el punto en el que el usuario proporciona las credenciales, pero antes de que AD FS las evalúe. En esta fase, además del contexto de la solicitud, también tiene información sobre el contexto de seguridad (por ejemplo, el token de usuario, el identificador de usuario, etc.) y el contexto de protocolo (por ejemplo, el protocolo de autenticación, ClientID, ResourceID, etc.) que se usarán en la lógica de evaluación de riesgos. Por ejemplo, puede crear un complemento para evitar ataques de pulverización de contraseñas mediante la lectura de la contraseña del usuario desde el token de usuario y el bloqueo de la solicitud de autenticación si la contraseña está en la lista predefinida de contraseñas de riesgo.

3.    **Posterior a la autenticación** : habilita el complemento de compilación para evaluar el riesgo después de que el usuario haya proporcionado credenciales y AD FS haya realizado la autenticación. En esta fase, además del contexto de la solicitud, el contexto de seguridad y el contexto del Protocolo, también tiene información sobre el resultado de la autenticación (éxito o error). El complemento puede evaluar la puntuación de riesgo en función de la información disponible y pasar la puntuación de riesgo a las reglas de notificaciones y directivas para su posterior evaluación.

Para comprender mejor cómo crear un complemento de evaluación de riesgos y ejecutarlo en línea con AD FS proceso, vamos a crear un complemento de ejemplo que bloquee las solicitudes procedentes de ciertas direcciones IP de **extranet** identificadas como peligrosas, registrar el complemento con AD FS y, por último, probar la funcionalidad.

>[!NOTE]
>También puede crear un complemento de [usuario de riesgo](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection), un complemento de ejemplo que aprovecha el nivel de riesgo del usuario determinado por Azure ad Identity Protection para bloquear la autenticación o exigir la autenticación multifactor (MFA). [Aquí](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection) encontrará los pasos para crear un complemento de usuario de riesgo

## <a name="building-a-sample-plug-in"></a>Compilar un complemento de ejemplo

>[!NOTE]
>En este tutorial solo se muestra cómo se puede crear un complemento de ejemplo. No se trata de la solución en la que estamos creando una solución preparada para la empresa.

### <a name="pre-requisites"></a>Requisitos previos
A continuación se muestra la lista de requisitos previos necesarios para compilar este complemento de ejemplo.

- AD FS 2019 instalado y configurado
- .NET Framework 4,7 y versiones posteriores
- Visual Studio

### <a name="build-plug-in-dll"></a>DLL del complemento de compilación
El siguiente procedimiento le guiará a través de la creación de un archivo dll de complemento de ejemplo.

1. Descargue el complemento de ejemplo, use git bash y escriba lo siguiente:

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Cree un archivo **. csv** en cualquier ubicación del servidor de AD FS (en mi caso, creé el archivo de **authconfigdb.csv** en **C:\extensions**) y agregue las direcciones IP que desea bloquear a este archivo.

   El complemento de ejemplo bloqueará las solicitudes de autenticación procedentes de las **direcciones IP** de la extranet que se enumeran en este archivo.

   >{! Nota] Si tiene una granja de AD FS, puede crear el archivo en cualquiera de los servidores de AD FS o en todos. Cualquiera de los archivos se puede usar para importar las direcciones IP de riesgo en AD FS. Analizaremos el proceso de importación en detalle en la [dll de registro del complemento con AD FS](#register-the-plug-in-dll-with-ad-fs) sección siguiente.

3. Abrir el proyecto `ThreatDetectionModule.sln` mediante Visual Studio

4. Quite del `Microsoft.IdentityServer.dll` Explorador de soluciones como se muestra a continuación:</br>
   ![model](media/ad-fs-risk-assessment-model/risk2.png)

5. Agregue una referencia al `Microsoft.IdentityServer.dll` de su AD FS como se muestra a continuación

   a.    Haga clic con el botón derecho en **referencias** en el **Explorador de soluciones** y seleccione **Agregar referencia..** .</br>
   ![model](media/ad-fs-risk-assessment-model/risk3.png)

   b.    En la ventana **Administrador de referencias** , seleccione **examinar**. En el, **Seleccione los archivos a los que se va a hacer referencia...** , seleccione en `Microsoft.IdentityServer.dll` la carpeta de instalación de AD FS (en mi caso **C:\Windows\ADFS**) y haga clic en **Agregar**.

   >[!NOTE]
   >En mi caso, voy a crear el complemento en el propio servidor de AD FS. Si el entorno de desarrollo está en un servidor diferente, copie el `Microsoft.IdentityServer.dll` de la carpeta de instalación de AD FS en AD FS Server en el cuadro de desarrollo.</br>

   ![modelo](media/ad-fs-risk-assessment-model/risk4.png)

   c.    Haga clic en **Aceptar** en la ventana **Administrador de referencias** después de asegurarse de que la `Microsoft.IdentityServer.dll` casilla está activada.</br>
   ![model](media/ad-fs-risk-assessment-model/risk5.png)

6. Todas las clases y referencias están ahora en su lugar para realizar una compilación.   Sin embargo, puesto que la salida de este proyecto es un archivo dll, tendrá que instalarse en la **caché global de ensamblados**(GAC) del servidor de AD FS y la dll debe estar firmada en primer lugar. Se puede hacer de la forma siguiente:

   a.    **Haga clic con el botón derecho** en el nombre del proyecto, ThreatDetectionModule. En el menú, haga clic en **propiedades**.</br>
   ![model](media/ad-fs-risk-assessment-model/risk6.png)

   b.    En la página **propiedades** , haga clic en **firma**, a la izquierda y, a continuación, active la casilla **firmar el ensamblado**. En el menú seleccionar **un archivo de clave de nombre seguro**:, seleccione **<nuevo... >**</br>
   ![model](media/ad-fs-risk-assessment-model/risk7.png)

   c.    En el **cuadro de diálogo crear clave de nombre seguro**, escriba un nombre (puede elegir cualquier nombre) para la clave, desactive la casilla **proteger mi archivo de clave con la contraseña**. A continuación, haga clic en **Aceptar**.</br>
   ![model](media/ad-fs-risk-assessment-model/risk8.png)

   d.    Guarde el proyecto como se muestra a continuación</br>
   ![model](media/ad-fs-risk-assessment-model/risk9.png)

7. Compile el proyecto haciendo clic en **compilar** y, a continuación, **vuelva a generar la solución** como se muestra a continuación</br>
   ![model](media/ad-fs-risk-assessment-model/risk10.png)

   Compruebe la **ventana de salida**, en la parte inferior de la pantalla, para ver si se ha producido algún error.</br>
   ![model](media/ad-fs-risk-assessment-model/risk11.png)


El complemento (dll) ya está listo para su uso y se encuentra en la carpeta **\bin\debug** de la carpeta del proyecto (en mi caso, eso es **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**).

El siguiente paso es registrar este archivo dll con AD FS, por lo que se ejecuta en línea con AD FS proceso de autenticación.

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Registrar el archivo DLL del complemento con AD FS

Es necesario registrar el archivo dll en AD FS mediante el `Register-AdfsThreatDetectionModule` comando de PowerShell en el servidor de AD FS, sin embargo, antes de registrarse, es necesario obtener el token de clave pública. Este token de clave pública se creó cuando creamos la clave y firmaba el archivo dll con esa clave. Para saber cuál es el token de clave pública para el archivo dll, puede utilizar el **SN.exe** como se indica a continuación.

1. Copie el archivo dll de la carpeta **\bin\debug** a otra ubicación (en mi caso, cópielo a **C:\extensions**).

2. Inicie el **símbolo del sistema para desarrolladores** para Visual Studio y vaya al directorio que contiene el **sn.exe** (en mi caso, el directorio es **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**) ![ Model](media/ad-fs-risk-assessment-model/risk12.png)

3. Ejecutar el comando **SN** con el parámetro **-T** y la ubicación del modelo de archivo (en mi caso `SN -T "C:\extensions\ThreatDetectionModule.dll"` ) ![](media/ad-fs-risk-assessment-model/risk13.png)</br>
   El comando le proporcionará el token de clave pública (para mí, el **token de clave pública es 714697626ef96b35**)

4. Agregue el archivo DLL a la **caché global de ensamblados** del servidor de AD FS. nuestro procedimiento recomendado sería crear un instalador adecuado para el proyecto y usar el instalador para agregar el archivo a la GAC. Otra solución es usar **Gacutil.exe** (más información sobre **Gacutil.exe** disponibles [aquí](/dotnet/framework/tools/gacutil-exe-gac-tool)) en el equipo de desarrollo.  Puesto que mi Visual Studio está en el mismo servidor que AD FS, usaremos **Gacutil.exe** como se indica a continuación.

   a.    En Símbolo del sistema para desarrolladores para Visual Studio y vaya al directorio que contiene el **Gacutil.exe** (en mi caso, el directorio es c:\Archivos de programa **(x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.    Ejecutar el modelo de comando de **Gacutil** (en mi caso `Gacutil /IF C:\extensions\ThreatDetectionModule.dll` ) ![](media/ad-fs-risk-assessment-model/risk14.png)

   >[!NOTE]
   >Si tiene una granja de AD FS, se debe ejecutar lo anterior en cada servidor AD FS de la granja.

5. Abra **Windows PowerShell** y ejecute el siguiente comando para registrar el archivo dll.
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>"
   ```
   En mi caso, el comando es:
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv"
   ```

   >[!NOTE]
   >Solo tiene que registrar la dll una vez, incluso si tiene una granja de AD FS.

6. Reiniciar el servicio de AD FS después de registrar el archivo dll

Eso es, el archivo dll se ha registrado con AD FS y está listo para su uso.

 >[!NOTE]
 > Si se realizan cambios en el complemento y el proyecto se vuelve a generar, es necesario volver a registrar el archivo dll actualizado. Antes de registrarse, tendrá que anular el registro del archivo dll actual con el siguiente comando:</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br>
 >En mi caso, el comando es:
 >```
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Probar el complemento

1. Abra el archivo de **authconfig.csv** que hemos creado anteriormente (en mi caso, en la ubicación **C:\extensions**) y agregue las **direcciones IP de extranet** que quiera bloquear. Cada IP debe estar en una línea independiente y no debe haber espacios al final</br>
   ![model](media/ad-fs-risk-assessment-model/risk18.png)

2. Guarde y cierre el archivo.

3. Importe el archivo actualizado en AD FS ejecutando el siguiente comando de PowerShell.

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```

   En mi caso, el comando es:
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```

4. Inicie la solicitud de autenticación desde el servidor con la misma dirección IP que agregó en **authconfig.csv**.

   En esta demostración, usaré [AD FS herramienta de ayuda X-Ray](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) para iniciar una solicitud. Si desea usar la herramienta X-Ray, siga las instrucciones.

   Escriba la instancia de servidor de Federación y el botón de **autenticación de prueba** de posicionamiento.</br>
   ![model](media/ad-fs-risk-assessment-model/risk15.png)

5. La autenticación se bloquea como se muestra a continuación.</br>
   ![model](media/ad-fs-risk-assessment-model/risk16.png)

Ahora que sabemos cómo compilar y registrar el complemento, vamos a guiar el código del complemento para comprender la implementación mediante las nuevas interfaces y clases que se introdujeron con el modelo.

## <a name="plug-in-code-walkthrough"></a>Tutorial de código de complemento

Abra el proyecto `ThreatDetectionModule.sln` con Visual Studio y, a continuación, abra el archivo principal **UserRiskAnalyzer.CS** en el **Explorador de soluciones** de la derecha de la pantalla.</br>
![model](media/ad-fs-risk-assessment-model/risk17.png)

El archivo contiene la clase principal UserRiskAnalyzer que implementa la clase abstracta [ThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule) y la interfaz [IRequestReceivedThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule) para leer la dirección IP desde el contexto de la solicitud, comparar la IP obtenida con las direcciones ip CARGAdas desde AD FS base de archivos y bloquear la solicitud si hay una coincidencia de IP. Vamos a analizar estos tipos con más detalle.

### <a name="threatdetectionmodule-abstract-class"></a>Clase abstracta ThreatDetectionModule

Esta clase abstracta carga el complemento en AD FS canalización, lo que permite ejecutar el código del complemento en línea con AD FS proceso.

```
public abstract class ThreatDetectionModule
{
    protected ThreatDetectionModule();

    public abstract string VendorName { get; }
    public abstract string ModuleIdentifier { get; }

 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData);
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger);
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData);
 }
```
La clase incluye los siguientes métodos y propiedades.

|Método |Tipo|Definición|
|-----|-----|-----|
|[OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) |Void|Llamado por AD FS cuando el complemento se carga en su canalización|
|[OnAuthenticationPipelineUnload](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload) |Void|Llamado por AD FS cuando el complemento se descarga de su canalización|
|[OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate)| Void|Llamado por AD FS en la actualización de la configuración |
|**Property** |**Type** |**Definición**|
|[VendorName](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname)|String |Obtiene el nombre del proveedor propietario del complemento.|
|[ModuleIdentifier](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier)|String |Obtiene el identificador del complemento.|

En nuestro complemento de ejemplo, usamos los métodos [OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) y [OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate) para leer las direcciones IP predefinidas de AD FS dB. Se llama a [OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) cuando el complemento se registra con AD FS mientras se llama a [OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate) cuando se importa el archivo. csv mediante el `Import-AdfsThreatDetectionModuleConfiguration` cmdlet.

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>Interfaz IRequestReceivedThreatDetectionModule

Esta [interfaz](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule) le permite implementar la evaluación de riesgos en el punto en el que AD FS recibe la solicitud de autenticación, pero antes de que el usuario escriba las credenciales, es decir, en la fase de solicitud recibida del proceso de autenticación.

```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger,
RequestContext requestContext );
}
```

La interfaz incluye el método [EvaluateRequest](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest) que permite usar el contexto de la solicitud de autenticación que se ha pasado en el parámetro de entrada requestContext para escribir la lógica de evaluación de riesgos. El parámetro requestContext es de tipo [requestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext).

El otro parámetro de entrada pasado es Logger, que es de tipo [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger). El parámetro se puede utilizar para escribir los mensajes de error, de auditoría o de depuración en los registros de AD FS.

El método devuelve [ThrottleStatus](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus) (0 si NotEvaluated, 1 para bloquear y 2 para permitir) AD FS que, a continuación, bloquea o permite la solicitud.

En nuestro complemento de ejemplo, la implementación del método [EvaluateRequest](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest) analiza el [clientIpAddress](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) desde el parámetro [requestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext) y lo compara con todas las direcciones IP cargadas desde la base de AD FS. Si se encuentra una coincidencia, el método devuelve 2 para el **bloque**; de lo contrario, devuelve 1 para **permitir**. Según el valor devuelto, AD FS bloquea o permite la solicitud.

>[!NOTE]
>El complemento de ejemplo descrito anteriormente solo implementa la interfaz IRequestReceivedThreatDetectionModule. Sin embargo, el modelo de evaluación de riesgos proporciona dos interfaces adicionales: IPreAuthenticationThreatDetectionModule (para implementar la lógica de evaluación de riesgos de la fase de autenticación previa de durante) y IPostAuthenticationThreatDetectionModule (para implementar la lógica de evaluación de riesgos durante la fase posterior a la autenticación). A continuación se proporcionan los detalles de las dos interfaces.

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interfaz IPreAuthenticationThreatDetectionModule

Esta [interfaz](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule) le permite implementar la lógica de evaluación de riesgos en el punto en el que el usuario proporciona las credenciales, pero antes de que AD FS las evalúe, es decir, la fase de autenticación previa.

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger,
RequestContext requestContext,
SecurityContext securityContext,
ProtocolContext protocolContext,
IList<Claim> additionalClams
);
}
```
La interfaz incluye el método [EvaluatePreAuthentication](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication) que permite usar la información pasada en los parámetros de entrada [RequestContext RequestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext), [SecurityContext SecurityContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext), [ProtocolContext ProtocolContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext)y [IList <Claim> additionalClams](/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) para escribir la lógica de evaluación de riesgos de autenticación previa.

>[!NOTE]
>Para obtener una lista de las propiedades que se pasan con cada tipo de contexto, visite las definiciones de clase [RequestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext), [SecurityContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)y [ProtocolContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext) .

El otro parámetro de entrada pasado es Logger, que es de tipo [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger). El parámetro se puede utilizar para escribir los mensajes de error, de auditoría o de depuración en los registros de AD FS.

El método devuelve [ThrottleStatus](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus) (0 si NotEvaluated, 1 para bloquear y 2 para permitir) AD FS que, a continuación, bloquea o permite la solicitud.

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interfaz IPostAuthenticationThreatDetectionModule

Esta [interfaz](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule) le permite implementar la lógica de evaluación de riesgos después de que el usuario haya proporcionado las credenciales y AD FS haya realizado la autenticación, es decir, la fase posterior a la autenticación.

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger,
RequestContext requestContext,
SecurityContext securityContext,
ProtocolContext protocolContext,
AuthenticationResult authenticationResult,
IList<Claim> additionalClams
);
}
```

La interfaz incluye el método [EvaluatePostAuthentication](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication) que le permite usar la información que se pasa en los parámetros de entrada [RequestContext RequestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext), [SecurityContext SecurityContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext), [ProtocolContext ProtocolContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext)y [IList <Claim> additionalClams](/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) para escribir la lógica de evaluación del riesgo posterior a la autenticación.

>[!NOTE]
> Para obtener una lista completa de las propiedades que se pasan con cada tipo de contexto, consulte las definiciones de la clase [RequestContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext), [SecurityContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)y [ProtocolContext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext) .

El otro parámetro de entrada pasado es Logger, que es de tipo [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger). El parámetro se puede utilizar para escribir los mensajes de error, de auditoría o de depuración en los registros de AD FS.

El método devuelve la [puntuación de riesgo](/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants) que se puede usar en AD FS reglas de directiva y de notificaciones.

>[!NOTE]
>Para que el complemento funcione, la clase principal (en este caso UserRiskAnalyzer) debe derivar la clase abstracta [ThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule) y debe implementar al menos una de las tres interfaces descritas anteriormente. Una vez que se registra el archivo dll, AD FS comprueba qué interfaces se han implementado y las llama en la fase adecuada de la canalización.

### <a name="faqs"></a>Preguntas más frecuentes

**¿Por qué debo crear estos complementos?**</br>
**R:** Estos complementos no solo proporcionan funcionalidad adicional para proteger su entorno frente a ataques como los ataques de pulverización de contraseñas, sino que también le ofrecen la flexibilidad de crear su propia lógica de evaluación de riesgos en función de sus requisitos.

**¿Dónde se capturan los registros?**</br>
**R:** Puede escribir los registros de errores en el registro de eventos "AD FS/admin" mediante el método WriteAdminLogErrorMessage, los registros de auditoría en el registro de seguridad "AD FS auditoría" mediante el método WriteAuditMessage y los registros de depuración en el registro de depuración "AD FS Tracing" con el método WriteDebugMessage.

**¿Puede agregar estos complementos aumentar AD FS latencia del proceso de autenticación?**</br>
**R:** El impacto en la latencia se determinará por el tiempo que se tarda en ejecutar la lógica de evaluación de riesgos que se implementa. Se recomienda evaluar el impacto de la latencia antes de implementar el complemento en el entorno de producción.

**¿Por qué no AD FS sugerir la lista de direcciones IP de riesgo, usuarios, etc.?**</br>
**R:** Aunque no está disponible actualmente, estamos trabajando en la creación de la inteligencia para sugerir IP de riesgo, usuarios, etc. en el modelo de evaluación de riesgos conectables. Pronto se compartirán las fechas de inicio.

**¿Qué otros complementos de ejemplo están disponibles?**</br>
**R:** Están disponibles los siguientes complementos de ejemplo:

|NOMBRE|Descripción|
|-----|-----|
|[Complemento de usuario de riesgo](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection)|Complemento de ejemplo que bloquea la autenticación o aplica MFA en función del nivel de riesgo del usuario determinado por Azure AD Identity Protection.|
