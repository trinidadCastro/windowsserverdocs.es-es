---
title: Crear complementos con el modelo de evaluación de riesgos 2019 de AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e43f505a02ec2241a84f74ff57e217c2fb95157b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445356"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Crear complementos con el modelo de evaluación de riesgos 2019 de AD FS

Ahora puede crear sus propios complementos para bloquear o asignar una puntuación de riesgo a las solicitudes de autenticación durante varias fases: solicitud recibida, la autenticación previa y posterior a la autenticación. Esto puede realizarse usando el nuevo modelo de evaluación de riesgos introducidos con AD FS de 2019. 

## <a name="what-is-the-risk-assessment-model"></a>¿Qué es el modelo de evaluación de riesgo?

El modelo de evaluación de riesgos es un conjunto de interfaces y clases que permiten a los desarrolladores leer los encabezados de solicitud de autenticación e implementar su propia lógica de evaluación de riesgos. El código implementado (complemento), a continuación, se ejecuta en línea con el proceso de autenticación de AD FS. Por ejemplo, mediante las interfaces y clases que se incluyen con el modelo, puede implementar cualquier bloque de código o Permitir solicitud de autenticación basada en la dirección IP del cliente incluida en el encabezado de solicitud. AD FS se ejecute el código para cada solicitud de autenticación y tomar las medidas adecuadas según la lógica implementada. 

El modelo permite al código del complemento en cualquiera de las tres fases de canalización de autenticación de AD FS como se muestra a continuación

![modelo](media/ad-fs-risk-assessment-model/risk1.png)

1.  **Solicitar la fase recibido** : permite crear complementos para permitir o bloquear la solicitud cuando AD FS recibe la solicitud de autenticación, es decir, antes de usuario proporciona credenciales. Puede usar el contexto de solicitud (por ejemplo, IP de cliente, el método de Http, el servidor proxy DNS, etc.) disponibles en esta fase para realizar la evaluación de riesgos. Para p. ej., puede crear un complemento para leer la dirección IP desde el contexto de solicitud y bloquear la solicitud de autenticación si la dirección IP en la lista de direcciones IP de riesgo predefinida. 

2.  **Fase de autenticación previa** : permite crear complementos para permitir o bloquear la solicitud en el punto donde el usuario proporciona las credenciales, pero antes de que AD FS evalúa. En esta fase, además del contexto de solicitud también tiene información sobre el contexto de seguridad (por ejemplo, el token de usuario, identificador de usuario, etcetera) y el contexto de protocolo (por ejemplo, un protocolo de autenticación, clientid, resourceid, etcetera) para usar en su lógica de evaluación de riesgos. Para p. ej., puede crear un complemento para evitar ataques a contraseñas aerosol leyendo la contraseña de usuario desde el token de usuario y bloqueo de la solicitud de autenticación si la contraseña está en la lista predefinida de contraseñas de riesgo. 

3.  **La autenticación posterior** : permite crear un complemento para evaluar el riesgo después de que el usuario ha proporcionado las credenciales y ha realizado la autenticación de AD FS. En esta fase, además del contexto de solicitud, el contexto de seguridad y el contexto de protocolo, también tiene información sobre el resultado de la autenticación (éxito o error). El complemento puede evaluar la puntuación de riesgo en función de la información disponible y pasar la puntuación de riesgo para la notificación y reglas de directivas de evaluación adicional. 

Para entender mejor cómo crear una complemento de evaluación de riesgos y ejecutarlo en consonancia con el proceso de AD FS, vamos a compilar un ejemplo de complemento que bloquea las solicitudes procedentes de determinadas **extranet** IPs identificado como peligroso, registrar el complemento con AD FS y, por último, probará la funcionalidad. 

>[!NOTE]
>En este tutorial es solo mostrar cómo puede crear un ejemplo de complemento. No es la solución que estamos creando una solución listo de la empresa.  

## <a name="building-a-sample-plug-in"></a>Al compilar un ejemplo de complemento

### <a name="pre-requisites"></a>Requisitos previos
La siguiente es la lista de requisitos previos necesarios para compilar este ejemplo de complemento

- AD FS 2019 instalado y configurado
- .NET framework 4.7 y versiones posteriores
- Programa para la mejora

### <a name="build-plug-in-dll"></a>Generar la dll del complemento
El siguiente procedimiento le guiará por la creación de una dll de complemento de ejemplo.

1. Descargue el ejemplo de complemento, use Git Bash y escriba lo siguiente: 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Crear un **.csv** archivo en cualquier ubicación en el servidor de AD FS (en mi caso, creé la **authconfigdb.csv** archivo a **C:\extensions**) y agregue las direcciones IP que desea bloquear este archivo. 

   El ejemplo de complemento bloqueará las solicitudes de autenticación procedentes de la **IPs Extranet** enumerados en este archivo. 

   >{! Nota] si tiene una granja de AD FS, puede crear el archivo en cualquier o todos los servidores de AD FS. Cualquiera de los archivos puede usarse para importar la IP de riesgo en AD FS. Analizaremos el proceso de importación con detalle en la [registrar la dll del complemento con AD FS](#register-the-plug-in-dll-with-ad-fs) sección más adelante. 

3. Abra el proyecto `ThreatDetectionModule.sln` con Visual Studio

4. Quitar el `Microsoft.IdentityServer.dll` desde el Explorador de soluciones, tal como se muestra a continuación:</br>
   ![model](media/ad-fs-risk-assessment-model/risk2.png)

5. Agregar la referencia a la `Microsoft.IdentityServer.dll` de AD FS como se muestra a continuación

   a.   Haga clic con el botón derecho en **referencias** en **el Explorador de soluciones** y seleccione **Agregar referencia...**</br> 
   ![model](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.   En el **Administrador de referencias** ventana Seleccione **examinar**. En el **seleccionar archivos para referencia...** cuadro de diálogo, seleccione `Microsoft.IdentityServer.dll` desde la carpeta de instalación de AD FS (en mi caso **C:\Windows\ADFS**) y haga clic en **agregar**.
   
   >[!NOTE]
   >En mi caso que estoy creando el complemento en el mismo servidor de AD FS. Si el entorno de desarrollo está en un servidor diferente, copie el `Microsoft.IdentityServer.dll` desde la carpeta de instalación de AD FS en el servidor de AD FS en el cuadro de desarrollo.</br> 
   
   ![modelo](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.   Haga clic en **Aceptar** en el **Administrador de referencias** ventana después de asegurarse `Microsoft.IdentityServer.dll` está activada la casilla</br>
   ![model](media/ad-fs-risk-assessment-model/risk5.png)
 
6. Todas las clases y las referencias están ahora en su lugar para realizar una compilación.   Sin embargo, puesto que el resultado de este proyecto es un archivo dll, tendrá que estar instalado en el **caché Global de ensamblados**, o bien GAC, del servidor de AD FS y el archivo dll debe firmarse en primer lugar. Esto puede hacerse como sigue:

   a.   **Haga clic en** en el nombre del proyecto, ThreatDetectionModule. En el menú, haga clic en **propiedades**.</br>
   ![model](media/ad-fs-risk-assessment-model/risk6.png)
   
   b.   Desde el **propiedades** página, haga clic en **firma**, a la izquierda y, a continuación, activar la casilla de verificación **firmar el ensamblado**. Desde el **elegir un archivo de clave de nombre seguro**: desplegar el menú, seleccione **< nuevo... >**</br>
   ![model](media/ad-fs-risk-assessment-model/risk7.png)

   c.   En el **cuadro de diálogo Crear clave de nombre seguro**, escriba un nombre (puede elegir cualquier nombre) para la clave, desactive la casilla de verificación **proteger mi archivo de clave mediante contraseña**. A continuación, haga clic en **Aceptar**.
   ![model](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.   Guarde el proyecto como se muestra a continuación</br>
   ![model](media/ad-fs-risk-assessment-model/risk9.png)

7. Compile el proyecto, haga clic en **compilar** y, a continuación, **recompilar solución** tal como se muestra a continuación</br>
   ![model](media/ad-fs-risk-assessment-model/risk10.png)
 
   Compruebe el **ventana de salida**, en la parte inferior de la pantalla, para ver si se produce algún error</br>
   ![model](media/ad-fs-risk-assessment-model/risk11.png)


El complemento (dll) ahora está lista para su uso y se encuentra en la **\bin\Debug** carpeta de la carpeta del proyecto (en mi caso, es **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

El siguiente paso es registrar esta dll con AD FS, para que se ejecute en consonancia con el proceso de autenticación de AD FS. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Registrar la dll del complemento con AD FS

Es necesario registrar la dll en AD FS mediante el `Register-AdfsThreatDetectionModule` comando de PowerShell en el servidor de AD FS, sin embargo, antes de registrarte, debemos obtener el Token de clave pública. Este token de clave pública se creó cuando se creó la clave y firma la dll mediante esa clave. Para obtener información sobre el Token de clave pública para el archivo dll What ' s, puede usar el **SN.exe** como se indica a continuación.

1. Copie el archivo dll desde el **\bin\Debug** carpeta a otra ubicación (en mi caso copiarlo a **C:\extensions**)

2. Iniciar el **símbolo** para Visual Studio y vaya al directorio que contiene el **sn.exe** (en mi caso, el directorio es **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A herramientas de \bin\NETFX 4.7.2**) ![modelo](media/ad-fs-risk-assessment-model/risk12.png)

3. Ejecute el **SN** comando con el **-T** parámetro y la ubicación del archivo (en mi caso `SN -T “C:\extensions\ThreatDetectionModule.dll”`) ![modelo](media/ad-fs-risk-assessment-model/risk13.png)</br>
   El comando le proporcionará el token de clave pública (para mí, la **Token de clave pública es 714697626ef96b35**)

4. Agregar el archivo dll para el **caché Global de ensamblados** de servidor de AD FS nuestra práctica recomendada sería que crear un instalador adecuado para el proyecto y utilice el instalador para agregar el archivo a la GAC. Otra solución consiste en usar **Gacutil.exe** (obtener más información sobre **Gacutil.exe** disponibles [aquí](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) en el equipo de desarrollo.  Dado que tengo mi visual studio en el mismo servidor que AD FS, usaré **Gacutil.exe** como se indica a continuación.

   a.   Línea de comandos de desarrollador para Visual Studio y vaya al directorio que contiene el **Gacutil.exe** (en mi caso, el directorio es **C:\Program archivos (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   Ejecute el **Gacutil** comando (en mi caso `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modelo](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >Si tiene una granja de AD FS, el anterior se debe ejecutar en cada servidor de AD FS en la granja de servidores. 

5. Abra **Windows PowerShell** y ejecute el siguiente comando para registrar la dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
   ```
   En mi caso, el comando es: 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
   ```
 
   >[!NOTE]
   >Debe registrar la dll de una sola vez, incluso si tiene una granja de AD FS. 

6. Reinicie el servicio AD FS después de registrar el archivo dll

Eso es todo, el archivo dll ya está registrado con AD FS y listo para su uso.

 >[!NOTE]
 > Si se realizan cambios en el complemento y se vuelve a generar el proyecto, la dll actualizada debe registrarse de nuevo. Antes de registrar, necesita anular el registro de la dll actual con el comando siguiente:</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> En mi caso, el comando es:
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Probar el complemento

1. Abra el **authconfig.csv** archivo que creamos anteriormente (en mi caso en la ubicación **C:\extensions**) y agregue el **IPs Extranet** que desea bloquear. Todas las direcciones IP debe estar en una línea independiente y no debe haber ningún espacio al final</br>
   ![model](media/ad-fs-risk-assessment-model/risk18.png)
 
2. Guarde y cierre el archivo

3. Importe el archivo actualizado en AD FS ejecutando el siguiente comando de PowerShell 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   En mi caso, el comando es: 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. Solicitud de autenticación de inicio del servidor con la misma dirección IP que agregó en **authconfig.csv**.

   Para esta demostración, usaré [herramienta AD FS ayuda Claims x-Ray](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) para iniciar una solicitud. Si desea utilizar la herramienta de rayos x, siga las instrucciones 

   Escriba la instancia de servidor de federación y presione **probar autenticación** botón.</br> 
   ![model](media/ad-fs-risk-assessment-model/risk15.png) 

5. La autenticación se bloqueó porque se muestra a continuación.</br>
   ![model](media/ad-fs-risk-assessment-model/risk16.png)
 
Ahora que sabemos cómo crear y registrar el complemento, vamos a tutorial el código del complemento para entender la implementación mediante las clases y nuevas interfaces introducido con el modelo. 

## <a name="plug-in-code-walkthrough"></a>Tutorial de código del complemento

Abra el proyecto `ThreatDetectionModule.sln` con Visual Studio y, a continuación, abra el archivo principal **UserRiskAnalyzer.cs** desde el **el Explorador de soluciones** a la derecha de la pantalla</br>
![model](media/ad-fs-risk-assessment-model/risk17.png)
 
El archivo contiene la clase principal UserRiskAnalyzer que implementa la clase abstracta [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) e interfaz [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) para leer la dirección IP de la solicitud contexto, compare la dirección IP obtenida con las direcciones IP de carga desde la base de datos de AD FS y bloquear la solicitud si hay una coincidencia IP. Repasemos estos tipos con más detalle

### <a name="threatdetectionmodule-abstract-class"></a>Clase abstracta ThreatDetectionModule

Esta clase abstracta carga el complemento en la canalización de AD FS, lo que permite ejecutar el código del complemento en consonancia con el proceso de AD FS. 

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
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|Llamado por AD FS cuando se carga el complemento en su canalización| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|Lo llama cuando el complemento se descarga desde su canalización de AD FS| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|Llamado por AD FS en la actualización de la configuración |
|**Propiedad** |**Tipo** |**Definición**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|Cadena |Obtiene el nombre del proveedor propietario del complemento|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|Cadena |Obtiene el identificador del complemento|

En el complemento de nuestro ejemplo, usamos [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) y [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) métodos para leer las direcciones IP predefinida de la base de datos de AD FS. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) se llama cuando el complemento está registrado con AD FS al [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) se llama cuando se importa el CSV mediante el `Import-AdfsThreatDetectionModuleConfiguration` cmdlet. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>Interfaz IRequestReceivedThreatDetectionModule

Esto [interfaz](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) le permite implementar la evaluación de riesgos en el punto donde AD FS recibe la solicitud de autenticación, pero antes de usuario proporciona credenciales, es decir, en fase de solicitud recibido del proceso de autenticación. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

La interfaz incluye [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) pasa el método que le permite usar el contexto de la solicitud de autenticación en el parámetro de entrada requestContext que escribir la lógica de evaluación de riesgos. El parámetro requestContext es de tipo [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

El parámetro de entrada pasado es registrador que es de tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). El parámetro puede usarse para escribir el error, auditoría o depurar los mensajes a los registros de AD FS. 

El método devuelve [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 para el bloque y 2 para permitir) para AD FS que luego bloquea o permite la solicitud.

En nuestro ejemplo de complemento, [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) analiza la implementación del método la [clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) desde el [requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) parámetro y lo compara con todas las direcciones IP cargar desde la base de datos de AD FS. Si se encuentra una coincidencia, el método devuelve 2 para **bloque**, en caso contrario devuelve 1 para **permitir**. Según el valor devuelto, AD FS bloquea o permite que la solicitud. 

>[!NOTE]
>El ejemplo de complemento que se ha explicado anteriormente sólo implementa la interfaz IRequestReceivedThreatDetectionModule. Sin embargo, el modelo de evaluación del riesgo proporciona dos interfaces: IPreAuthenticationThreatDetectionModule (para implementar la fase de autenticación previa de durante de lógica de evaluación de riesgo) y IPostAuthenticationThreatDetectionModule (para implementar el riesgo evaluación de la lógica de durante la fase posterior a la autenticación). A continuación, se proporcionan los detalles de las dos interfaces. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interfaz IPreAuthenticationThreatDetectionModule 

Esto [interfaz](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) le permite implementar la lógica de evaluación de riesgos en el punto donde el usuario proporciona las credenciales, pero antes de que AD FS evalúa fase; es decir, la autenticación previa. 

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
La interfaz incluye [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) pasa el método que le permite usar la información en el [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), y [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parámetros de entrada al escribir su lógica de evaluación de riesgos de la autenticación previa. 

>[!NOTE]
>Para obtener una lista de propiedades pasados con cada tipo de contexto, visite [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), y [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) las definiciones de clases. 

El parámetro de entrada pasado es registrador que es de tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). El parámetro puede usarse para escribir el error, auditoría o depurar los mensajes a los registros de AD FS.

El método devuelve [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 si NotEvaluated, 1 para el bloque y 2 para permitir) para AD FS que luego bloquea o permite la solicitud. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interfaz IPostAuthenticationThreatDetectionModule

Esto [interfaz](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) le permite implementar la lógica de evaluación del riesgo después de que el usuario ha proporcionado las credenciales y ha realizado la autenticación, es decir, autenticación posterior a la fase de AD FS. 

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

La interfaz incluye [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) pasa el método que le permite usar la información en el [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), y [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parámetros de entrada al escribir su lógica de evaluación del riesgo posteriores a la autenticación. 

>[!NOTE]
> Para obtener una lista completa de propiedades pasados con cada tipo de contexto, consulte [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), y [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) las definiciones de clases. 

El parámetro de entrada pasado es registrador que es de tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). El parámetro puede usarse para escribir el error, auditoría o depurar los mensajes a los registros de AD FS. 

El método devuelve el [puntuación de riesgo](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) que puede usarse en la directiva de AD FS y las reglas de notificación. 

>[!NOTE]
>Para complementos para que funcione, debe derivar la clase principal (en este caso UserRiskAnalyzer) [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) clase abstracta y debe implementar al menos una de las tres interfaces que se ha descrito anteriormente. Una vez que se registra el archivo dll, AD FS comprueba qué interfaces se implementan y llama a ellos en una etapa adecuada en la canalización.

### <a name="faqs"></a>P+F

**¿Por qué debo crear estos complementos?**</br>
**R:** Estos complementos no sólo ofrecen funcionalidad adicional para proteger su entorno frente a ataques como contraseña aerosol ataques, pero también proporcionan la flexibilidad para crear su propia lógica de evaluación del riesgo según sus requisitos. 

**¿Donde se capturan los registros?**</br>
**R:** Puede escribir los registros de errores en registro de eventos "AD FS/Admin" mediante el método WriteAdminLogErrorMessage, registros de auditoría para "AD FS Auditing" seguridad de registro mediante el método WriteAuditMessage y depuración los registros de "Seguimiento de AD FS" registro de depuración mediante el método WriteDebugMessage. 

**¿Puede agregar estos complementos aumentar la latencia de proceso de autenticación de AD FS?**</br>
**R:** Impacto de la latencia se determinará por el tiempo dedicado a ejecutar la lógica de evaluación de riesgos que implementa. Se recomienda evaluar el impacto de latencia antes de implementar el complemento en el entorno de producción. 

**¿Por qué no pueden sugerir ADFS la lista de direcciones IP de riesgo, usuarios, etcetera?**</br>
**R:** Aunque no está disponible actualmente, estamos trabajando en la creación de la inteligencia para sugerir de IP de riesgo, usuarios, etc. en el modelo de evaluación de riesgo conectable. Se compartirá el lanzamiento pronto de fechas. 
