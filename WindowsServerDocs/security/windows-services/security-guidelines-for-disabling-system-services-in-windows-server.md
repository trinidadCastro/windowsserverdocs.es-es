---
title: Directrices de seguridad para los servicios del sistema en Windows Server 2016
description: Directrices de seguridad para deshabilitar los servicios en Windows Server 2016 con Experiencia de escritorio
ms.custom: na
ms.prod: windows-server
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 1711eb94b622775feaf02f6bada596fe03b08ea9
ms.sourcegitcommit: b8e120fc574450e9eee13e7315424137a43e6a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74044809"
---
# <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Guía para deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio

Se aplica a: Windows Server 2016

El sistema operativo Windows incluye muchos servicios del sistema que proporcionan una funcionalidad importante. Los distintos servicios tienen directivas de inicio diferentes: algunos se inician de manera predeterminada (automáticamente), algunos cuando es necesario (manualmente) y otros están deshabilitados de manera predeterminada y se deben habilitar de forma explícita antes de poder ejecutarse. Estos valores predeterminados se eligieron cuidadosamente para cada servicio a fin de equilibrar el rendimiento, la funcionalidad y la seguridad para los clientes típicos.

Sin embargo, es posible que algunos clientes empresariales prefieran un equilibrio más centrado en la seguridad para sus servidores y PC Windows, uno que disminuya la superficie expuesta a ataques al mínimo absoluto y, por lo tanto, deseen deshabilitar por completo todos los servicios que no son necesarios en sus entornos específicos. Para esos clientes, Microsoft® brinda estas directrices relacionadas con los servicios que se pueden deshabilitar de manera seguridad para este fin.

Las directrices solo se aplican a Windows Server 2016 con Experiencia de escritorio (a menos que se use como un reemplazo del escritorio para los usuarios finales). A partir de Windows Server 2019, estas directrices están configuradas de manera predeterminada. Cada servicio del sistema se clasifica por categorías como se indica a continuación:

-   **Se debe deshabilitar:** una empresa centrada en la seguridad probablemente preferirá deshabilitar este servicio y prescindir de su funcionalidad (más abajo aparecen otros detalles).
- **Se puede deshabilitar:** este servicio proporciona una funcionalidad útil para algunas empresas, pero no para todas, y las empresas centradas en la seguridad que no lo utilicen pueden deshabilitarlo sin problemas.
- **No deshabilitar:** la deshabilitación de este servicio afectará una funcionalidad fundamental o impedirá que características o roles específicos funcionen correctamente. Por lo tanto, no se debe deshabilitar.
-  **(Sin instrucciones):** las consecuencias de deshabilitar estos servicios no se han evaluado completamente. Por lo tanto, no se debe modificar la configuración predeterminada de estos servicios.


Los clientes pueden configurar sus servidores y PC Windows para deshabilitar los servicios seleccionados mediante las plantillas de seguridad de sus directivas de grupo o a través de la automatización de PowerShell. En algunos casos, las instrucciones incluyen una configuración específica de las directivas de grupo que deshabilita directamente la funcionalidad del servicio, como alternativa a la deshabilitación del servicio en sí.

Microsoft recomienda que los clientes deshabiliten los servicios siguientes y sus tareas programadas respectivas en Windows Server 2016 con Experiencia de escritorio:

Servicios: 
1. Administración de autenticación de Xbox Live
2. Partida guardada en Xbox Live

Tareas programadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(También puedes acceder a la información sobre todos los servicios detallados en este artículo si consultas la hoja de cálculo de Microsoft Excel adjunta: [Guía para deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))



### <a name="disabling-services-not-installed-by-default"></a>Deshabilitación de los servicios no instalados de manera predeterminada

Microsoft no recomienda aplicar las directivas para deshabilitar los servicios no instalados de manera predeterminada.
-  Si se instala la característica, el servicio suele ser necesario. La instalación del servicio o de la característica requiere derechos administrativos. Impide la instalación de la característica, no el inicio del servicio.
-  Bloquear el servicio de Microsoft Windows no impide que un administrador (o, en algunos casos, un usuario no administrador) instale un equivalente de terceros similar, quizás uno con un riesgo de seguridad mayor.
-  Una línea base o banco de pruebas que deshabilite un servicio de Windows no predeterminado (por ejemplo, W3SVC) dará a algunos auditores la impresión errónea de que la tecnología (por ejemplo, IIS) es inherentemente insegura y que no se debería usar nunca.
-  Si nunca se instala la característica (ni el servicio), esto solo agrega elementos innecesarios a la línea base y al trabajo de verificación.


En el caso de todos los servicios del sistema que se mencionan en este documentos, las dos tablas siguientes ofrecen una explicación de las columnas y las recomendaciones de Microsoft para habilitar y deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio: 



### <a name="explanation-of-columns"></a>Explicación de las columnas

| | |
|---|---|
|**Descripción del servicio**|   La descripción del servicio desde sc.exe qdescription.|
|**Nombre** |Nombre clave (interno) del servicio.|
|**Instalación** | *Siempre instalado*: El servicio se instala en Windows Server 2016 Core y Windows Server 2016 con Experiencia de escritorio. *Solo con Experiencia de escritorio*: El servicio se instala en Windows Server 2016 con Experiencia de escritorio, pero ***no*** se instala en Server Core. |
|**StartType**  |Tipo de inicio del servicio en Windows Server 2016|
|**Recomendación** |La recomendación o el consejo de Microsoft sobre la deshabilitación de este servicio en Windows Server 2016 es una implementación empresarial típica bien administrada y donde el servidor no se usa como un reemplazo del escritorio del usuario final.|
|**Comentarios** |Una explicación adicional.|



### <a name="explanation-of-microsoft-recommendations"></a>Explicación de las recomendaciones de Microsoft

| | |
|---|---|
|**No deshabilitar** |Este servicio no se debe deshabilitar.|
|**Se puede deshabilitar**| Este servicio se puede deshabilitar si no se usa la característica que admite.|
|**Ya deshabilitado**|  Este servicio está deshabilitado de manera predeterminada, por lo que no es necesario aplicar la directiva.|
|**Debe estar deshabilitado** |Este servicio nunca se debe habilitar en un sistema empresarial bien administrado.|



En las tablas siguientes se brindan las directrices de Microsoft sobre cómo deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio:



##  <a name="activex-installer-axinstsv"></a>Instalador de ActiveX (AxInstSV)

| | |
|---|---|
|   **Descripción del servicio** |   Proporciona la validación del Control de cuentas de usuario de la instalación de los controles ActiveX desde Internet y permite administrar la instalación de los controles ActiveX en función de la configuración de la directiva de grupo. Este servicio se inicia a petición y, si está deshabilitado, la instalación de los controles ActiveX se comportarán según la configuración predeterminada del explorador.    |
|   **Nombre del servicio**    |   AxInstSV    |
|   **Instalación**    |   Solo con Experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Se puede deshabilitar   |
|   **Comentarios**    |   Se puede deshabilitar si no se necesita la característica |




## <a name="alljoyn-router-service"></a>Servicio del enrutador de AllJoyn   

| | |
|---|---|
|   **Descripción del servicio** |   Enruta los mensajes de AllJoyn para los clientes locales de AllJoyn. Si este servicio se detiene, los clientes de AllJoyn que no tengan sus propios enrutadores integrados no podrán ejecutarse. |
|   **Nombre del servicio**    |   AJRouter    |
|   **Instalación**    |   Solo con Experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  | Sin instrucciones       |
|   **Comentarios**    |       |
| | |



## <a name="app-readiness"></a>Preparación de las aplicaciones

| | |
|---|---|
**Descripción del servicio** |   Prepara las aplicaciones para usarlas la primera vez que un usuario inicia sesión en este equipo y cuando se agregan aplicaciones nuevas.
**Nombre del servicio**    |   AppReadiness
**Instalación**    |   Solo con Experiencia de escritorio
**StartType**   |   Manual
**Recomendación**  |   No deshabilitar
**Comentarios**    |   
| | |



##  <a name="application-identity"></a>Identidad de aplicación

| | |       
|---|---|   
**Descripción del servicio** |   Determina y comprueba la identidad de una aplicación. La deshabilitación de este servicio impedirá que se apliquen las directivas de AppLocker.
**Nombre del servicio**    |   AppIDSvc
**Instalación**    |   Siempre instalado
**StartType**   |   Manual
**Recomendación**  |Sin instrucciones    
**Comentarios**    |   
|||     



##  <a name="application-information"></a>Información de las aplicaciones 

| | |       
|---|---|   
|   **Descripción del servicio** |   Facilita la ejecución de aplicaciones interactivas con privilegios administrativos adicionales.  Si se detiene este servicio, los usuarios no podrán iniciar las aplicaciones con los privilegios administrativos adicionales que podrían necesitar para realizar las tareas de usuario deseadas.
|   **Nombre del servicio**    |   Appinfo
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   Admite la elevación del control de cuentas de usuario en el mismo escritorio
|||     



##  <a name="application-layer-gateway-service"></a>Servicio de puerta de enlace de nivel de aplicación       

| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona compatibilidad con los complementos de protocolos de terceros para la Conexión compartida a Internet.
|   **Nombre del servicio**    |   ALG
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |Sin instrucciones    
|   **Comentarios**    |   
|||     



##  <a name="application-management"></a>Administración de aplicaciones      

| | |           
|---|---|       
|   **Descripción del servicio** |   Procesa las solicitudes de instalación, eliminación y enumeración del software implementado a través de la directiva de grupo. Si el servicio está deshabilitado, los usuarios no podrán instalar, quitar ni enumerar el software implementado a través de la directiva de grupo. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   AppMgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="appx-deployment-service-appxsvc"></a>Servicios de implementación de AppX (AppXSVC)       

| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura para implementar las aplicaciones de Microsoft Store. Este servicio se inicia a petición y, si está deshabilitado, las aplicaciones de Microsoft Store no se implementarán en el sistema y es posible que no funcionen correctamente.
|   **Nombre del servicio**    |   AppXSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="auto-time-zone-updater"></a>Actualizador automático de la zona horaria.           

| | |           
|---|---|           
|   **Descripción del servicio** |   Establece automáticamente la zona horaria del sistema.
|   **Nombre del servicio**    |   tzautoupdate
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="background-intelligent-transfer-service"></a>Servicio de transferencia inteligente en segundo plano          

| | |           
|---|---|   
|   **Descripción del servicio** |   Transfiere los archivos en segundo plano mediante el ancho de banda de red inactivo. Si el servicio se deshabilita, las aplicaciones que dependen de BITS, como Windows Update o MSN Explorer, no podrán descargar automáticamente programas ni otro tipo de información.
|   **Nombre del servicio**    |   BITS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         




## <a name="background-tasks-infrastructure-service"></a>Servicio de la infraestructura de tareas en segundo plano      

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de la infraestructura de Windows que controla cuáles son las tareas en segundo plano que se pueden ejecutar en el sistema.
|   **Nombre del servicio**    |   BrokerInfrastructure
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="base-filtering-engine"></a>Motor de filtrado de base            

| | |           
|---|---|       
|   **Descripción del servicio** |   El Motor de filtrado de base (BFE) es un servicio que administra las directivas del protocolo de seguridad de Internet (IPsec) y del firewall e implementa el filtrado en modo usuario. Detener o deshabilitar el servicio BFE disminuirá considerablemente la seguridad del sistema. También se generará un comportamiento imprevisible en las aplicaciones de firewall y administración de IPsec.
|   **Nombre del servicio**    |   BFE
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="bluetooth-support-service"></a>Servicio de compatibilidad con Bluetooth            

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de Bluetooth admite la detección y la asociación de dispositivos Bluetooth remotos.  Detener o deshabilitar este servicio podría provocar que los dispositivos Bluetooth ya instalados no funcionen correctamente e impedir que se detecten o asocien dispositivos nuevos.
|   **Nombre del servicio**    |   bthserv
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Se puede deshabilitar si no se usa. Otro mecanismo de deshabilitación: https://technet.microsoft.com/library/dd252791.aspx
|||         




## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio de usuario se usa en los escenarios de la plataforma de dispositivos conectados.
|   **Nombre del servicio**    |   CDPUserSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         




##  <a name="certificate-propagation"></a>Propagación de certificados     

| | |           
|---|---|
|   **Descripción del servicio** |   Copia los certificados del usuario y los certificados raíz de las tarjetas inteligentes en el almacén de certificados del usuario actual, detecta cuando una tarjeta inteligente se inserta en un lector de tarjeta inteligente y, si es necesario, instala el minicontrolador Plug and Play de tarjetas inteligentes.
|   **Nombre del servicio**    |   CertPropSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="client-license-service-clipsvc"></a>Servicio de licencias de cliente (ClipSVC)        

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad de infraestructura con Microsoft Store. Este servicio se inicia a petición y, si está deshabilitado, las aplicaciones que se compran a través de Microsoft Store no funcionarán correctamente.
|   **Nombre del servicio**    |   ClipSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="cng-key-isolation"></a>Aislamiento de claves CNG

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de aislamiento de claves CNG está hospedado en el proceso LSA. El servicio proporciona aislamiento de proceso de claves para las claves privadas y las operaciones criptográficas asociadas según lo requieren los criterios comunes. El servicio almacena y usa claves de larga duración en un proceso seguro que cumple los requisitos de los criterios comunes.
|   **Nombre del servicio**    |   KeyIso
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="com-event-system"></a>Sistema de eventos COM+       

| | |           
|---|---|       
|   **Descripción del servicio** |   Admite el Servicio de notificación de eventos del sistema (SENS), que proporciona la distribución automática de eventos a los componentes del Modelo de objetos componentes (COM). Si se detiene este servicio, SENS se cerrará y no podrá ofrecer notificaciones de inicio ni de cierre de sesión. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   EventSystem
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="com-system-application"></a>Aplicación del sistema COM+     

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la configuración y el seguimiento de los componentes basados en el Modelo de objetos componentes (COM+). Si se detiene el servicio, la mayoría de los componentes COM+ no funcionarán correctamente. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   COMSysApp
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="computer-browser"></a>Explorador de equipos        

| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene una lista actualizada de los equipos en la red y proporciona esta lista a los equipos designados como exploradores. Si se detiene este servicio, esta lista no se actualizará ni mantendrá. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Browser
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="connected-devices-platform-service"></a>Servicio de plataforma de dispositivos conectados       

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio se usa en escenarios de dispositivos conectados y Universal Glass.
|   **Nombre del servicio**    |   CDPSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="connected-user-experiences-and-telemetry"></a>Experiencias del usuario y telemetría asociadas     

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio Experiencias del usuario y telemetría asociadas proporciona características compatibles con las experiencia del usuario conectado y en aplicación. Asimismo, este servicio administra la recopilación y transmisión de la información de diagnóstico y uso basada en los eventos (se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando la configuración de opciones de diagnóstico y privacidad de uso está habilitada en la opción Comentarios y diagnósticos.
|   **Nombre del servicio**    |   DiagTrack
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="contact-data"></a>Datos de contacto        

| | |           
|---|---|       
|   **Descripción del servicio** |   Indexa los datos de contacto para poder buscar los contactos rápidamente. Si detienes o deshabilitas este servicio, es posible que falten contactos en tus resultados de búsqueda.
|   **Nombre del servicio**    |   PimIndexMaintenanceSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         



## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **Descripción del servicio** |   Administra la comunicación entre los componentes del sistema.
|   **Nombre del servicio**    |   CoreMessagingRegistrar
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="credential-manager"></a>Administrador de credenciales           

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un almacenamiento seguro y la recuperación de credenciales para usuarios, aplicaciones y paquetes de servicios de seguridad.
|   **Nombre del servicio**    |   VaultSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="cryptographic-services"></a>Servicios criptográficos           

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona tres servicios de administración: Servicio de catálogo de base de datos, que confirma las firmas de los archivos de Windows y permite la instalación de programas nuevos; Servicio de raíz protegida, que agrega y quita certificados de entidades de certificación raíz de confianza del equipo, y Servicio automático de actualización de certificados raíz, que recupera certificados raíz de Windows Update y habilita escenarios como SSL. Si se detiene este servicio, los servicios de administración mencionados no funcionarán correctamente. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   CryptSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="data-sharing-service"></a>Servicio de uso compartido de datos         

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona servicios de administración de datos entre aplicaciones.
|   **Nombre del servicio**    |   DsSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCP (Recopilación y publicación de datos) admite aplicaciones de origen para cargar los datos a la nube.
|   **Nombre del servicio**    |   DcpSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="dcom-server-process-launcher"></a>Iniciador de procesos de servidor DCOM         

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCOMLAUNCH inicia los servidores COM y DCOM en respuesta a las solicitudes de activación de objetos. Si este servicio se detiene o se deshabilita, los programas que usen COM o DCOM no funcionarán correctamente. Por ello, es muy recomendable que ejecutes el servicio DCOMLAUNCH.
|   **Nombre del servicio**    |   DcomLaunch
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |Sin instrucciones    
|   **Comentarios**    |   
|||         



##  <a name="device-association-service"></a>Servicio de asociación de dispositivos      

| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita el emparejamiento entre el sistema y los dispositivos cableados o inalámbricos.
|   **Nombre del servicio**    |   DeviceAssociationService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="device-install-service"></a>Servicio de instalación de dispositivos

| | |
|---|---|
|   **Descripción del servicio** |   Habilita un equipo para que reconozca y se adapte a los cambios de hardware con el menor esfuerzo por parte del usuario. Si se detiene o deshabilita este servicio, el sistema se volverá inestable.
|   **Nombre del servicio**    |   DeviceInstall
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones
|   **Comentarios**    |
|||



##  <a name="device-management-enrollment-service"></a>Servicio de inscripción de administración de dispositivos        

| | |           
|---|---|       
|   **Descripción del servicio** |   Realiza actividades de inscripción de dispositivos para la administración de dispositivos.
|   **Nombre del servicio**    |   DmEnrollmentSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="device-setup-manager"></a>Administrador de configuración del dispositivo         

| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita la detección, descarga e instalación de software relacionado con el dispositivo. Si se deshabilita este servicio, es posible que los dispositivos se configuren con software obsoleto y no funcionen correctamente.
|   **Nombre del servicio**    |   DsmSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="devquery-background-discovery-broker"></a>Agente de detección en segundo plano de DevQuery         

| | |           
|---|---|           
|   **Descripción del servicio** |   Permite a las aplicaciones detectar dispositivos con una tarea en segundo plano.
|   **Nombre del servicio**    |   DevQueryBroker
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="dhcp-client"></a>Cliente DHCP          

| | |           
|---|---|       
|   **Descripción del servicio** |   Registra y actualiza las direcciones IP y los registros DNS de este equipo. Si este servicio se detiene, el equipo no recibirá direcciones IP dinámicas ni actualizaciones de DNS. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Dhcp
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="diagnostic-policy-service"></a>Servicio de directivas de diagnóstico            

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de directivas de diagnóstico permite la detección, solución y resolución de problemas para componentes de Windows.  Si este servicio se detiene, los diagnósticos ya no funcionarán.
|   **Nombre del servicio**    |   DPS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="diagnostic-service-host"></a>Host de servicio de diagnóstico     

| | |           
|---|---|       
|   **Descripción del servicio** |   El Servicio de directivas de diagnóstico usa el Host de servicio de diagnóstico para hospedar los diagnósticos que deben ejecutarse en un contexto de servicio local.  Si se detiene este servicio, los diagnósticos que dependan de él dejarán de funcionar.
|   **Nombre del servicio**    |   WdiServiceHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="diagnostic-system-host"></a>Host de sistema de diagnóstico           

| | |           
|---|---|       
|   **Descripción del servicio** |   El Servicio de directivas de diagnóstico usa el Host de sistema de diagnóstico para hospedar los diagnósticos que deben ejecutarse en un contexto de sistema local.  Si se detiene este servicio, los diagnósticos que dependan de él dejarán de funcionar.
|   **Nombre del servicio**    |   WdiSystemHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="distributed-link-tracking-client"></a>Cliente de seguimiento de vínculos distribuidos.            

| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene los vínculos entre archivos NTFS dentro de un equipo o entre los equipos de una red.
|   **Nombre del servicio**    |   TrkWks
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="distributed-transaction-coordinator"></a>Coordinador de transacciones distribuidas     

| | |           
|---|---|   
|   **Descripción del servicio** |   Coordina las transacciones que abarcan varios administradores de recursos, como las bases de datos, las colas de mensajes y los sistemas de archivos. Si se detiene este servicio, se producirá un error en estas transacciones. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   MSDTC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de enrutamiento de mensajes de inserción WAP
|   **Nombre del servicio**    |   dmwappushservice
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   El servicio es necesario en dispositivos cliente para Intune, MDM y tecnologías de administración similares y también para el Filtro de escritura unificado. No es necesario para el servidor.
|||         



##  <a name="dns-client"></a>Cliente DNS      

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio Cliente DNS (dnscache) almacena en caché nombres del Sistema de nombres de dominio (DNS) y registra el nombre completo del equipo para este equipo. Si el servicio se para, los nombres DNS seguirán resolviéndose. Sin embargo, los resultados de las consultas de nombres DNS no se almacenarán en caché y el nombre de los equipos no se registrará. Si el servicio se deshabilita, cualquier servicio que dependa explícitamente de él no se podrá iniciar.
|   **Nombre del servicio**    |   Dnscache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="downloaded-maps-manager"></a>Administrador de mapas descargados     

| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de Windows para que las aplicaciones obtengan acceso a mapas descargados. Este servicio lo pide la aplicación que accederá a los mapas descargados. Si deshabilitas este servicio, las aplicaciones no obtendrán acceso a los mapas.
|   **Nombre del servicio**    |   MapsBroker
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Si se deshabilita, se interrumpen las aplicaciones que se basan en el servicio; se puede deshabilitar si las aplicaciones no se basan en él.
|||         



## <a name="embedded-mode"></a>Modo incrustado            

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de modo incrustado habilita escenarios relacionados con las aplicaciones en segundo plano.  Si se deshabilita este servicio, se impedirá que se activen las aplicaciones en segundo plano.
|   **Nombre del servicio**    |   embeddedmode
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="encrypting-file-system-efs"></a>Sistema de archivos cifrados (EFS)

| | |                   
|---|---|           
|   **Descripción del servicio** | Proporciona la tecnología de cifrado de archivos principal que se usa para almacenar archivos cifrados en volúmenes con el sistema de archivos NTFS. Si este servicio se detiene o se deshabilita, las aplicaciones no podrán tener acceso a los archivos cifrados.            
|   **Nombre del servicio**  |  EFS            
|   **Instalación**  |  Siempre instalado           
|   **StartType**   |  Manual           
|   **Recomendación**  | Sin instrucciones           
|   **Comentarios**   |
|||                 



## <a name="enterprise-app-management-service"></a>Servicio de administración de aplicaciones de empresa            

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la administración de aplicaciones de empresa.
|   **Nombre del servicio**    |   EntAppSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="extensible-authentication-protocol"></a>Protocolo de autenticación extensible           

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio Protocolo de autenticación extensible (EAP) proporciona autenticación de red en escenarios como 802.1x con cable e inalámbrica, VPN y Protección de acceso a redes (NAP).  EAP también proporciona interfaces de programación de aplicaciones (API) usadas por clientes de acceso a redes, incluidos clientes inalámbricos y VPN, durante el proceso de autenticación.  Si deshabilitas este servicio, este equipo no podrá obtener acceso a redes que requieran autenticación EAP.
|   **Nombre del servicio**    |   EapHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="function-discovery-provider-host"></a>Host de proveedor de detección de funciones         

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio FDPHOST hospeda los proveedores de detección de redes FD (detección de funciones). Estos proveedores FD proporcionan servicios de detección de redes para el protocolo de detección de servicios simple (SSDP) y el protocolo de detección de servicios web (WS-D). Si se detiene o deshabilita el servicio FDPHOST, se deshabilitará la detección de redes para estos protocolos cuando se use FD. Si este servicio no está disponible, los servicios de red que usen FD y estén basados en estos protocolos de detección no podrán encontrar dispositivos o recursos de red.
|   **Nombre del servicio**    |   fdPHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="function-discovery-resource-publication"></a>Publicación de recurso de detección de función      

| | |           
|---|---|       
|   **Descripción del servicio** |   Publica este equipo y los recursos conectados a él para poder detectarlos a través de la red.  Si se detiene este servicio, los recursos de red dejarán de publicarse y no podrán detectarlos otros equipos de la red.
|   **Nombre del servicio**    |   FDResPub
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="geolocation-service"></a>Servicio de geolocalización          

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio supervisa la ubicación actual del sistema y administra las geovallas (una ubicación geográfica con eventos asociados).  Si desactivas este servicio, las aplicaciones no podrán usar ni recibir notificaciones de geolocalización o geovallas.
|   **Nombre del servicio**    |   lfsvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Si se deshabilita, se interrumpen las aplicaciones que se basan en el servicio; se puede deshabilitar si las aplicaciones no se basan en él.
|||         



##  <a name="group-policy-client"></a>Cliente de directiva de grupo     

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio es responsable de la aplicación de la configuración establecida por los administradores para el equipo y los usuarios mediante el componente de la directiva de grupo. Si el servicio se deshabilita, la configuración no se aplicará y las aplicaciones y componentes no se podrán administrar mediante la directiva de grupo. Todo componente o aplicación que dependa del componente de la directiva de grupo podrían no funcionar si se deshabilita el servicio.
|   **Nombre del servicio**    |   gpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         




## <a name="human-interface-device-service"></a>Servicio de dispositivos de interfaz humana           

| | |           
|---|---|       
|   **Descripción del servicio** |   Activa y mantiene el uso de botones de acceso directo predefinidos en los teclados, controles remotos y otros dispositivos multimedia. Se recomienda mantener este servicio en ejecución.
|   **Nombre del servicio**    |   hidserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="hv-host-service"></a>Servicio de host HV     

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para que el hipervisor de Hyper-V proporcione contadores de rendimiento por partición al sistema operativo del host.
|   **Nombre del servicio**    |   HvHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Optimizadores de rendimiento de las máquinas virtuales invitadas. Hoy no se usa, excepto en las máquinas virtuales rellenadas explícitamente, pero se usará en Protección de aplicaciones.
|||         



## <a name="hyper-v-data-exchange-service"></a>Servicio de intercambio de datos de Hyper-V        

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona un mecanismo para intercambiar datos entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre del servicio**    |   vmickvpexchange
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-guest-service-interface"></a>Interfaz de servicio de invitado de Hyper-V          

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para que el host de Hyper-V interactúe con servicios específicos que se ejecutan en la máquina virtual.
|   **Nombre del servicio**    |   vmicguestinterface
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-guest-shutdown-service"></a>Servicio de cierre de invitado de Hyper-V           

| | |           
|---|---|       
|   **Descripción del servicio** |   Ofrece un mecanismo para apagar el sistema operativo de esta máquina virtual desde las interfaces de administración del equipo físico.
|   **Nombre del servicio**    |   vmicshutdown
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-heartbeat-service"></a>Servicio de latido de Hyper-V
| | |
|---|---|
|   **Descripción del servicio** |   Supervisa el estado de la máquina virtual mediante la notificación de un latido a intervalos regulares. Este servicio ayuda a identificar las máquinas virtuales en ejecución que dejaron de responder.
|   **Nombre del servicio**    |   vmicheartbeat
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||



## <a name="hyper-v-powershell-direct-service"></a>Servicio directo de PowerShell de Hyper-V            

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un mecanismo para administrar la máquina virtual con PowerShell a través de una sesión de máquina virtual sin una red virtual.
|   **Nombre del servicio**    |   vmicvmsession
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servicio de virtualización de escritorio remoto de Hyper-V            

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona una plataforma para la comunicación entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre del servicio**    |   vmicrdv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-time-synchronization-service"></a>Servicio de sincronización de hora de Hyper-V         

| | |           
|---|---|       
|   **Descripción del servicio** |   Sincroniza la hora del sistema de esta máquina virtual con la hora del sistema del equipo físico.
|   **Nombre del servicio**    |   vmictimesync
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de instantáneas de volumen de Hyper-V         

| | |           
|---|---|           
|   **Descripción del servicio** |   Coordina las comunicaciones que requieren el uso del Servicio de instantáneas de volumen para realizar copias de seguridad de las aplicaciones y los datos de esta máquina virtual desde el sistema operativo del equipo físico.
|   **Nombre del servicio**    |   vmicvss
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Consulta HvHost
|||         



## <a name="ike-and-authip-ipsec-keying-modules"></a>Módulos de creación de claves de IPsec para IKE y AuthIP          

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio IKEEXT hospeda los módulos de creación de claves de Intercambio de claves por red (IKE) y protocolo de Internet autenticado (AuthIP). Estos módulos de creación de claves se usan para la autenticación y el intercambio de claves en el protocolo de seguridad de Internet (IPsec). Si se detiene o deshabilita el servicio IKEEXT, se deshabilitará el intercambio de claves IKE y AuthIP con equipos del mismo nivel. Normalmente, IPsec está configurado para usar IKE o AuthIP; es posible que detener o deshabilitar el servicio IKEEXT provoque errores de IPsec y ponga en peligro la seguridad del sistema. Por ello, es muy recomendable que ejecutes el servicio IKEEXT.
|   **Nombre del servicio**    |   IKEEXT
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |    
|||         



## <a name="interactive-services-detection"></a>Detección de servicios interactivos           

| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita la notificación al usuario sobre la entrada de usuario para los servicios interactivos; esto habilita el acceso a los cuadros de diálogo creados por los servicios interactivos cuando aparecen. Si se detiene este servicio, dejarán de funcionar las notificaciones de cuadros de diálogo de nuevos servicios interactivos y es posible que no se pueda obtener acceso a los cuadros de diálogo de servicios interactivos. Si se deshabilita este servicio, dejarán de funcionar las notificaciones y el acceso a los cuadros de diálogo de nuevos servicios interactivos.
|   **Nombre del servicio**    |   UI0Detect
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="internet-connection-sharing-ics"></a>Conexión compartida a Internet (ICS)            

| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona servicios de traducción de direcciones de red, direccionamiento, resolución de nombres y prevención de intrusiones para una red doméstica o de oficina pequeña.
|   **Nombre del servicio**    |   SharedAccess
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Se requiere para los clientes que se usan como zonas Wi-Fi y también en ambos extremos de la proyección de Miracast. Es posible bloquear la Conexión compartida a Internet con la configuración de GPO, "Prohibir el uso de Conexión compartida a Internet en su red de dominio DNS".
|||         



## <a name="ip-helper"></a>Aplicación auxiliar IP            

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona conectividad de túnel con tecnologías de transición IPv6 (6to4, ISATAP, Proxy de puerto y Teredo) e IP-HTTPS. Si se detiene este servicio, el equipo no contará con los beneficios de conectividad mejorada que ofrecen estas tecnologías.
|   **Nombre del servicio**    |   iphlpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         




##  <a name="ipsec-policy-agent"></a>Agente de directiva IPsec      

| | |           
|---|---|       
|   **Descripción del servicio** |   El protocolo de seguridad de Internet (IPsec) admite la autenticación del mismo nivel de la red, la autenticación de los orígenes de los datos, la integridad de los datos, la confidencialidad de los datos (cifrado) y la protección de la reproducción.  Este servicio aplica las directivas IPSec creadas a través del complemento Directivas de seguridad IP o de la herramienta de la línea de comandos "netsh ipsec".  Si detienes este servicio, es posible que experimentes problemas de conectividad de red si la directiva requiere que las conexiones usen IPSec.  Además, la administración remota del Firewall de Windows no está disponible cuando se detiene este servicio.
|   **Nombre del servicio**    |   PolicyAgent
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="kdc-proxy-server-service-kps"></a>Servicio de servidor proxy KDC (KPS)      

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de servidor proxy KDC se ejecuta en servidores perimetrales para actuar como proxy para mensajes de protocolo de Kerberos para controladores de dominio en la red corporativa.
|   **Nombre del servicio**    |   KPSSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones    
|   **Comentarios**    |   
|||         



## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KTMRM para DTC (Coordinador de transacciones distribuidas)            

| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina transacciones entre el coordinador de transacciones distribuidas (MSDTC) y el administrador de transacciones de kernel (KTM). Si no es necesario, es recomendable que este servicio permanezca detenido. Si es necesario, tanto MSDTC como KTM iniciarán el servicio automáticamente. Si se deshabilita este servicio, cualquier transacción de MSDTC que interactúe con un administrador de transacciones de kernel no se podrá realizar y cualquier servicio que dependa explícitamente de él no podrá iniciarse.
|   **Nombre del servicio**    |   KtmRm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="link-layer-topology-discovery-mapper"></a>Asignador de detección de topologías de nivel de vínculo        

| | |       
|---|---|       
|   **Descripción del servicio** |   Crea un mapa de red con información sobre la topología de dispositivos y de equipos (conectividad) y los metadatos que describen cada equipo y dispositivo.  Si se deshabilita este servicio, el mapa de red no funcionará correctamente.
|   **Nombre del servicio**    |   lltdsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Se puede deshabilitar si no existen dependencias del mapa de red.
|||         



## <a name="local-session-manager"></a>Administrador de sesión local                    

| | |                   
|---|---|   
|   **Descripción del servicio** |   Servicio central de Windows que administra las sesiones de usuario locales. Si se detiene o deshabilita este servicio, el sistema se volverá inestable.    
|   **Nombre del servicio**    |   LSM |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Automático   |
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||                 



## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Recopilador estándar del concentrador de diagnósticos de Microsoft (R)         

| | |           
|---|---|           
|   **Descripción del servicio** |   Servicio central de Windows que administra las sesiones de usuario locales. Si se detiene o deshabilita este servicio, el sistema se volverá inestable.
|   **Nombre del servicio**    |   LSM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="microsoft-account-sign-in-assistant"></a>Ayudante para el inicio de sesión de cuenta Microsoft
| | |
|---|---|
|   **Descripción del servicio** |   Permite al usuario iniciar sesión mediante los servicios de identidad de cuentas Microsoft. Si se detiene este servicio, los usuarios no podrán iniciar sesión en el equipo con sus cuentas Microsoft.
|   **Nombre del servicio**    |   wlidsvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Las cuentas Microsoft no están disponibles en Windows Server.
|||



##  <a name="microsoft-app-v-client"></a>Cliente de Microsoft App-V      

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra aplicaciones virtuales y usuarios de App-V.
|   **Nombre del servicio**    |   AppVClient
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="microsoft-iscsi-initiator-service"></a>Servicio del iniciador iSCSI de Microsoft            

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las sesiones SCSI de Internet (iSCSI) desde este equipo hacia los dispositivos de destino iSCSI remotos. Si se detiene este servicio, el equipo no podrá iniciar sesión en los destinos iSCSI ni tener acceso a ellos. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   MSiSCSI
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Los datos de diagnóstico indican que se usa en un cliente y también en un servidor. Deshabilitarlo no brinda ningún beneficio.
|||         



## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona aislamiento de procesos de claves criptográficas usadas para autenticarse en los proveedores de identidades asociados de un usuario. Si se deshabilita este servicio, todos los usos y administración de estas claves dejarán de estar disponibles, lo cual incluye el inicio de sesión de máquina y el inicio de sesión único para aplicaciones y sitios web. Este servicio se inicia y detiene automáticamente. Es recomendable no reconfigurar el servicio.
|   **Nombre del servicio**    |   NgcSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Es necesario para los inicios de sesión de PIN/Hello, que no son compatibles en el servidor.
|||         



## <a name="microsoft-passport-container"></a>Contenedor de Microsoft Passport         

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las claves de identidad de usuario locales para autenticar al usuario en los proveedores de identidad, así como las tarjetas inteligentes virtuales del TPM. Si se deshabilita este servicio, no se podrá acceder a las claves de identidad de usuario locales y las tarjetas inteligentes virtuales del TPM. Es recomendable no reconfigurar el servicio.
|   **Nombre del servicio**    |   NgcCtnrSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="microsoft-software-shadow-copy-provider"></a>Proveedor de instantáneas de software de Microsoft          

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las instantáneas de volumen basadas en software y tomadas por el Servicio de instantáneas de volumen. Si se detiene el servicio, no se podrán administrar las instantáneas de volumen basadas en software. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   swprv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="microsoft-storage-spaces-smp"></a>SMP de Espacios de almacenamiento de Microsoft         

| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio host para el proveedor de administración de Espacios de almacenamiento de Microsoft. Si se detiene o deshabilita este servicio, Espacios de almacenamiento no se puede administrar.
|   **Nombre del servicio**    |   smphost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Sin este servicio, las API de administración de almacenamiento presentarán errores. Por ejemplo: "Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         



## <a name="nettcp-port-sharing-service"></a>Servicio de uso compartido de puertos Net.Tcp         

| | |           
|---|---|       
|   **Descripción del servicio** |   Ofrece la posibilidad de compartir puertos TCP a través del protocolo net.tcp.
|   **Nombre del servicio**    |   NetTcpPortSharing
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **Descripción del servicio** |   Mantiene un canal seguro entre este equipo y el controlador de dominio para la autenticación de usuarios y servicios. Si se detiene este servicio, el equipo no podrá autenticar usuarios y servicios y el controlador de dominio no puede registrar registros DNS. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Netlogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="network-connection-broker"></a>Agente de conexión de red            

| | |           
|---|---|       
|   **Descripción del servicio** |   Conexiones de los agentes que permiten que las aplicaciones de Microsoft Store reciban notificaciones de Internet.
|   **Nombre del servicio**    |   NcbService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



##  <a name="network-connections"></a>Conexiones de red         

| | |           
|---|---|   
|   **Descripción del servicio** |   Administra objetos en la carpeta Conexiones de red y acceso telefónico, donde se pueden ver conexiones de red de área local y remotas.
|   **Nombre del servicio**    |   Netman
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="network-connectivity-assistant"></a>Asistente para la conectividad de red      

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la notificación de estado de DirectAccess para componentes de UI.
|   **Nombre del servicio**    |   NcaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="network-list-service"></a>Servicio de lista de redes        

| | |           
|---|---|   
|   **Descripción del servicio** |   Identifica las redes a las que se conectó el equipo, recopila y almacena las propiedades de estas redes y notifica a las aplicaciones cuando estas propiedades cambian.
|   **Nombre del servicio**    |   netprofm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="network-location-awareness"></a>Reconocimiento de ubicación de red           

| | |           
|---|---|       
|   **Descripción del servicio** |   Recopila y almacena información de configuración de la red y notifica a los programas cuando esta información se modifica. Si se detiene este servicio, es posible que la información de configuración no esté disponible. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   NlaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="network-setup-service"></a>Servicio de configuración de red       

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de configuración de red administra la instalación de controladores de red y permite la configuración de valores de red de bajo nivel.  Si se detiene este servicio, pueden cancelarse las instalaciones de controladores que estén en curso.
|   **Nombre del servicio**    |   NetSetupSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="network-store-interface-service"></a>Servicio Interfaz de almacenamiento en red      

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio entrega notificaciones de red (por ejemplo, interfaces agregadas/eliminadas, etc.) a los clientes en modo usuario. Si se detiene este servicio, se perderá la conectividad de la red. Si se deshabilita este servicio, no se iniciará ningún servicio que dependa de él de manera explícita.
|   **Nombre del servicio**    |   nsi
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="offline-files"></a>Archivos sin conexión            

| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de archivos sin conexión realiza actividades de mantenimiento en la caché de archivos sin conexión, responde a eventos de inicio y cierre de sesión del usuario, implementa la información interna de la API pública y procesa eventos interesantes para los interesados en las actividades de archivos sin conexión y los cambios de estado de la caché.
|   **Nombre del servicio**    |   CscService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="optimize-drives"></a>Optimizar unidades          

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite que el equipo funcione de manera más eficiente al optimizar los archivos en las unidades de almacenamiento.
|   **Nombre del servicio**    |   defragsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="performance-counter-dll-host"></a>DLL de host del Contador de rendimiento         

| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita a los usuarios remotos y los procesos de 64 bits para consultar los contadores de rendimiento proporcionados por las DLL de 32 bits. Si se detiene este servicio, solo los usuarios locales y los procesos de 32 bits podrán consultar los contadores de rendimiento proporcionados por las DLL de 32 bits.
|   **Nombre del servicio**    |   PerfHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones    
|   **Comentarios**    |   
|||         



## <a name="performance-logs--alerts"></a>Registros y alertas de rendimiento            

| | |           
|---|---|   
|   **Descripción del servicio** |   Registros y alertas de rendimiento recopila los datos de rendimiento de equipos locales o remotos según parámetros de programación preconfigurados y, después, escribe los datos en un registro o activa una alerta. Si se detiene este servicio, no se recopilará información de rendimiento. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   pla
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="phone-service"></a>Servicio telefónico       

| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el estado de telefonía en el dispositivo
|   **Nombre del servicio**    |   PhoneSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Usado por aplicaciones VoIP modernas
|||         



##      <a name="plug-and-play"></a>Plug and Play       

| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita un equipo para que reconozca y se adapte a los cambios de hardware con el menor esfuerzo por parte del usuario. Si se detiene o deshabilita este servicio, el sistema se volverá inestable.
|   **Nombre del servicio**    |   PlugPlay
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="portable-device-enumerator-service"></a>Servicio enumerador de dispositivos portátiles           

| | |           
|---|---|       
|   **Descripción del servicio** |   Exige el cumplimiento de directivas de grupo para dispositivos de almacenamiento extraíble. Permite que aplicaciones como el Reproductor de Windows Media y el Asistente para la importación de imágenes transfieran y sincronicen el contenido mediante el uso de dispositivos de almacenamiento extraíble.
|   **Nombre del servicio**    |   WPDBusEnum
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="power"></a>Alimentación            

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la directiva de energía y la entrega de notificaciones de dicha directiva.
|   **Nombre del servicio**    |   Alimentación
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="print-spooler"></a>Print Spooler            

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio pone en cola los trabajos de impresión y administra la interacción con la impresora.  Si lo desactiva, no podrá imprimir ni ver las impresoras.
|   **Nombre del servicio**    |   Cola de impresión
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar si no se trata de un servidor de impresión ni un controlador de dominio.
|   **Comentarios**    |   En un controlador de dominio, la instalación del rol del controlador de dominio agrega un subproceso al servicio del administrador de trabajos en cola que es responsable de realizar la eliminación de las impresiones, quitando de Active Directory los objetos de la cola de impresión obsoleta.  Si el servicio del administrador de trabajos en cola no se ejecuta en al menos un controlador de dominio de cada sitio, AD no tiene ninguna forma de quitar las colas antiguas que ya no existen. [https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/](https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/ )
|||         



##  <a name="printer-extensions-and-notifications"></a>Extensiones y notificaciones de impresora        

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio abre cuadros de diálogo personalizados de la impresora y administra notificaciones desde un servidor de impresión o una impresora remotos. Si lo desactiva, no podrá ver las extensiones ni las notificaciones de impresora.
|   **Nombre del servicio**    |   PrintNotify
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar si no se trata de un servidor de impresión.
|   **Comentarios**    |   
|||         



##  <a name="problem-reports-and-solutions-control-panel-support"></a>Ayuda del Panel de control de Informes de problemas y soluciones     

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio proporciona ayuda para ver, enviar y eliminar los informes de problemas del nivel de sistema para el Panel de control de Informes de problemas y soluciones.
|   **Nombre del servicio**    |   wercplsupport
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="program-compatibility-assistant-service"></a>Servicio Asistente para la compatibilidad de programas     

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio proporciona soporte al Asistente para la compatibilidad de programas (PCA).  El PCA supervisa los programas que instala y ejecuta el usuario, y detecta los problemas de compatibilidad conocidos. Si se detiene este servicio, el PCA no funcionará correctamente.
|   **Nombre del servicio**    |   PcaSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



##  <a name="quality-windows-audio-video-experience"></a>Windows Audio Video Experience (qWAVE)      

| | |           
|---|---|   
|   **Descripción del servicio** |   Windows Audio Video Experience (qWave) es una plataforma de red para aplicaciones de streaming de audio y vídeo (AV) en redes domésticas IP. qWave mejora el rendimiento y la confiabilidad del streaming de audio y vídeo al garantizar la calidad de servicio (QoS) de las aplicaciones de AV en la red. Proporciona mecanismos para control de admisión, supervisión y cumplimiento en tiempo de ejecución, recopilación de comentarios acerca de aplicaciones y establecimiento de la prioridad del tráfico.
|   **Nombre del servicio**    |   QWAVE
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Servicio de calidad de servicio del lado cliente
|||         



##      <a name="radio-management-service"></a>Servicio de administración de radio        

| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de administración de radio y modo avión
|   **Nombre del servicio**    |   RmSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="remote-access-auto-connection-manager"></a>Administrador de conexiones automáticas de acceso remoto            

| | |           
|---|---|   
|   **Descripción del servicio** |   Crea una conexión a una red remota cada vez que un programa hace referencia a un DNS remoto o a un nombre o una dirección NetBIOS.
|   **Nombre del servicio**    |   RasAuto
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="remote-access-connection-manager"></a>Administrador de conexiones de acceso remoto         

| | |           
|---|---|   
|   **Descripción del servicio** |   Administra conexiones de acceso telefónico y de red privada virtual (VPN) desde este equipo a Internet u otras redes remotas. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   RasMan
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="remote-desktop-configuration"></a>Configuración de Escritorio remoto         

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio Configuración de Escritorio remoto (RDCS) se encarga de todas las actividades de mantenimiento de sesiones y configuración relacionadas con Servicios de Escritorio remoto y Escritorio remoto que requieran el contexto SYSTEM. Entre ellas, se incluyen las carpetas temporales por sesión, los temas de Escritorio remoto y los certificados de Escritorio remoto.
|   **Nombre del servicio**    |   SessionEnv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   
|||         



## <a name="remote-desktop-services"></a>Servicios de Escritorio remoto          

| | |           
|---|---|   
|   **Descripción del servicio** |   Esta característica permite a los usuarios conectarse de manera interactiva a un equipo remoto. Escritorio remoto y el servidor host de sesión de Escritorio remoto dependen de este servicio.  Para impedir el uso remoto de este equipo, desactive las casillas de la pestaña Acceso remoto, en el elemento Propiedades del sistema del Panel de control.
|   **Nombre del servicio**    |   TermService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   
|||         



##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector de puerto en modo usuario de Servicios de Escritorio remoto        

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite la redirección de impresoras, unidades o puertos para conexiones RDP.
|   **Nombre del servicio**    |   UmRdpService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Admite los redireccionamientos en el lado servidor de la conexión.
|||         



## <a name="remote-procedure-call-rpc"></a>Llamada a procedimiento remoto (RPC)          

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio RPCSS es el Administrador de control de servicios para los servidores COM y DCOM. Realiza solicitudes de activación de objetos, resoluciones del exportador de objetos y recolección distribuida de elementos no usados para servidores COM y DCOM. Si este servicio se detiene o se deshabilita, los programas que usen COM o DCOM no funcionarán correctamente. Por ello, es muy recomendable que ejecute el servicio RPCSS.
|   **Nombre del servicio**    |   RpcSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="remote-procedure-call-rpc-locator"></a>Localizador de llamada a procedimiento remoto (RPC)             

| | |               
|---|---|   
|   **Descripción del servicio** |   En Windows 2003 y versiones anteriores de Windows, el servicio Localizador de llamada a procedimiento remoto (RPC) administra la base de datos del servicio de nombres RPC. En Windows Vista y versiones posteriores de Windows, este servicio no ofrece ninguna funcionalidad y existe para la compatibilidad de aplicaciones.   |
|   **Nombre del servicio**    |   RpcLocator  |
|   **Instalación**    |   Solo con Experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  | Sin instrucciones   |
|   **Comentarios**    |       |
|||             



## <a name="remote-registry"></a>Registro remoto          

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite que los usuarios remotos modifiquen la configuración del Registro en este equipo. Si se detiene este servicio, solo los usuarios de este equipo podrán modificar el Registro. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   RemoteRegistry
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   
|||         



##  <a name="resultant-set-of-policy-provider"></a>Conjunto resultante de proveedor de directivas            

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un servicio de red que procesa las solicitudes para simular la aplicación en varias situaciones de cierta configuración de directiva de grupo en un usuario o equipo de destino, y procesa la configuración del Conjunto resultante de directivas.
|   **Nombre del servicio**    |   RSoPProv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |Sin instrucciones    
|   **Comentarios**    |   
|||         



## <a name="routing-and-remote-access"></a>Enrutamiento y acceso remoto            

| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece servicios de enrutamiento a empresas en entornos de red de área local y extensa.
|   **Nombre del servicio**    |   RemoteAccess
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   Ya deshabilitado
|||         



## <a name="rpc-endpoint-mapper"></a>Asignador de extremos de RPC          

| | |           
|---|---|   
|   **Descripción del servicio** |   Resuelve los identificadores de las interfaces de RPC en los puntos de conexión de transporte. Si se detiene o deshabilita este servicio, los programas que usen los servicios de Llamada a procedimiento remoto (RPC) no funcionarán correctamente.
|   **Nombre del servicio**    |   RpcEptMapper
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="secondary-logon"></a>Inicio de sesión secundario     

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite iniciar procesos con credenciales alternativas. Si se detiene este servicio, este tipo de acceso de inicio de sesión no estará disponible. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   seclogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="secure-socket-tunneling-protocol-service"></a>Servicio de protocolo de túnel de sockets seguros            

| | |               
|---|---|       
|   **Descripción del servicio** |   Ofrece compatibilidad con el protocolo de túnel de sockets seguros (SSTP) para conectarse con equipos remotos usando VPN. Si se deshabilita este servicio, los usuarios no podrán usar SSTP para tener acceso a servidores remotos.    |
|   **Nombre del servicio**    |   SstpSvc |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   No deshabilitar  |
|   **Comentarios**    |   La deshabilitación interrumpe a RRAS.   |
|||             



## <a name="security-accounts-manager"></a>Administrador de cuentas de seguridad            

| | |           
|---|---|       
|   **Descripción del servicio** |   El inicio de este servicio indica a otros servicios que el Administrador de cuentas de seguridad (SAM) está listo para aceptar solicitudes.  Si deshabilita este servicio, impedirá que se notifique a otros servicios del sistema cuándo está listo SAM, lo que a su vez puede provocar un error de inicio de dichos servicios. No debes deshabilitar este servicio.
|   **Nombre del servicio**    |   SamSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No deshabilitar
|   **Comentarios**    |   
|||         



## <a name="sensor-data-service"></a>Servicio de datos del sensor  

| | |           
|---|---|   
|   **Descripción del servicio** |   Entrega datos de varios sensores.
|   **Nombre del servicio**    |   SensorDataService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="sensor-monitoring-service"></a>Servicio de supervisión de sensores            

| | |           
|---|---|       
|   **Descripción del servicio** |   Supervisa los diversos sensores para exponer los datos y adaptarse al sistema y el estado del usuario.  Si se detiene o se deshabilita, el brillo de la pantalla no se adaptará a las condiciones de iluminación. Al detenerlo, probablemente también se vean afectadas otras características y funcionalidades.
|   **Nombre del servicio**    |   SensrSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         

## <a name="sensor-service"></a>Servicio de sensores

| | |
|---|---|
|   **Descripción del servicio** |   Un servicio para sensores que administra la funcionalidad de distintos sensores. Administra la orientación de dispositivo simple (SDO) y el historial de los sensores. Carga el sensor SDO que notifica los cambios en la orientación del dispositivo.  Si se detiene o deshabilita este servicio, no se cargará el sensor SDO y no se producirá la rotación automática. También se detendrá la recopilación del historial de los sensores.
|   **Nombre del servicio**    |   SensorService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |
|||

## <a name="server"></a>Servidor           

| | |           
|---|---|   
|   **Descripción del servicio** |   Admite el uso compartido de canalizaciones con nombre, impresiones y archivos a través de la red para este equipo. Si se detiene este servicio, estas funciones no estarán disponibles. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   LanmanServer
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Es necesario para la administración remota, IPC$ y el uso compartido de archivos SMB.
|||         



## <a name="shell-hardware-detection"></a>Detección de hardware shell             

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona notificaciones sobre los eventos de hardware de Reproducción automática.
|   **Nombre del servicio**    |   ShellHWDetection
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="smart-card"></a>Tarjeta inteligente           

| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el acceso a las tarjetas inteligentes que lee este equipo. Si se detiene este servicio, este equipo no podrá leer las tarjetas inteligentes. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   SCardSvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



## <a name="smart-card-device-enumeration-service"></a>Servicio de enumeración de dispositivos de tarjeta inteligente                    

| | |               
|---|---|       
|   **Descripción del servicio** |   Crea nodos de dispositivos de software para todos los lectores de tarjetas inteligentes accesibles a una sesión determinada. Si se deshabilita este servicio, las API de WinRT no podrán enumerar los lectores de tarjetas inteligentes.   |
|   **Nombre del servicio**    |   ScDeviceEnum    |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Se puede deshabilitar   |
|   **Comentarios**    |   Es necesario casi exclusivamente para las aplicaciones de WinRT.    |
|||             



## <a name="smart-card-removal-policy"></a>Directiva de extracción de tarjetas inteligentes        

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite configurar el sistema para bloquear el escritorio del usuario al quitar la tarjeta inteligente.
|   **Nombre del servicio**    |   SCPolicySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="snmp-trap"></a>Captura de SNMP            

| | |           
|---|---|       
|   **Descripción del servicio** |   Recibe mensajes de captura generados por agentes locales o remotos del Servicio de Protocolo simple de administración de redes (SNMP) y retransmite los mensajes a programas de administración de SNMP que se ejecutan en este equipo. Si se detiene este servicio, los programas basados en SNMP en este equipo no recibirán mensajes de captura SNMP. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   SNMPTRAP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="software-protection"></a>Protección de software             

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la descarga, instalación y aplicación de las licencias digitales de Windows y aplicaciones de Windows. Si el servicio está deshabilitado, el sistema operativo y las aplicaciones con licencia se pueden ejecutar en modo de notificación. Es muy recomendable que no deshabilites el servicio de protección de software.
|   **Nombre del servicio**    |   sppsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="special-administration-console-helper"></a>Aplicación auxiliar especial de la consola de administración        

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite que los administradores obtengan acceso de manera remota al símbolo del sistema mediante los Servicios de administración de emergencia.
|   **Nombre del servicio**    |   sacsvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="spot-verifier"></a>Comprobador puntual            

| | |           
|---|---|   
|   **Descripción del servicio** |   Comprueba posibles daños en el sistema de archivos.
|   **Nombre del servicio**    |   svsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="ssdp-discovery"></a>Detección SSDP           

| | |           
|---|---|   
|   **Descripción del servicio** |   Detecta dispositivos y servicios en red que usan el protocolo de detección SSDP, como los dispositivos UPnP. También anuncia dispositivos y servicios SSDP que se ejecutan en el equipo local. Si se detiene este servicio, no se detectarán los dispositivos basados en SSDP. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   SSDPSRV
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="state-repository-service"></a>Servicio de repositorio de estado         

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona soporte a la infraestructura necesaria para el modelo de aplicación.
|   **Nombre del servicio**    |   StateRepository
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="still-image-acquisition-events"></a>Eventos de adquisición de imágenes estáticas

| | |           
|---|---|   
|   **Descripción del servicio** |   Inicia las aplicaciones asociadas con eventos de adquisición de imágenes estáticas.
|   **Nombre del servicio**    |   WiaRpc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="storage-service"></a>Servicio de almacenamiento          

| | |           
|---|---|       
|   **Descripción del servicio** |   Ofrece servicios de habilitación para la configuración del almacenamiento y la expansión del almacenamiento externo.
|   **Nombre del servicio**    |   StorSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="storage-tiers-management"></a>Administración de capas de almacenamiento        

| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza la colocación de los datos en capas de almacenamiento en todos los espacios de almacenamiento en capas del sistema.
|   **Nombre del servicio**    |   TieringEngineService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="superfetch"></a>SuperFetch          

| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene y mejora el rendimiento del sistema a lo largo del tiempo.
|   **Nombre del servicio**    |   SysMain
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="sync-host"></a>Host de sincronización            

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio sincroniza el correo, los contactos, el calendario y varios otros datos del usuario. El correo y otras aplicaciones que dependen de esta funcionalidad no funcionarán correctamente si este servicio no se está ejecutando.
|   **Nombre del servicio**    |   OneSyncSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         



## <a name="system-event-notification-service"></a>Servicio de notificación de eventos del sistema            

| | |           
|---|---|       
|   **Descripción del servicio** |   Supervisa los eventos del sistema y notifica de estos eventos a los suscripciones del sistema de eventos COM+.
|   **Nombre del servicio**    |   SENS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="system-events-broker"></a>Agente de eventos del sistema             

| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina la ejecución del trabajo en segundo plano para la aplicación WinRT. Si se deshabilita o se detiene este servicio, el trabajo en segundo plano podría no desencadenarse.
|   **Nombre del servicio**    |   SystemEventsBroker
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   A pesar de que su descripción implica que solamente es para las aplicaciones de WinRT, es necesario para el programador de tareas, el servicio de infraestructura del agente y otros componentes internos.
|||         



## <a name="task-scheduler"></a>Programador de tareas           

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite que un usuario configure y programe tareas automatizadas en este equipo. El servicio también hospeda varias tareas críticas del sistema de Windows. Si se detiene o deshabilita este servicio, estas tareas no se ejecutarán a la hora programada. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Programa
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="tcpip-netbios-helper"></a>Asistente NetBIOS sobre TCP/IP            

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad para el servicio NetBIOS sobre TCP/IP (NetBT) y resolución de nombres NetBIOS para los clientes de la red, lo que permite a los usuarios compartir archivos, imprimir e iniciar sesión en la red. Si se detiene este servicio, puede que estas funciones no estén disponibles. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   lmhosts
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="telephony"></a>Telefonía           

| | |           
|---|---|       
|   **Descripción del servicio** |   Ofrece compatibilidad con la API de telefonía (TAPI) para programas que controlan dispositivos de telefonía en el equipo local y, a través de la LAN, en servidores que también usan el servicio.
|   **Nombre del servicio**    |   TapiSrv
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   La deshabilitación interrumpe a RRAS.
|||         



## <a name="themes"></a>Temas           

| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona administración de temas de experiencia de usuario.
|   **Nombre del servicio**    |   Temas
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Si este servicio está deshabilitado, no se pueden establecer los temas de accesibilidad.
|||         



## <a name="tile-data-model-server"></a>Servidor del modelo de datos de mosaicos           

| | |           
|---|---|   
|   **Descripción del servicio** |   Servidor de mosaicos para las actualizaciones de los mosaicos.
|   **Nombre del servicio**    |   tiledatamodelsvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Si este servicio está deshabilitado, el menú Inicio se interrumpe.
|||         



##  <a name="time-broker"></a>Agente de eventos de tiempo     

| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina la ejecución del trabajo en segundo plano para la aplicación WinRT. Si se deshabilita o se detiene este servicio, el trabajo en segundo plano podría no desencadenarse.
|   **Nombre del servicio**    |   TimeBrokerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   A pesar de que su descripción implica que solamente es para las aplicaciones de WinRT, es necesario para el programador de tareas, el servicio de infraestructura del agente y otros componentes internos.
|||         



## <a name="touch-keyboard-and-handwriting-panel-service"></a>Servicio de teclado táctil y panel de escritura a mano         

| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita las funciones de lápiz y entradas de lápiz del teclado táctil y del panel de escritura a mano.
|   **Nombre del servicio**    |   TabletInputService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="update-orchestrator-service-for-windows-update"></a>Servicio de actualizaciones Orchestrator para Windows Update           

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las actualizaciones de Windows. Si se detiene, los dispositivos no podrán descargar ni instalar las actualizaciones más recientes.
|   **Nombre del servicio**    |   UsoSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   La descripción del servicio faltaba en la versión 1607; Windows Update (incluido WSUS) depende de este servicio.
|||         



## <a name="upnp-device-host"></a>Host de dispositivo UPnP         

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite hospedar los dispositivos UPnP en este equipo. Si se detiene este servicio, cualquier dispositivo UPnP hospedado dejará de funcionar y no se podrá agregar ningún dispositivo hospedado adicional. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   upnphost
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="user-access-logging-service"></a>Servicio de registro de acceso de usuarios          

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio registra solicitudes de acceso de cliente únicas, como direcciones IP y nombres de usuario, de productos y roles instalados en el servidor local. Los administradores que necesitan cuantificar la demanda de clientes de software de servidor de administración de licencia de acceso de cliente (CAL) sin conexión pueden consultar esta información por medio de PowerShell. Si el servicio se deshabilita, las solicitudes de los clientes no se registrarán y no podrán recuperarse por medio de consultas de PowerShell. La detención del servicio no afectará la consulta de datos históricos (consulte en la documentación de soporte técnico los pasos para eliminar datos históricos). El administrador del sistema local debe consultar los términos de licencia de Windows Server para determinar el número de CAL que el software de servidor necesita para tener la licencia apropiada; el uso del servicio y datos UAL no afecta a esta obligación.
|   **Nombre del servicio**    |   UALSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="user-data-access"></a>Acceso a datos de usuario        

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona a las aplicaciones acceso a datos de usuario estructurados, incluida la información de contacto, los calendarios, los mensajes y otro tipo de contenido. Si detienes o deshabilitas este servicio, puede que las aplicaciones que usen estos datos no funcionen correctamente.
|   **Nombre del servicio**    |   UserDataSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         



## <a name="user-data-storage"></a>Almacenamiento de datos de usuarios            

| | |           
|---|---|       
|   **Descripción del servicio** |   Controla el almacenamiento de datos de usuario estructurados, incluida la información de contacto, los calendarios, los mensajes y otro tipo de contenido. Si detienes o deshabilitas este servicio, puede que las aplicaciones que usen estos datos no funcionen correctamente.
|   **Nombre del servicio**    |   UnistoreSvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         



## <a name="user-experience-virtualization-service"></a>Servicio de virtualización de la experiencia de usuario           

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad para la itinerancia de configuración de las aplicaciones y del sistema operativo.
|   **Nombre del servicio**    |   UevAgentService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



##  <a name="user-manager"></a>Administrador de usuarios        

| | |           
|---|---|   
|   **Descripción del servicio** |   El administrador de usuarios proporciona los componentes de tiempo de ejecución necesarios para la interacción con múltiples usuarios.  Si se detiene este servicio, es posible que algunas aplicaciones no funcionen correctamente.
|   **Nombre del servicio**    |   UserManager
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="user-profile-service"></a>Servicio de perfiles de usuario         

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio es responsable de cargar y descargar los perfiles de usuario. Si se detiene o se deshabilita, los usuarios no podrán iniciar ni cerrar la sesión, las aplicaciones podrían experimentar problemas para obtener los datos de los usuarios y no recibirán notificaciones de eventos de perfil los componentes registrados para recibirlas.
|   **Nombre del servicio**    |   ProfSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="virtual-disk"></a>Disco virtual             

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de administración para discos, volúmenes, sistemas de archivos y matrices de almacenamiento.
|   **Nombre del servicio**    |   vds
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Sin instrucciones
|   **Comentarios**    |   
|||         



## <a name="volume-shadow-copy"></a>Instantánea de volumen           

| | |           
|---|---|   
|   **Descripción del servicio** |   Administra e implementa instantáneas de volumen usadas para copias de seguridad y otros propósitos. Si este servicio se detiene, las instantáneas se deshabilitarán para la copia de seguridad y esta podría generar un error. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   VSS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Sin instrucciones
|   **Comentarios**    |   
|||         



##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **Descripción del servicio** |   Hospeda objetos usados por los clientes de la cartera.
|   **Nombre del servicio**    |   WalletService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="windows-audio"></a>Audio de Windows            

| | |           
|---|---|       
|   **Descripción del servicio** |   Administra el audio de los programas basados en Windows.  Si se detiene este servicio, los efectos y los dispositivos de audio no funcionarán correctamente.  Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Audiosrv
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="windows-audio-endpoint-builder"></a>Compilador del punto de conexión de audio de Windows           

| | |           
|---|---|
|   **Descripción del servicio** |   Administra los dispositivos de audio del servicio de audio de Windows.  Si se detiene este servicio, los efectos y los dispositivos de audio no funcionarán correctamente.  Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   AudioEndpointBuilder
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="windows-biometric-service"></a>Servicio biométrico de Windows            

| | |           
|---|---|   
|   **Descripción del servicio** |   El Servicio biométrico de Windows les proporciona a las aplicaciones cliente la capacidad de capturar, comparar, manipular y almacenar datos biométricos sin acceder directamente a muestras biométricas ni a hardware biométrico. El servicio se hospeda en un proceso SVCHOST con privilegios.
|   **Nombre del servicio**    |   WbioSrvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="windows-camera-frame-server"></a>Servicio FrameServer de la Cámara de Windows         

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que varios clientes tengan acceso a los fotogramas de vídeo de las cámaras.
|   **Nombre del servicio**    |   FrameServer
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="windows-connection-manager"></a>Administrador de conexiones de Windows           

| | |           
|---|---|   
|   **Descripción del servicio** |   Toma decisiones de conexión/desconexión automáticas en función de las opciones de conectividad de red disponibles actualmente para el equipo y permite administrar la conectividad de red basándose en la configuración de una directiva de grupo.
|   **Nombre del servicio**    |   Wcmsvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-defender-network-inspection-service"></a>Servicio de inspección de red de Windows Defender          

| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a proteger contra intentos de intrusión dirigidos a vulnerabilidades conocidas o recientemente descubiertas en los protocolos de red.
|   **Nombre del servicio**    |   WdNisSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones    
|   **Comentarios**    |   
|||         



## <a name="windows-defender-service"></a>Servicio de Windows Defender         

| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a proteger a los usuarios contra malware y otro software potencialmente no deseado.
|   **Nombre del servicio**    |   WinDefend
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation: marco de controlador en modo usuario           

| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y administra los procesos de controlador en modo usuario. No es posible detener este servicio.
|   **Nombre del servicio**    |   wudfsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-encryption-provider-host-service"></a>Servicio host de proveedor de cifrado de Windows     

| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio host de proveedor de cifrado de Windows sirve como intermediario para proporcionar funcionalidades de cifrado de proveedores de cifrado de terceros a los procesos que necesitan evaluar y aplicar directivas EAS. Si lo detienes, se verán afectadas las comprobaciones de cumplimiento de EAS que han establecido las cuentas de correo conectadas.
|   **Nombre del servicio**    |   WEPHOSTSVC
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-error-reporting-service"></a>Servicio Informe de errores de Windows          

| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que se informe acerca de los errores cuando los programas dejan de funcionar o responder y permite que se entreguen las soluciones existentes. También permite generar registros para los servicios de diagnóstico y reparaciones. Si se detiene este servicio, es posible que el informe de errores no funcione correctamente y que no se muestren los resultados de los servicios de diagnóstico y reparaciones.
|   **Nombre del servicio**    |   WerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Recopila y envía los datos sobre bloqueo usados por los ISV o IHV de Microsoft o de terceros. Los datos se usan para diagnosticar errores que generan bloqueos, los que pueden incluir errores de seguridad. También es necesario para generar los informes de errores corporativos.
|||         



## <a name="windows-event-collector"></a>Recopilador de eventos de Windows          

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio administra suscripciones persistentes a eventos desde orígenes remotos que admiten el protocolo de administración remota de Windows (WS-Management). Esto incluye registros de eventos de Windows Vista, hardware y orígenes de eventos con la interfaz IPMI habilitada. El servicio almacena los eventos reenviados en un registro de eventos local. Si se detiene o deshabilita este servicio, no podrán crearse suscripciones de eventos y no podrán aceptarse los eventos reenviados.
|   **Nombre del servicio**    |   Wecsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Recopila eventos de ETW (incluidos eventos de seguridad) para la capacidad de administración y los diagnósticos.  Son muchas las características y las herramientas de terceros que se basan en el servicio, incluidas herramientas de auditoría de seguridad.
|||         



## <a name="windows-event-log"></a>Registro de sucesos de Windows            

| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio administra los eventos y los registros de eventos. Permite registrar los eventos, consultar los eventos, suscribirse a los eventos, archivar los registros de eventos y administrar los metadatos de eventos. Puede mostrar los eventos en formato XML y de texto sin formato. Si detienes este servicio, puedes poner en peligro la seguridad y confiabilidad del sistema.
|   **Nombre del servicio**    |   EventLog
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-firewall"></a>Firewall de Windows         

| | |           
|---|---|   
|   **Descripción del servicio** |   Firewall de Windows ayuda a proteger el equipo, ya que evita que usuarios no autorizados accedan a él a través de Internet o de una red.
|   **Nombre del servicio**    |   MpsSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="windows-font-cache-service"></a>Servicio de caché de fuentes de Windows      

| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza el rendimiento de las aplicaciones almacenando en caché los datos de las fuentes más usadas. Las aplicaciones iniciarán este servicio si no se está ejecutando. Es posible deshabilitarlo, aunque si se hace, el rendimiento de las aplicaciones disminuirá.
|   **Nombre del servicio**    |   FontCache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-image-acquisition-wia"></a>Adquisición de imágenes de Windows (WIA)          

| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece servicios de adquisición de imágenes para escáneres y cámaras.
|   **Nombre del servicio**    |   stisvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



##  <a name="windows-insider-service"></a>Servicio de Windows Insider     

| | |           
|---|---|   
|   **Descripción del servicio** |   wisvc
|   **Nombre del servicio**    |   wisvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   El servidor no admite la distribución de paquetes piloto, .por lo que no opera en el servidor. La característica también se puede deshabilitar a través de la directiva de grupo.
|||         



##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **Descripción del servicio** |   Agrega, modifica y quita las aplicaciones proporcionadas como un paquete de Windows Installer (*.msi, *.msp). Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   msiserver
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-license-manager-service"></a>Servicio de administrador de licencias de Windows          

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad de infraestructura con Microsoft Store.  Este servicio se inicia a petición y, si se deshabilita, el contenido adquirido a través de Microsoft Store no funcionará correctamente.
|   **Nombre del servicio**    |   LicenseManager
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-management-instrumentation"></a>Instrumental de administración de Windows       

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona una interfaz común y un modelo de objeto para tener acceso a la información de administración acerca del sistema operativo, los dispositivos, las aplicaciones y los servicios. Si se detiene este servicio, la mayoría del software basado en Windows no funcionará correctamente. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   Winmgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



##  <a name="windows-mobile-hotspot-service"></a>Servicio de zona con cobertura inalámbrica móvil de Windows          

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la capacidad de compartir una conexión de datos móviles con otro dispositivo.
|   **Nombre del servicio**    |   icssvc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   
|||         



## <a name="windows-modules-installer"></a>Instalador de módulos de Windows        

| | |           
|---|---|   
|   **Descripción del servicio** |   Permite la instalación, la modificación y la eliminación de componentes opcionales y de las actualizaciones de Windows. Si se deshabilita este servicio, la instalación o desinstalación de las actualizaciones de Windows podría presentar un error en este equipo.
|   **Nombre del servicio**    |   TrustedInstaller
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="windows-push-notifications-system-service"></a>Servicio del sistema de notificaciones de inserción de Windows            

| | |           
|---|---|
|   **Descripción del servicio** |   Este servicio se ejecuta en la sesión 0 y hospeda la plataforma de notificaciones y el proveedor de conexiones que controla la conexión entre el dispositivo y el servidor WNS.
|   **Nombre del servicio**    |   WpnService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Es necesario para los iconos dinámicos y otras características.
|||         



## <a name="windows-push-notifications-user-service"></a>Servicio de usuario de notificaciones de inserción de Windows          

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio hospeda la plataforma de notificaciones de Windows, que proporciona compatibilidad para las notificaciones locales y las de inserción. Las notificaciones admitidas son los iconos, las notificaciones del sistema y las sin procesar.
|   **Nombre del servicio**    |   WpnUserService
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Se puede deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         



## <a name="windows-remote-management-ws-management"></a>Administración remota de Windows (WS-Management)
| | |
|---|---|
|   **Descripción del servicio** |   El servicio de administración remota de Windows (WinRM) implementa el protocolo WS-Management para la administración remota. WS-Management es un protocolo estándar de servicios web para la administración remota de software y hardware. El servicio WinRM está a la escucha en la red de las solicitudes de WS-Management y las procesa. El servicio WinRM debe configurarse con un agente de escucha mediante la herramienta de línea de comandos winrm.cmd o mediante la directiva de grupo para que esté a la escucha en la red. El servicio WinRM proporciona acceso a datos WMI y permite la recopilación de eventos. La recopilación de eventos y la suscripción a eventos requieren que se ejecute el servicio. Los mensajes de WinRM usan HTTP y HTTPS como transportes. El servicio WinRM no depende de IIS, sino que está configurado previamente para compartir un puerto con IIS en el mismo equipo.  El servicio WinRM se reserva el prefijo de dirección URL /wsman. Para evitar conflictos con IIS, los administradores deben asegurarse de que ningún sitio web hospedado en IIS utilice el prefijo de dirección URL /wsman.
|   **Nombre del servicio**    |   WinRM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Necesario para la administración remota.
|||



##  <a name="windows-search"></a>Windows Search      

| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la indexación del contenido, el almacenamiento en caché de las propiedades y los resultados de la búsqueda de archivos, correo electrónico y otro contenido.
|   **Nombre del servicio**    |   WSearch
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya deshabilitado
|   **Comentarios**    |   
|||         



##  <a name="windows-time"></a>Hora de Windows        

| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene la sincronización de fecha y hora en todos los clientes y servidores de la red. Si este servicio está detenido, la sincronización de fecha y hora no estará disponible. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   W32Time
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones
|   **Comentarios**    |   
|||         



## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita la detección, descarga e instalación de las actualizaciones de Windows y de otros programas. Si el servicio está deshabilitado, los usuarios de este equipo no podrán usar Windows Update ni su característica de actualización automática y los programas no podrán usar la API del Agente de Windows Update (WUA).
|   **Nombre del servicio**    |   wuauserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servicio de detección automática del proxy web WinHTTP         

| | |           
|---|---|   
|   **Descripción del servicio** |   WinHTTP implementa la pila HTTP de cliente y proporciona a los desarrolladores una API Win32 y un componente de automatización COM para enviar solicitudes HTTP y recibir respuestas. Además, WinHTTP ofrece compatibilidad con la detección automática de una configuración proxy mediante la implementación del protocolo de detección automática de proxy web (WPAD).
|   **Nombre del servicio**    |   WinHttpAutoProxySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilitar
|   **Comentarios**    |   Todo lo que usa la pila de red puede tener una dependencia funcional de este servicio. Muchas de las organizaciones se basan en esto para configurar el enrutamiento de proxy HTTP de sus redes internas.  Sin el servicio, todas las conexiones HTTP a Internet originadas internamente presentarán un error.
|||         



## <a name="wired-autoconfig"></a>Servicio de configuración automática de redes cableadas         

| | |           
|---|---|       
|   **Descripción del servicio** |   El Servicio de configuración automática de redes cableadas (DOT3SVC) es responsable de realizar la autenticación IEEE 802.1X en las interfaces de Ethernet. Si la implementación de la red cableada actual exige la autenticación 802.1X, el servicio DOT3SVC debe configurarse de modo que se ejecute para establecer la conectividad de nivel 2 o proporcionar acceso a los recursos de red. Las redes cableadas que no exigen autenticación 802.1X no se verán afectadas por el servicio DOT3SVC.
|   **Nombre del servicio**    |   dot3svc
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones   
|   **Comentarios**    |   
|||         



## <a name="wmi-performance-adapter"></a>Adaptador de rendimiento de WMI          

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona información de la biblioteca de rendimiento de proveedores WMI (Instrumental de administración de Windows) a clientes en la red. Este servicio solo se ejecuta si la Aplicación auxiliar de datos de rendimiento está activado.
|   **Nombre del servicio**    |   wmiApSrv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Sin instrucciones       
|   **Comentarios**    |   
|||         



## <a name="workstation"></a>Estación de trabajo          

| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y mantiene conexiones de red de cliente para los servidores remotos mediante el protocolo SMB. Si se detiene este servicio, estas conexiones no estarán disponibles. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar.
|   **Nombre del servicio**    |   LanmanWorkstation
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Sin instrucciones       
|   **Comentarios**    |   
|||         



## <a name="xbox-live-auth-manager"></a>Administración de autenticación de Xbox Live           

| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de autenticación y de autorización para interactuar con Xbox Live. Si se detiene este servicio, es posible que algunas aplicaciones no funcionen correctamente.
|   **Nombre del servicio**    |   XblAuthManager
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitado
|   **Comentarios**    |   
|||         



## <a name="xbox-live-game-save"></a>Partida guardada en Xbox Live          

| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio sincroniza los datos guardados para los juegos que pueden guardarse en Xbox Live.  Si se detiene el servicio, la partida guardada no se cargará a Xbox Live ni se descargará desde ahí.
|   **Nombre del servicio**    |   XblGameSave
|   **Instalación**    |   Solo con Experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitado
|   **Comentarios**    |   Este servicio sincroniza los datos guardados para los juegos que pueden guardarse en Xbox Live.  Si se detiene el servicio, la partida guardada no se cargará a Xbox Live ni se descargará desde ahí.
|||         




