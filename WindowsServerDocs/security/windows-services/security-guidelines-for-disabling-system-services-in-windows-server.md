---
title: Directrices de seguridad para los servicios del sistema en Windows Server 2016
description: Directrices de seguridad para deshabilitar los servicios en Windows Server 2016 con la experiencia de escritorio
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 5/22/2017
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 8f60d5095a3e4cebdffba5d3c02c69bc06326b3a
ms.sourcegitcommit: 68952ac7a29d0e2f07023958ad949fecc1b783b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2018
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Instrucciones sobre cómo deshabilitar servicios del sistema en Windows Server 2016 con la experiencia de escritorio

Se aplica a: Windows Server 2016

El sistema operativo Windows incluye muchos servicios del sistema que proporcionan funcionalidad importante. Diferentes servicios tienen directivas de inicio predeterminada diferente: algunos se inician de forma predeterminada (automática), algunas cuando sea necesario (manuales), y algunos están deshabilitadas de manera predeterminada y deben estar habilitados de forma explícita antes de que se pueden ejecutar. Estos valores predeterminados se eligieron con cuidado para que cada servicio Equilibrio entre rendimiento, la funcionalidad y la seguridad para los clientes típicos.

Sin embargo, algunos clientes de empresa pueden prefieren un equilibrio más centrados en la seguridad para sus equipos con Windows y los servidores, lo que reduce su superficie de ataque para el mínimo y, por lo tanto, puede desactivar completamente todos los servicios que no son necesarios en sus entornos específicos. Para los clientes, Microsoft® ofrece la guía correspondiente referentes a qué servicios de forma segura se puede deshabilitar para este propósito.

La guía es para Windows Server 2016 con la experiencia de escritorio (a menos que se usa como una sustitución de escritorio para los usuarios finales). Cada servicio en el sistema se clasifica como sigue:

-   **Debes deshabilitar:** deshabilitar este servicio y renunciar a su funcionalidad es muy probable que prefieren una empresa centrados en la seguridad (ver detalles adicionales a continuación).
- **Aceptar para deshabilitar:** este servicio proporciona funciones que son útiles para algunas pero no todas las empresas y las empresas centrados en la seguridad que no lo uses lo pueden deshabilitar con seguridad.
- **No deshabilite:** deshabilitar este servicio se afectar funcionalidad fundamental o impedir que determinadas funciones o características funcionan correctamente. Por lo tanto no debe deshabilitarse.
-  **(Ninguna directriz):** el impacto de la deshabilitación de estos servicios no se han evaluado completamente. Por lo tanto, no se debe cambiar la configuración predeterminada de estos servicios.


Los clientes pueden configurar sus equipos con Windows y los servidores para deshabilitar seleccionado los servicios con las plantillas de seguridad en las directivas de grupo o mediante la automatización de PowerShell. En algunos casos, la guía incluye la configuración de directiva de grupo específica que deshabilita la funcionalidad del servicio directamente, como una alternativa para deshabilitar el propio servicio.

Microsoft recomienda que los clientes deshabiliten los siguientes servicios y sus respectivas tareas programadas en Windows Server 2016 con la experiencia de escritorio:

Servicios: 
1. Administrador de Xbox Live de autenticación
2. Guardar de juego de Xbox Live

Tareas programadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(También puedes acceder a la información de todos los servicios que se detalla en este artículo mediante la visualización de la hoja de cálculo de Microsoft Excel adjunta: [instrucciones deshabilitar servicios del sistema en Windows Server 2016 con la experiencia de escritorio](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>Si se deshabilitan servicios que no se instala de forma predeterminada

Microsoft recomienda contra aplicar directivas para deshabilitar los servicios que no se instalan de forma predeterminada.
-  El servicio suele ser necesario si la característica está instalada. Instalar el servicio o la característica requiere derechos administrativos. Impedir la instalación de la característica, no el inicio del servicio.
-  Bloqueando el servicio de Microsoft Windows no impide que un administrador (o no administrador en algunos casos) se instale un equivalente de terceros similares, por ejemplo, uno con un mayor riesgo de seguridad.
-  Una línea base o referencia que deshabilita un servicio de Windows no predeterminado (por ejemplo, W3SVC) proporcionará a algunos los auditores la impresión errónea de que la tecnología (por ejemplo, IIS) es inherentemente no segura y nunca debe usarse.
-  Si nunca se instala la característica (y el servicio), simplemente Esto agrega masiva innecesario a la línea base y el trabajo de verificación.

<br />
Para todos los servicios del sistema enumerados en este documento, las dos tablas siguientes ofrecen una explicación de columnas y las recomendaciones de Microsoft para habilitar y deshabilitar servicios del sistema en Windows Server 2016 con la experiencia de escritorio: 
 
<br />

### <a name="explanation-of-columns"></a>Explicación de columnas

| | |
|---|---|
|**Descripción del servicio**|   La descripción del servicio, desde sc.exe qdescription.|
|**Nombre** |Nombre de la clave (interno) del servicio|
|**Instalación** |Siempre instalado: servicio está en Server Core y el servidor con la experiencia de escritorio  <br /> Solo en Datacenter Edition: servicio de 2016 de servidor con la experiencia de escritorio, pero es ***no*** en Server Core |
|**StartType**  |Tipo de inicio de servicio en Windows Server 2016|
|**Recomendación** |Recomendación de Microsoft o consejos acerca de cómo deshabilitar este servicio en Windows Server 2016 en una implementación típica, bien administrados empresarial y donde el servidor no se utiliza como una sustitución de escritorio del usuario final.|
|**Comentarios** |Explicación adicional|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explicación de las recomendaciones de Microsoft

| | |
|---|---|
|**No deshabilite** |Este servicio no debe deshabilitarse|
|**Aceptar para deshabilitar**| Este servicio se puede deshabilitar si no se usa la característica que admite.|
|**Ya se ha deshabilitado**|  Este servicio está deshabilitado de forma predeterminada; no es necesario aplicar con la directiva|
|**Debe estar deshabilitado** |Este servicio nunca debe habilitarse en un sistema de la empresa bien administrados.|

<br />

Las tablas siguientes ofrecen instrucciones de Microsoft acerca de cómo deshabilitar servicios del sistema en Windows Server 2016 con la experiencia de escritorio:

<br />

##  <a name="activex-installer-axinstsv"></a>Instalador de ActiveX (AxInstSV)

| | |
|---|---|
|   **Descripción del servicio** |   Proporciona la validación de Control de cuentas de usuario para la instalación de los controles ActiveX de Internet y permite la administración de control de instalación de ActiveX en función de la configuración de directiva de grupo. Este servicio se inicia a petición y si se deshabilita la instalación de los controles ActiveX se comportará según la configuración predeterminada del explorador.    |
|   **Nombre de servicio**    |   AxInstSV    |
|   **Instalación**    |   Solo en Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Aceptar para deshabilitar   |
|   **Comentarios**    |   Aceptar deshabilitar si la característica no es necesario |


<br />

## <a name="alljoyn-router-service"></a>Servicio de enrutador AllJoyn   
| | |
|---|---|
|   **Descripción del servicio** |   Mensajes de AllJoyn de rutas para los clientes de AllJoyn locales. Si este servicio se detiene, los clientes de AllJoyn que no tienen sus propias enrutadores agrupadas será no se puede ejecutar. |
|   **Nombre de servicio**    |   AJRouter    |
|   **Instalación**    |   Solo en Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendación**  | Ninguna directriz       |
|   **Comentarios**    |       |
| | |

<br />

## <a name="app-readiness"></a>Preparación de la aplicación
| | |
|---|---|
**Descripción del servicio** |   Obtiene la aplicaciones de la lista para usar la primera vez que un usuario inicia sesión el equipo y al agregar nuevas aplicaciones.
**Nombre de servicio**    |   AppReadiness
**Instalación**    |   Solo en Datacenter Edition
**StartType**   |   Manual
**Recomendación**  |   No deshabilite
**Comentarios**    |   
| | |

<br />

##  <a name="application-identity"></a>Identidad de aplicación
| | |       
|---|---|   
**Descripción del servicio** |   Determina y comprueba la identidad de una aplicación. Deshabilitación de este servicio impedirá que AppLocker aplicarlas.
**Nombre de servicio**    |   AppIDSvc
**Instalación**    |   Siempre instalado
**StartType**   |   Manual
**Recomendación**  |Ninguna directriz    
**Comentarios**    |   
|||     

<br />

##  <a name="application-information"></a>Información de la aplicación 
| | |       
|---|---|   
|   **Descripción del servicio** |   Facilita la ejecución de aplicaciones interactivas con privilegios administrativos adicionales.  Si este servicio se detiene, los usuarios podrán iniciar aplicaciones con los privilegios administrativos adicionales que puedan necesitar para realizar tareas de usuario deseada.
|   **Nombre de servicio**    |   Appinfo
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   Admite la elevación de UAC mismo escritorio
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Servicio de puerta de enlace de nivel de aplicación       
| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona compatibilidad para complementos de terceros protocolo de conexión compartida a Internet
|   **Nombre de servicio**    |   ALGORITMO
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |Ninguna directriz    
|   **Comentarios**    |   
|||     

<br />

##  <a name="application-management"></a>Administración de aplicaciones      
| | |           
|---|---|       
|   **Descripción del servicio** |   Procesa las solicitudes de enumeración, instalación y eliminación de software implementado mediante la directiva de grupo. Si el servicio está deshabilitado, los usuarios podrán instalar, quitar o enumerar el software implementado a través de la directiva de grupo. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   AppMgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Servicios de implementación de AppX (AppXSVC)       
| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura para implementar aplicaciones de la tienda. Este servicio se inicia a petición y, si no se implementarán en el sistema deshabilitadas aplicaciones de la tienda y no pueden funcionan correctamente.
|   **Nombre de servicio**    |   AppXSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Actualizador de zona horaria automática           
| | |           
|---|---|           
|   **Descripción del servicio** |   La zona horaria se establece automáticamente.
|   **Nombre de servicio**    |   tzautoupdate
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Servicio de transferencia inteligente en segundo plano          
| | |           
|---|---|   
|   **Descripción del servicio** |   Transfiere archivos en segundo plano mediante el ancho de banda de red inactivo. Si se deshabilita el servicio, las aplicaciones que dependen de BITS, como Windows Update o el Explorador de MSN, podrá descargar automáticamente los programas y otra información.
|   **Nombre de servicio**    |   BITS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Servicio de infraestructura de tareas en segundo plano      
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de infraestructura de Windows que controla qué tareas en segundo plano puede ejecutarse en el sistema.
|   **Nombre de servicio**    |   BrokerInfrastructure
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Motor de filtro de base            
| | |           
|---|---|       
|   **Descripción del servicio** |   El motor de filtrado de Base (BFE) es un servicio que administra la seguridad y las directivas de seguridad (IPsec) del protocolo de Internet e implementa el filtrado de modo de usuario. Al detener o deshabilitar el servicio BFE reducirá considerablemente la seguridad del sistema. También producirá un comportamiento impredecible en aplicaciones de firewall y administración de IPsec.
|   **Nombre de servicio**    |   BFE
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Servicio de soporte técnico de Bluetooth            
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de Bluetooth admite el descubrimiento y la asociación de dispositivos Bluetooth remotos.  Al detener o deshabilitar este servicio puede provocar que todos los dispositivos Bluetooth ya instalados no funcionar correctamente e impedir nuevos dispositivos detectados o asociados.
|   **Nombre de servicio**    |   bthserv
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Aceptar para deshabilitar si no usa. Otro mecanismo deshabilitar:https://technet.microsoft.com/en-us/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de este usuario se usa para escenarios de la plataforma de dispositivos conectados
|   **Nombre de servicio**    |   CDPUserSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagación de certificados     
| | |           
|---|---|
|   **Descripción del servicio** |   Copia los certificados de usuario y los certificados raíz de tarjetas inteligentes en el almacén de certificados del usuario actual, detecta cuando se inserta una tarjeta inteligente en un lector de tarjetas inteligentes y si es necesario, instala al minicontrolador Plug and Play de tarjeta inteligente.
|   **Nombre de servicio**    |   CertPropSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Servicio de licencias de cliente (ClipSVC)        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura de Microsoft Store. Este servicio se inicia a petición y si comprado aplicaciones deshabilitadas usando Microsoft Store no se comportará correctamente.
|   **Nombre de servicio**    |   ClipSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Aislamiento de claves CNG
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de aislamiento de claves CNG se hospeda en el proceso de LSA. El servicio proporciona el aislamiento de procesos clave para las claves privadas y las operaciones criptográficas asociadas según sea necesario por los criterios comunes. El servicio almacena y usa las teclas de larga duración en un proceso seguro que cumpla los requisitos de criterios comunes.
|   **Nombre de servicio**    |   KeyIso
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema de eventos COM +       
| | |           
|---|---|       
|   **Descripción del servicio** |   Admite el sistema evento Notification Service (tenor), que proporciona la distribución automática de eventos a los componentes de modelo de objetos componentes (COM). Si se detiene el servicio, tenor se cerrará y no podrán proporcionar notificaciones y cierre de sesión. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   EventSystem
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Aplicación del sistema COM +     
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la configuración y el seguimiento de los componentes de modelo de objetos componentes (COM) y basados en. Si se detiene el servicio, la mayoría COM +-basado en componentes no funcionará correctamente. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   COMSysApp
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Explorador de equipo        
| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene una lista actualizada de equipos de la red y proporciona esta lista para equipos designados como exploradores. Si se detiene el servicio, esta lista no se actualiza o se mantiene. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   Explorador
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Servicio de la plataforma de dispositivos conectados       
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio se usa para escenarios de dispositivos conectados y vidrio Universal
|   **Nombre de servicio**    |   CDPSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Telemetría y experiencia del usuario conectado     
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de telemetría y experiencias del usuario conectado permite características que admiten la experiencia del usuario en la aplicación y conectado. Además, este servicio administra la recopilación de controlados por eventos y la transmisión de información de diagnóstico y uso (que se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando se habilitan los diagnósticos y opciones de privacidad de uso en los comentarios y diagnósticos.
|   **Nombre de servicio**    |   DiagTrack
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Datos de contacto        
| | |           
|---|---|       
|   **Descripción del servicio** |   Datos de contacto de índices para la búsqueda rápida de contacto. Si se detiene o deshabilitar este servicio, contactos puede faltar en los resultados de búsqueda.
|   **Nombre de servicio**    |   PimIndexMaintenanceSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Descripción del servicio** |   Administra la comunicación entre componentes del sistema.
|   **Nombre de servicio**    |   CoreMessagingRegistrar
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Administrador de credenciales           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona almacenamiento seguro y la recuperación de credenciales a los usuarios, aplicaciones y los paquetes de servicio de seguridad.
|   **Nombre de servicio**    |   VaultSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Servicios de cifrado           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona tres servicios de administración: servicio de base de datos de catálogo, que confirma las firmas de archivos de Windows y permite que nuevos programas para instalarse; Servicio de raíz, que agrega y quita los certificados de entidad de certificación raíz de confianza de este equipo; protegida y el servicio de actualización automática de certificados de raíz, que recupera los certificados raíz desde Windows Update y permiten escenarios como SSL. Si se detiene el servicio, es posible que estos servicios de administración no funcionará correctamente. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   CryptSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Servicio de uso compartido de datos         
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona el intercambio de datos entre aplicaciones.
|   **Nombre de servicio**    |   DsSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCP (recopilación de datos y publicación) admite aplicaciones propias para cargar datos en la nube.
|   **Nombre de servicio**    |   DcpSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Iniciador de procesos DCOM         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCOMLAUNCH inicia servidores COM y DCOM en respuesta a las solicitudes de activación de objetos. Si este servicio se detiene o se deshabilita, los programas que usan DCOM o COM no funcionará correctamente. Se recomienda encarecidamente que tienes la ejecución del servicio DCOMLAUNCH.
|   **Nombre de servicio**    |   DcomLaunch
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |Ninguna directriz    
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Servicio de asociación de dispositivos      
| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita el emparejamiento entre el sistema y los dispositivos con cables o inalámbricos.
|   **Nombre de servicio**    |   DeviceAssociationService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Servicio de instalación de dispositivos      
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite a un equipo a reconocer y adaptarse a los cambios de hardware con poca o ninguna entrada del usuario. Al detener o deshabilitar este servicio se producirá inestabilidad del sistema.
|   **Nombre de servicio**    |   DeviceInstall
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Servicio de inscripción de administración de dispositivos        
| | |           
|---|---|       
|   **Descripción del servicio** |   Realiza las actividades de inscripción de dispositivos de administración de dispositivos
|   **Nombre de servicio**    |   DmEnrollmentSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Administrador de configuración de dispositivos         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la detección, descarga e instalación de software relacionado con el dispositivo. Si este servicio está deshabilitado, dispositivos pueden configurarse con software obsoleto y no funcionen correctamente.
|   **Nombre de servicio**    |   DsmSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Agente del detección DevQuery en segundo plano         
| | |           
|---|---|           
|   **Descripción del servicio** |   Permite que las aplicaciones detectar dispositivos con una tarea de fondo
|   **Nombre de servicio**    |   DevQueryBroker
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Cliente DHCP          
| | |           
|---|---|       
|   **Descripción del servicio** |   Registra y actualiza los registros DNS para este equipo y direcciones IP. Si se detiene el servicio, este equipo no recibirá actualizaciones DNS y direcciones IP dinámicas. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   DHCP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Servicio de diagnóstico de directiva            
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de diagnóstico directiva permite la detección de problemas, solución de problemas y componentes de Windows.  Si se detiene el servicio, diagnósticos dejará de funcionar.
|   **Nombre de servicio**    |   DP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host del servicio de diagnóstico     
| | |           
|---|---|       
|   **Descripción del servicio** |   El Host del servicio de diagnóstico se usa el servicio de directiva de diagnóstico para diagnóstico de host que necesita ejecutarse en un contexto de servicio Local.  Si se detiene el servicio, ya no funcionará cualquier diagnóstico que dependan de ella.
|   **Nombre de servicio**    |   WdiServiceHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host de sistema de diagnóstico           
| | |           
|---|---|       
|   **Descripción del servicio** |   El Host de sistema de diagnóstico se usa el servicio de directiva de diagnóstico para diagnóstico de host que necesite ejecutarse en un contexto de sistema Local.  Si se detiene el servicio, ya no funcionará cualquier diagnóstico que dependan de ella.
|   **Nombre de servicio**    |   WdiSystemHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Cliente de seguimiento de vínculos distribuidos            
| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene vínculos entre archivos NTFS dentro de un equipo o entre equipos en una red.
|   **Nombre de servicio**    |   TrkWks
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Coordinador de transacciones distribuidas     
| | |           
|---|---|   
|   **Descripción del servicio** |   Coordina las transacciones que ocupan varios administradores de recursos, como bases de datos, colas de mensajes y sistemas de archivos. Si se detiene el servicio, se producirá un error en estas operaciones. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   MSDTC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de enrutamiento de WAP inserción mensajes
|   **Nombre de servicio**    |   dmwappushservice
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servicio necesitado en dispositivos de cliente para Intune, MDM y tecnologías similares de administración y Unified Write Filter. No es necesario para el servidor.
|||         
            
<br />      

##  <a name="dns-client"></a>Cliente DNS      
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio cliente DNS (dnscache) se almacena en caché el sistema de nombres de dominio (DNS) y registra el nombre de equipo completo para este equipo. Si se detiene el servicio, nombres DNS seguirá resolverse. Sin embargo, no se almacenarán en caché los resultados de las consultas de nombre DNS y no se registrarán el nombre del equipo. Si se deshabilita el servicio, se producirá un error en los servicios que dependen explícitamente a inicio.
|   **Nombre de servicio**    |   DnsCache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Administrador de mapas descargados     
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de Windows para la aplicación acceso a mapas descargados. Este servicio se inicia a petición por aplicación acceder a mapas descargados. Deshabilitar este servicio impedirá que las aplicaciones acceder a mapas.
|   **Nombre de servicio**    |   MapsBroker
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Deshabilitar aplicaciones saltos que se basan en el servicio; Aceptar deshabilitar si no dependen de aplicaciones
|||         
            
<br />          

## <a name="embedded-mode"></a>Modo incrustado            
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de modo incrustado permite escenarios relacionados con las aplicaciones en segundo plano.  Deshabilitación de este servicio impedirá en segundo plano aplicaciones que se está activando.
|   **Nombre de servicio**    |   embeddedmode
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Cifrado de sistema de archivos (EFS)
| | |                   
|---|---|           
|   **Descripción del servicio** | Proporciona la tecnología de cifrado de archivos principal que se usa para almacenar archivos cifrados en volúmenes del sistema de archivos NTFS. Si este servicio se detiene o se deshabilita, las aplicaciones podrán acceder a archivos cifrados.            
|   **Nombre de servicio**  |  EFS            
|   **Instalación**  |  Siempre instalado           
|   **StartType**   |  Manual           
|   **Recomendación**  | Ninguna directriz           
|   **Comentarios**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Servicio de administración de aplicaciones de empresa            
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la administración de aplicaciones de empresa.
|   **Nombre de servicio**    |   EntAppSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Protocolo de autenticación extensible           
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de protocolo de autenticación Extensible (EAP) proporciona la autenticación de red en escenarios como 802.1x inalámbricas y con cable, VPN y protección de acceso de red (NAP).  EAP también proporciona interfaces de programación de aplicaciones (API) que se usan los clientes de acceso de red, incluidos los adaptadores inalámbricos y los clientes VPN, durante el proceso de autenticación.  Si deshabilitas este servicio, este equipo, no podrá obtener acceso a redes que requieren una autenticación EAP.
|   **Nombre de servicio**    |   EapHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host del proveedor de detección de función         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio FDPHOST hospeda los proveedores de detección de red de detección de función (FD). Estos proveedores FD proporcionan servicios de detección de red para el protocolo Simple de detección de servicios (SSDP) y servicios Web: protocolo de detección (WS-D). Al detener o deshabilitar el servicio FDPHOST deshabilitará la detección de redes para estos protocolos al usar FD. Cuando este servicio está disponible, los servicios de red usando FD y depender de estos protocolos de detección podrá encontrar los dispositivos de red o recursos.
|   **Nombre de servicio**    |   fdPHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Publicación de recurso de detección de función      
| | |           
|---|---|       
|   **Descripción del servicio** |   Publica este equipo y los recursos que se adjunta a este equipo para que puedan detectarse a través de la red.  Si se detiene el servicio, ya no se publicarán los recursos de red y no se detectarán por otros equipos de la red.
|   **Nombre de servicio**    |   FDResPub
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Servicio de ubicación geográfica          
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio controla la ubicación actual del sistema y administra perímetros (es decir, una ubicación geográfica con los eventos asociados).  Si desactiva este servicio, aplicaciones podrán usar o recibir notificaciones para la ubicación geográfica o de perímetros.
|   **Nombre de servicio**    |   lfsvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Deshabilitar aplicaciones saltos que se basan en el servicio; Aceptar deshabilitar si no dependen de aplicaciones
|||         
            
<br />          

##  <a name="group-policy-client"></a>Cliente de directiva de grupo     
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio es responsable de aplicar la configuración de los administradores del equipo y los usuarios a través del componente de directiva de grupo. Si se deshabilita el servicio, no se aplicará la configuración y aplicaciones y componentes no se podrán administrables mediante la directiva de grupo. Los componentes o las aplicaciones que dependen del componente de directiva de grupo pueden no ser funcionales si el servicio está deshabilitado.
|   **Nombre de servicio**    |   gpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Servicio de dispositivos de interfaz humana           
| | |           
|---|---|       
|   **Descripción del servicio** |   Activa y mantiene el uso de botones de acceso directo en teclados, controles remotos y otros dispositivos multimedia. Se recomienda que tengas que este servicio se ejecuta.
|   **Nombre de servicio**    |   Hidserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Servicio de Host HV     
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para el hipervisor Hyper-V proporcionar los contadores de rendimiento de cada partición que el sistema operativo host.
|   **Nombre de servicio**    |   HvHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Potenciadores para máquinas virtuales de invitado. No se utilizan hoy excepto explícitamente rellena las máquinas virtuales, pero se usará en protección de la aplicación
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Servicio de Exchange de datos de Hyper-V        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona un mecanismo para intercambiar datos entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre de servicio**    |   vmickvpexchange
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interfaz de servicio de invitado de Hyper-V          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para el host de Hyper-V interactuar con servicios específicos que se ejecutan dentro de la máquina virtual.
|   **Nombre de servicio**    |   vmicguestinterface
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Servicio de apagado de invitado de Hyper-V           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un mecanismo para apagar el sistema operativo de esta máquina virtual de las interfaces de administración en el equipo físico.
|   **Nombre de servicio**    |   vmicshutdown
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Servicio de latido Hyper-V            
| | |           
|---|---|           
|   **Descripción del servicio** |   Supervisa el estado de esta máquina virtual denunciando una pulsación a intervalos regulares. Este servicio ayuda a identificar las máquinas virtuales que han dejado de responder.
|   **Nombre de servicio**    |   vmicheartbeat
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Servicio de Hyper-V PowerShell directa            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un mecanismo para administrar la máquina virtual con PowerShell a través de la sesión de la máquina virtual sin una red virtual.
|   **Nombre de servicio**    |   vmicvmsession
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servicio de virtualización de escritorio remoto de Hyper-V            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona una plataforma para la comunicación entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre de servicio**    |   vmicrdv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Servicio de sincronización de hora de Hyper-V         
| | |           
|---|---|       
|   **Descripción del servicio** |   Sincroniza la hora del sistema de esta máquina virtual con la hora del sistema del equipo físico.
|   **Nombre de servicio**    |   vmictimesync
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de copia de instantáneas de volumen de Hyper-V         
| | |           
|---|---|           
|   **Descripción del servicio** |   Coordenadas de las comunicaciones que son necesarias para usar el servicio de instantáneas de volumen para hacer una copia de datos y las aplicaciones en esta máquina virtual desde el sistema operativo en el equipo físico.
|   **Nombre de servicio**    |   vmicvss
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulta HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE e AuthIP IPsec módulos de claves          
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio IKEEXT hospeda el intercambio de claves (IKE) y protocolo de Internet autenticado (AuthIP) módulos de claves. Estos módulos de creación de claves se usan para la autenticación y el intercambio de claves de seguridad de protocolo de Internet (IPsec). Al detener o deshabilitar el servicio IKEEXT deshabilitará el intercambio de claves IKE y AuthIP con equipos del mismo nivel. IPsec normalmente está configurada para usar IKE o AuthIP; por lo tanto, detener o deshabilitar el servicio IKEEXT puede provocar errores de IPsec y podría poner en peligro la seguridad del sistema. Se recomienda encarecidamente que tienes que ejecutar el servicio IKEEXT.
|   **Nombre de servicio**    |   IKEEXT
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Detección de servicios interactivos           
| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita una notificación de usuario de entrada para los servicios interactivos, lo que permite el acceso a creado por servicios interactivos cuando aparecen los cuadros de diálogo del usuario. Si se detiene el servicio, las notificaciones de los cuadros de diálogo nuevo los servicios interactivos dejarán de funcionar y es posible que no haya acceso a los cuadros de diálogo de los servicios interactivos. Si se deshabilita este servicio, las notificaciones de y acceso a los cuadros de diálogo nuevo los servicios interactivos dejarán de funcionar.
|   **Nombre de servicio**    |   UI0Detect
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Conexión a Internet de uso compartido (ICS)            
| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona la traducción de direcciones de red, direcciones, servicios de prevención de nombre de la resolución o intrusiones para una red doméstica o de oficina pequeña.
|   **Nombre de servicio**    |   SharedAccess
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Necesario para que los clientes que se utiliza como zonas Wi-Fi y también en ambos extremos de proyección de Miracast. ICS puede bloquearse con la configuración de GPO, "Prohibir el uso de conexión compartida a Internet en su red de dominio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Aplicación auxiliar IP            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona conectividad de túnel con IPv6 tecnologías de transición (6to4, ISATAP, puerto Proxy y Teredo) y IP HTTPS. Si se detiene el servicio, el equipo no tendrá los beneficios de conectividad mejorada que ofrecen estas tecnologías.
|   **Nombre de servicio**    |   iphlpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente de directiva IPsec      
| | |           
|---|---|       
|   **Descripción del servicio** |   Protocolo de Internet (IPsec) de seguridad admite la autenticación de nivel de red del mismo nivel, autenticación de origen de datos, integridad de los datos, confidencialidad (cifrado) de datos y protección de reproducción.  Este servicio aplica las directivas IPsec creadas mediante el complemento de directivas de seguridad IP o la herramienta de línea de comandos "netsh ipsec".  Si deja este servicio, puede experimentar problemas de conectividad de red si la directiva requiere que las conexiones usan IPsec.  Además, la administración remota de Firewall de Windows no está disponible cuando se detiene el servicio.
|   **Nombre de servicio**    |   PolicyAgent
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Servicio del servidor Proxy de KDC (KPS)      
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio del servidor Proxy de KDC ejecuta en borde servidores proxy Kerberos mensajes de protocolo a controladores de dominio en la red corporativa.
|   **Nombre de servicio**    |   KPSSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm para Coordinador de transacciones distribuidas            
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordenadas de las transacciones entre el Coordinador de transacciones distribuidas (MSDTC) y el Administrador de transacciones de Kernel (KTM). Si no es necesario, se recomienda que mantienen de este servicio se ha detenido. Si es necesario, MSDTC y KTM iniciarán este servicio automáticamente. Si este servicio está deshabilitado, se producirá un error en cualquier transacción MSDTC interactuar con un administrador de recursos de Kernel y los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   KtmRm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Asignador de detección de topología de nivel de vínculo        
| | |       
|---|---|       
|   **Descripción del servicio** |   Crea un mapa de la red, que consta de equipo e información de la topología (conectividad) de dispositivo y metadatos que describen cada equipo y el dispositivo.  Si este servicio está deshabilitado, el mapa de red no funcionará correctamente.
|   **Nombre de servicio**    |   lltdsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Aceptar deshabilitar si no tengan dependencias en el mapa de la red
|||         
            
<br />

## <a name="local-session-manager"></a>Administrador de sesión local                    
| | |                   
|---|---|   
|   **Descripción del servicio** |   Servicios principales de Windows que administra sesiones de usuario local. Al detener o deshabilitar este servicio se producirá inestabilidad del sistema.    
|   **Nombre de servicio**    |   LSM |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Automático   |
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Concentrador de diagnósticos Microsoft (R) recopilador estándar         
| | |           
|---|---|           
|   **Descripción del servicio** |   Servicios principales de Windows que administra sesiones de usuario local. Al detener o deshabilitar este servicio se producirá inestabilidad del sistema.
|   **Nombre de servicio**    |   LSM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Ayudante para el inicio de sesión de cuenta de Microsoft          
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite el inicio de sesión del usuario a través de servicios de identidad de cuenta de Microsoft. Si este servicio se detiene, los usuarios no podrán iniciar sesión en el equipo con su cuenta de Microsoft.
|   **Nombre de servicio**    |   wlidsvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Accounts de Microsoft están n / en Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Cliente de App-V de Microsoft      
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra los usuarios de App-V y aplicaciones virtuales
|   **Nombre de servicio**    |   AppVClient
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Servicio de iniciador iSCSI de Microsoft            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las sesiones de Internet SCSI (iSCSI) desde este equipo a dispositivos de destino iSCSI remoto. Si se detiene el servicio, este equipo no podrá iniciar sesión o tener acceso a destinos iSCSI. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   MSiSCSI
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Nuestros datos de diagnóstico indican que se usa en el cliente, así como servidor. Para deshabilitar esta ninguna ventaja.
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona el proceso de aislamiento de las claves criptográficas utilizadas para autenticar a los proveedores de identidad asociados del usuario. Si este servicio se deshabilita, todos los usos y la administración de estas claves no estarán disponibles, que incluye el inicio de sesión de equipo y el inicio de sesión único en las aplicaciones y sitios Web. Este servicio se inicia y se detiene automáticamente. Se recomienda que no cambias este servicio.
|   **Nombre de servicio**    |   NgcSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Necesario para los inicios de sesión PIN o Hello, que no se admiten en el servidor
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contenedor de Microsoft Passport         
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las claves de identidad de usuario local que usan para autenticar el usuario a proveedores de identidad, así como tarjetas inteligentes virtuales TPM. Si se deshabilita este servicio, las claves de identidad de usuario local y tarjetas inteligentes virtuales TPM no será accesibles. Se recomienda que no cambias este servicio.
|   **Nombre de servicio**    |   NgcCtnrSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Proveedor de instantáneas de Software de Microsoft          
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las instantáneas de volumen basado en software tomadas por el servicio de instantáneas de volumen. Si se detiene el servicio, no se puede administrar instantáneas de volumen basado en software. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   SWPRV
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>Espacios de almacenamiento de Microsoft SMP         
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de host para el proveedor de administración de espacios de almacenamiento de Microsoft. Si este servicio se detiene o se deshabilita, no se puede administrar espacios de almacenamiento.
|   **Nombre de servicio**    |   smphost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   API de administración de almacenamiento producirá un error sin este servicio. Ejemplo: "Get-WmiObject-clase MSFT_Disk - Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Servicio de uso compartido de puertos Net.Tcp         
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la capacidad de compartir los puertos TCP a través del protocolo net.tcp.
|   **Nombre de servicio**    |   NetTcpPortSharing
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Netlogon         
| | |           
|---|---|           
|   **Descripción del servicio** |   Mantiene un canal seguro entre el equipo y el controlador de dominio para autenticar usuarios y servicios. Si se detiene el servicio, el equipo no puede autenticar a los usuarios y servicios y el controlador de dominio no pueden registrar los registros de DNS. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   Netlogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Agente de conexión de red            
| | |           
|---|---|       
|   **Descripción del servicio** |   Conexiones de agentes que permiten que las aplicaciones de la tienda de Microsoft recibir notificaciones de internet.
|   **Nombre de servicio**    |   NcbService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Conexiones de red         
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra los objetos en la carpeta de conexiones de red y acceso telefónico, en el que puede ver la red de área local y las conexiones remotas.
|   **Nombre de servicio**    |   Netman
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Asistente de conectividad de red      
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona una notificación de estado de DirectAccess para los componentes de interfaz de usuario
|   **Nombre de servicio**    |   NcaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Servicio de la lista de red        
| | |           
|---|---|   
|   **Descripción del servicio** |   Identifica las redes a los que el equipo se ha conectado, recopila y almacena las propiedades de estas redes y notifica a las aplicaciones cuando se modifican estas propiedades.
|   **Nombre de servicio**    |   netprofm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Reconocimiento de ubicación de red           
| | |           
|---|---|       
|   **Descripción del servicio** |   Recopila y almacena la información de configuración de la red y notifica a los programas cuando se modifica esta información. Si se detiene el servicio, la información de configuración es posible que esté disponible. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   NlaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Servicio de configuración de red       
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio del programa de instalación de red administra la instalación de controladores de red y permite la configuración de red de bajo nivel.  Si se detiene el servicio, puede cancelarse cualquier instalación de controladores que está en curso.
|   **Nombre de servicio**    |   NetSetupSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Servicio de interfaz de la tienda de la red      
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio proporciona notificaciones de red (por ejemplo, interfaz adición o eliminación de etcetera) a los clientes de modo de usuario. Detener este servicio hará que la pérdida de conectividad de red. Si este servicio está deshabilitado, cualquier otro servicio que dependa de este servicio no podrán iniciarse.
|   **Nombre de servicio**    |   nSi
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="offline-files"></a>Archivos sin conexión            
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de archivos sin conexión realiza actividades de mantenimiento en la memoria caché de archivos sin conexión, responde a cierre de sesión y el inicio de sesión de usuario de eventos, implementa los aspectos internos de la API pública y procesa eventos interesantes para aquellas personas interesadas en las actividades de archivos sin conexión y cambios de estado de la caché.
|   **Nombre de servicio**    |   CscService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Optimizar las unidades          
| | |           
|---|---|   
|   **Descripción del servicio** |   Ayuda a que el equipo funcionen de forma más eficaz mediante la optimización de archivos en unidades de almacenamiento.
|   **Nombre de servicio**    |   defragsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL de contador de rendimiento         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite a los usuarios remotos y procesos de 64 bits consultar los contadores de rendimiento de los DLL de 32 bits. Si se detiene el servicio, solo los usuarios locales y los procesos de 32 bits podrán consultar los contadores de rendimiento proporcionados por la DLL de 32 bits.
|   **Nombre de servicio**    |   PerfHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Alertas y registros de rendimiento            
| | |           
|---|---|   
|   **Descripción del servicio** |   Registros de rendimiento y alertas recopila datos de rendimiento de equipos locales o remotos, en función de parámetros preconfigurados de programación, a continuación, escribe los datos en un registro o desencadena una alerta. Si se detiene el servicio, no se recopilarán información de rendimiento. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   Pla
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Servicio de teléfono       
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el estado de la telefonía en el dispositivo
|   **Nombre de servicio**    |   PhoneSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Usado por las aplicaciones de VoIP modernas
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug and Play       
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a un equipo a reconocer y adaptarse a los cambios de hardware con poca o ninguna entrada del usuario. Al detener o deshabilitar este servicio se producirá inestabilidad del sistema.
|   **Nombre de servicio**    |   PlugPlay
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Servicio de enumerador de dispositivo portátil           
| | |           
|---|---|       
|   **Descripción del servicio** |   Aplica la directiva de grupo para dispositivos de almacenamiento masivo extraíbles. Permite que las aplicaciones, como el Reproductor de Windows Media y Asistente para importación de imagen para transferir y sincronizar contenido mediante dispositivos de almacenamiento masivo extraíbles.
|   **Nombre de servicio**    |   WPDBusEnum
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="power"></a>Energía            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la directiva de energía y entrega de notificaciones de directiva de energía.
|   **Nombre de servicio**    |   Energía
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Cola de impresión            
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio se pone en cola trabajos de impresión y controla la interacción con la impresora.  Si desactiva este servicio, no podrás imprimir o ver las impresoras.
|   **Nombre de servicio**    |   Cola de impresión
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar si no es un servidor de impresión o un controlador de dominio
|   **Comentarios**    |   En un controlador de dominio, la instalación de la función de controlador de dominio agrega un subproceso para el servicio de la cola que se encarga de realizar la eliminación de impresión: quitar los objetos de la cola de impresión obsoletos de Active Directory.  Si el servicio de cola no se ejecutara en al menos un controlador de dominio en cada sitio, a continuación, el anuncio no tiene forma quitar colas anteriores que ya no existen. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Las notificaciones y las extensiones de impresora        
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio abre cuadros de diálogo de impresora personalizado y controla las notificaciones desde un servidor de impresión remoto o una impresora. Si desactiva este servicio, no podrás ver las extensiones de la impresora o notificaciones.
|   **Nombre de servicio**    |   PrintNotify
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar si no es un servidor de impresión
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Informes de problemas y soluciones compatibilidad del Panel de Control     
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio proporciona compatibilidad para ver, enviar y eliminación de informes de problemas de nivel del sistema para el panel de control de informes de problemas y soluciones.
|   **Nombre de servicio**    |   wercplsupport
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Servicio de compatibilidad de programa     
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio proporciona compatibilidad con el Asistente de compatibilidad de programas (PCA).  PCA supervisa los programas instalados y ejecutadas por el usuario y detecta los problemas de compatibilidad conocidos. Si se detiene el servicio, PCA no funcionará correctamente.
|   **Nombre de servicio**    |   PcaSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Experiencia de vídeo Audio de Windows de calidad      
| | |           
|---|---|   
|   **Descripción del servicio** |   Experiencia de vídeo de Audio de Windows de calidad (qWave) es una plataforma de red para Audio vídeo (AV), aplicaciones en redes domésticas IP de transmisión. qWave mejora el rendimiento y fiabilidad asegurándose de red calidad de servicio (QoS) para aplicaciones de AV de transmisión de AV. Proporciona mecanismos de control de admisión, ejecutar supervisión de tiempo y la aplicación, comentarios de la aplicación y priorización de tráfico.
|   **Nombre de servicio**    |   QWAVE
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servicio de QoS de cliente
|||         
            
<br />          

##      <a name="radio-management-service"></a>Servicio de administración de radio        
| | |           
|---|---|   
|   **Descripción del servicio** |   Administración de radio y el servicio de modo de avión
|   **Nombre de servicio**    |   RmSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Administrador de conexión automática de acceso remoto            
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea una conexión a una red remota, siempre que un programa hace referencia a un nombre DNS o NetBIOS remoto o una dirección.
|   **Nombre de servicio**    |   RasAuto
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Administrador de conexiones de acceso remoto         
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra las conexiones de red privada telefónico y virtual (VPN) desde este equipo a Internet o a otras redes remotas. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   RasMan
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configuración de escritorio remoto         
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de configuración de escritorio remoto (RDC) es responsable de todos los servicios de escritorio remoto y escritorio remoto configuración relacionados y las actividades de mantenimiento de sesión que requieren contexto de sistema. Estos incluyen carpetas temporales por sesión, los temas de escritorio remoto y certificados de escritorio remoto.
|   **Nombre de servicio**    |   SessionEnv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Servicios de escritorio remoto          
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los usuarios conectar de forma interactiva a un equipo remoto. Este servicio dependen de escritorio remoto y el servidor de Host de sesión de escritorio remoto.  Para evitar el uso remoto de este equipo, desactive las casillas de verificación en la pestaña remoto del elemento del panel de control de sistema de propiedades.
|   **Nombre de servicio**    |   Inicialización
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector del puerto de modo de usuario de servicios de escritorio remoto        
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite la redirección de impresoras unidades o puertos para las conexiones RDP
|   **Nombre de servicio**    |   UmRdpService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Admite redirecciones en el servidor de la conexión.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>Llamada a procedimiento remoto (RPC)          
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio RPCSS es el Administrador de Control de servicios para servidores COM y DCOM. Realiza solicitudes de activaciones de objeto, resoluciones exportador de objeto y la recolección de servidores COM y DCOM. Si este servicio se detiene o se deshabilita, los programas que usan DCOM o COM no funcionará correctamente. Se recomienda encarecidamente que tienes la ejecución del servicio RPCSS.
|   **Nombre de servicio**    |   RpcSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localizador de procedimiento remoto (RPC) de llamada             
| | |               
|---|---|   
|   **Descripción del servicio** |   En Windows 2003 y versiones anteriores de Windows, el servicio Localizador de llamada a procedimiento remoto (RPC) administra la base de datos del servicio de nombres RPC. En Windows Vista y versiones posteriores de Windows, este servicio no proporciona ninguna funcionalidad y está presente compatibilidad de aplicaciones.   |
|   **Nombre de servicio**    |   RpcLocator  |
|   **Instalación**    |   Solo en Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendación**  | Ninguna directriz   |
|   **Comentarios**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro remoto          
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los usuarios remotos modificar la configuración del registro en este equipo. Si se detiene el servicio, el registro se puede modificar los usuarios en este equipo. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   RemoteRegistry
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Conjunto resultante de proveedor de directivas            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un servicio de red que procesa las solicitudes para simular la aplicación de la configuración de directiva de grupo para un usuario de destino o el equipo en diversas situaciones y calcula los valores de conjunto resultante de directivas.
|   **Nombre de servicio**    |   RSoPProv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |Ninguna directriz    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Enrutamiento y acceso remoto            
| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece servicios de enrutamiento a empresas en entornos de red de área extensa y local.
|   **Nombre de servicio**    |   Acceso remoto
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   Ya se ha deshabilitado
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Asignador de RPC          
| | |           
|---|---|   
|   **Descripción del servicio** |   Resuelve los identificadores de interfaces RPC a extremos de transporte. Si este servicio se detiene o se deshabilita, los programas que usan servicios de llamada a procedimiento remoto (RPC) no funcionará correctamente.
|   **Nombre de servicio**    |   RpcEptMapper
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Inicio de sesión secundario     
| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita procesos con credenciales alternativas de inicio. Si se detiene el servicio, este tipo de acceso de inicio de sesión estará disponible. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   seclogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Servicio de protocolo de túnel de sockets seguras            
| | |               
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad para el protocolo sockets seguros túnel (SSTP) para conectarse a equipos remotos con VPN. Si este servicio está deshabilitada, los usuarios no podrán usar SSTP para acceder a servidores remotos.    |
|   **Nombre de servicio**    |   SstpSvc |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   No deshabilite  |
|   **Comentarios**    |   Deshabilitar los saltos de RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Administrador de cuentas de seguridad            
| | |           
|---|---|       
|   **Descripción del servicio** |   El inicio de este servicio indica a otros servicios que el Administrador de cuentas de seguridad (SAM) está listo para aceptar solicitudes.  Deshabilitación de este servicio impedirá otros servicios en el sistema se le notifique cuando está listo SAM, que puede causar que dichos servicios no pueda iniciarse correctamente. Este servicio no debe estar deshabilitado.
|   **Nombre de servicio**    |   SamSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Servicio de sensor de datos  
| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece datos entre una variedad de sensores
|   **Nombre de servicio**    |   SensorDataService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Servicio de supervisión de sensor            
| | |           
|---|---|       
|   **Descripción del servicio** |   Supervisa varios sensores para exponer los datos y se adaptan al estado de usuario y del sistema.  Si este servicio se detiene o se deshabilita, el brillo de la pantalla no se adaptará a las condiciones de iluminación. Detener este servicio puede afectar a otros funcionalidad del sistema y características.
|   **Nombre de servicio**    |   SensrSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Servicio de sensor           
| | |           
|---|---|       
|   **Descripción del servicio** |   Un servicio para los sensores que administra la funcionalidad de diferentes de los sensores. Administra orientación Simple de dispositivo (SDO) y el historial de los sensores. Carga el sensor SDO que informa de los cambios de orientación del dispositivo.  Si este servicio se detiene o se deshabilita, no se cargará el sensor SDO y por lo tanto, no se produzca la rotación automática. También se detendrá la recopilación de historial de los sensores.
|   **Nombre de servicio**    |   SensorService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="server"></a>Servidor           
| | |           
|---|---|   
|   **Descripción del servicio** |   Admite el archivo, imprimir y compartir a través de la red para este equipo de canalización con nombre. Si se detiene el servicio, estas funciones estará disponibles. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   LanmanServer
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Es necesario para la administración remota, IPC$, uso compartido de archivos SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Detección de Hardware shell             
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona notificaciones para eventos de hardware de reproducción automática.
|   **Nombre de servicio**    |   ShellHWDetection
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Tarjeta inteligente           
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el acceso a este equipo leer las tarjetas inteligentes. Si se detiene el servicio, este equipo se pueda leer las tarjetas inteligentes. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   SCardSvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Servicio de enumeración de dispositivos de tarjeta inteligente                    
| | |               
|---|---|       
|   **Descripción del servicio** |   Crea los nodos de dispositivo de software para todos los lectores de tarjetas inteligentes pueden acceder a una sesión determinada. Si este servicio está deshabilitado, APIs WinRT no podrá enumerar los lectores de tarjetas inteligentes.   |
|   **Nombre de servicio**    |   ScDeviceEnum    |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Aceptar para deshabilitar   |
|   **Comentarios**    |   Es necesario casi exclusivamente para las aplicaciones de WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Directiva de extracción de tarjeta inteligente        
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que el sistema esté configurado para bloquear el escritorio del usuario al quitar la tarjeta inteligente.
|   **Nombre de servicio**    |   SCPolicySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Captura SNMP            
| | |           
|---|---|       
|   **Descripción del servicio** |   Recibe mensajes de captura generados por agentes de Protocolo Simple de administración de red (SNMP) local o remota y reenvía los mensajes a programas de administración de SNMP ejecutando en este equipo. Si este servicio se detiene, los programas basados en SNMP en este equipo no recibirá mensajes de captura SNMP. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   SNMPTRAP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Protección de software             
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la descarga, instalación y la aplicación de licencias digitales para Windows y Windows aplicaciones. Si el servicio está deshabilitado, el sistema operativo y las aplicaciones con licencia pueden ejecutar en un modo de notificación. Se recomienda encarecidamente que no deshabilitas el servicio de protección de Software.
|   **Nombre de servicio**    |   sppsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Auxiliar de la consola de administración especial        
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los administradores acceso remoto a un símbolo del sistema mediante los servicios de administración de emergencia.
|   **Nombre de servicio**    |   Sacsvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Comprobador de manchas            
| | |           
|---|---|   
|   **Descripción del servicio** |   Comprueba los posibles daños de sistema de archivos.
|   **Nombre de servicio**    |   svsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Detección SSDP           
| | |           
|---|---|   
|   **Descripción del servicio** |   Descubre los dispositivos en red y servicios que usan el protocolo de detección SSDP, como los dispositivos UPnP. También se anuncia dispositivos SSDP y servicios que se ejecutan en el equipo local. Si se detiene el servicio, no se detectarán dispositivos basados en SSDP. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   SSDPSRV
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Servicio de repositorio de estado         
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura necesaria para el modelo de aplicaciones.
|   **Nombre de servicio**    |   StateRepository
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Eventos de adquisición de imágenes estáticas
| | |           
|---|---|   
|   **Descripción del servicio** |   Inicia aplicaciones asociadas con los eventos de adquisición de imágenes aún.
|   **Nombre de servicio**    |   WiaRpc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Servicio de almacenamiento          
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona servicios de activación para la configuración de almacenamiento y la expansión de almacenamiento externo
|   **Nombre de servicio**    |   StorSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Administración de niveles de almacenamiento        
| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza la colocación de los datos en los niveles de almacenamiento de todos los espacios de almacenamiento en el sistema.
|   **Nombre de servicio**    |   TieringEngineService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene y mejora el rendimiento del sistema con el tiempo.
|   **Nombre de servicio**    |   SysMain
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host de sincronización            
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio sincroniza correo, contactos, calendario y varios otros datos de usuario. Correo electrónico y otras aplicaciones que dependen de esta funcionalidad no funcionan correctamente cuando este servicio no se está ejecutando.
|   **Nombre de servicio**    |   OneSyncSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Servicio de notificación de eventos de sistema            
| | |           
|---|---|       
|   **Descripción del servicio** |   Controla los eventos del sistema y notifica a los suscriptores COM + sistema de eventos de estos eventos.
|   **Nombre de servicio**    |   TENOR
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Agente de eventos del sistema             
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordenadas de ejecución del trabajo en segundo plano para aplicaciones de WinRT. Si este servicio se detiene o se deshabilita, trabajo en segundo plano no puede activarse.
|   **Nombre de servicio**    |   SystemEventsBroker
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   A pesar de que su descripción implica es solo para aplicaciones de WinRT, se necesita para programador de tareas, el servicio de infraestructura de agente y otros componentes internos.
|||         
            
<br />          

## <a name="task-scheduler"></a>Programador de tareas           
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite al usuario configurar y programar tareas automatizadas en este equipo. El servicio también aloja varias tareas críticas en el sistema de Windows. Si este servicio se detiene o se deshabilita, no se ejecutará estas tareas en su programada veces. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   Programación
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>Ayuda de NetBIOS sobre TCP/IP            
| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece compatibilidad con NetBIOS sobre TCP/IP (NetBT) y la resolución de nombre NetBIOS para clientes de la red, por lo tanto, permite que los usuarios para compartir archivos, imprimir e iniciar sesión en la red. Si se detiene el servicio, es posible que estas funciones no estén disponibles. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   LMHOSTS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telefonía           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad con la API de telefonía (TAPI) para programas que controlan los dispositivos en el equipo local y, a través de la LAN, en los servidores que también se esté ejecutando el servicio de telefonía.
|   **Nombre de servicio**    |   TapiSrv
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Deshabilitar los saltos de RRAS
|||         
            
<br />          

## <a name="themes"></a>Temas           
| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona administración de tema de experiencia de usuario.
|   **Nombre de servicio**    |   Temas
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   No se puede establecer los temas de accesibilidad cuando este servicio está deshabilitado
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Servidor de modelo de datos de icono           
| | |           
|---|---|   
|   **Descripción del servicio** |   Servidor de iconos para las actualizaciones de icono.
|   **Nombre de servicio**    |   tiledatamodelsvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Iniciar saltos de menú si este servicio está deshabilitado
|||         
            
<br />          

##  <a name="time-broker"></a>Agente de tiempo     
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordenadas de ejecución del trabajo en segundo plano para aplicaciones de WinRT. Si este servicio se detiene o se deshabilita, trabajo en segundo plano no puede activarse.
|   **Nombre de servicio**    |   TimeBrokerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   A pesar de que su descripción implica es solo para aplicaciones de WinRT, se necesita para programador de tareas, el servicio de infraestructura de agente y otros componentes internos.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Teclado táctil y los servicios del Panel de escritura a mano         
| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita la funcionalidad de lápiz y entrada de lápiz de teclado táctil y Panel de escritura a mano
|   **Nombre de servicio**    |   TabletInputService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Servicio de Orchestrator de actualización de Windows Update           
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las actualizaciones de Windows. Si se detiene, los dispositivos no podrá descargar e instalar las actualizaciones más recientes.
|   **Nombre de servicio**    |   UsoSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Descripción del servicio faltaba en v1607; Actualización de Windows (incluido WSUS) depende de este servicio.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Dispositivo Host de UPnP         
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los dispositivos UPnP se hospeden en este equipo. Si este servicio se detiene, los dispositivos UPnP hospedados dejarán de funcionar y no puede agregarse ningún dispositivo hospedado adicional. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   UPnPHost
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Servicio de registro de acceso de usuario          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio registra las solicitudes de acceso de clientes únicas, en el formulario de direcciones IP y nombres de usuario de productos instalados y roles en el servidor local. Esta información se puede consultar, mediante Powershell, los administradores que necesitan cuantificar demanda de cliente de software de servidor de administración de acceso de cliente (CAL) sin conexión. Si se deshabilita el servicio, las solicitudes de cliente no se registrará y no se pueden recuperables a través de las consultas de Powershell. Detener el servicio no afectará a la consulta de datos históricos (consulta la documentación para conocer los pasos eliminar los datos históricos complementaria). El administrador del sistema local debe consultar sus o sus, términos de licencia de Windows Server para determinar el número de CAL que son necesarias para el software de servidor se licencie correctamente; utiliza el servicio UAL y los datos no modifiquen esta obligación.
|   **Nombre de servicio**    |   UALSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Acceso a datos de usuario        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona acceso de aplicaciones a los datos de usuario estructurados, incluida la información de contacto, calendarios, mensajes y otros contenidos. Si se detiene o deshabilitar este servicio, las aplicaciones que usan estos datos podrían no funcionar correctamente.
|   **Nombre de servicio**    |   UserDataSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="user-data-storage"></a>Almacenamiento de datos de usuario            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra el almacenamiento estructurado de datos de usuario, incluida la información de contacto, calendarios, mensajes y otros contenidos. Si se detiene o deshabilitar este servicio, las aplicaciones que usan estos datos podrían no funcionar correctamente.
|   **Nombre de servicio**    |   UnistoreSvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Servicio de virtualización de experiencia de usuario           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad para la aplicación y configuración del sistema operativo móviles
|   **Nombre de servicio**    |   UevAgentService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Administrador de usuarios        
| | |           
|---|---|   
|   **Descripción del servicio** |   El Administrador de usuarios proporciona los componentes en tiempo de ejecución necesarios para la interacción de varios usuario.  Si se detiene el servicio, es podrán que algunas aplicaciones no funcione correctamente.
|   **Nombre de servicio**    |   UserManager
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Servicio de perfil de usuario         
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio es responsable de carga y descarga de perfiles de usuario. Si este servicio se detiene o se deshabilita, los usuarios ya no podrán iniciar sesión correctamente en o cerrar sesión, aplicaciones podrían tener problemas para acceder a datos del usuario los y componentes registrados para recibir notificaciones de eventos del perfil no recibirán.
|   **Nombre de servicio**    |   ProfSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtual             
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de administración de discos, volúmenes, sistemas de archivos y las matrices de almacenamiento.
|   **Nombre de servicio**    |   VDS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Ninguna directriz
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Instantáneas de volumen           
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra e implementa instantáneas de volumen usado para la copia de seguridad y otros fines. Si se detiene el servicio, instantáneas de volumen no estará disponibles para la copia de seguridad y puede producir un error en la copia de seguridad. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   VSS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Ninguna directriz
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descripción del servicio** |   Objetos de hosts que se usan los clientes de la cartera
|   **Nombre de servicio**    |   WalletService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Audio de Windows            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra el audio para programas basados en Windows.  Si se detiene el servicio, los dispositivos de audio y efectos no funcionarán correctamente.  Si este servicio está deshabilitado, se producirá un error en los servicios que dependen explícitamente a inicio
|   **Nombre de servicio**    |   AudioSrv
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Generador de extremo de Audio de Windows           
| | |           
|---|---|
|   **Descripción del servicio** |   Administra dispositivos de audio para el servicio de Audio de Windows.  Si se detiene el servicio, los dispositivos de audio y efectos no funcionarán correctamente.  Si este servicio está deshabilitado, se producirá un error en los servicios que dependen explícitamente a inicio
|   **Nombre de servicio**    |   AudioEndpointBuilder
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Servicio de datos biométricos de Windows            
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de datos biométrico de Windows ofrece a las aplicaciones de cliente la capacidad de capturar, comparar, manipular y almacenar datos biométricos sin obtener un acceso directo a cualquier hardware biométrico o muestras. El servicio se hospeda en un proceso SVCHOST con privilegios.
|   **Nombre de servicio**    |   WbioSrvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Marco de cámara de Windows Server         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que varios clientes acceder a los fotogramas de vídeo de dispositivos de cámara.
|   **Nombre de servicio**    |   FrameServer
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Administrador de conexiones de Windows           
| | |           
|---|---|   
|   **Descripción del servicio** |   Hace automática para conectar o desconectar decisiones en función de las opciones de conectividad de red disponibles actualmente para el equipo y permite la administración de conectividad de red en función de la configuración de directiva de grupo.
|   **Nombre de servicio**    |   Wcmsvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Servicio de inspección de red de Windows Defender          
| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a protegerlo contra los intentos de intrusiones destinados a vulnerabilidades conocidas y recientemente detectadas en protocolos de red
|   **Nombre de servicio**    |   WdNisSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Servicio de Windows Defender         
| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a protege a los usuarios de malware y otro software potencialmente no deseado
|   **Nombre de servicio**    |   WinDefend
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Controladores de Windows Foundation - marco de controlador de modo de usuario           
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y administra los procesos de controlador de modo de usuario. No se puede detener este servicio.
|   **Nombre de servicio**    |   wudfsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Servicio de Host del proveedor de cifrado de Windows     
| | |           
|---|---|   
|   **Descripción del servicio** |   Cifrado de agentes del servicio de Host de proveedor de cifrado de Windows relacionados con los procesos que necesitan para evaluar y aplicar directivas EAS funcionalidades de otros proveedores de cifrado. Si se detiene compromete comprobaciones de cumplimiento EAS que se han establecido las cuentas de correo conectado
|   **Nombre de servicio**    |   WEPHOSTSVC
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Servicio de informes de errores de Windows          
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite errores cuando los programas dejan de funcionar o no responde y soluciones existentes que se entreguen. También permite registros de diagnóstico generarse y servicios de reparación. Si se detiene el servicio informe de errores podría no funcionar correctamente y no se muestren los resultados de los servicios de diagnóstico y reparaciones.
|   **Nombre de servicio**    |   WerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Recopila y envía datos de bloqueo o utilizados por Microsoft y terceros ISV/IHV. Los datos se usan para diagnosticar errores inducir de bloqueo, que pueden incluir errores de seguridad. También fue necesario para informes de errores corporativos
|||         
            
<br />          

## <a name="windows-event-collector"></a>Recopilador de eventos de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio administra persistentes suscripciones a eventos desde orígenes remotos que admiten el protocolo de WS-Management. Esto incluye los registros de eventos de Windows Vista, hardware y orígenes de eventos con IPMI. El servicio almacena los eventos reenviados en un registro de eventos locales. Si este servicio se detiene o se deshabilita no se puede crear suscripciones a eventos y eventos reenviados no se aceptarán.
|   **Nombre de servicio**    |   Wecsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Recopila eventos ETW (incluidos los eventos de seguridad) para la administración de diagnóstico.  Muchas características y herramientas de terceros dependen de ella, incluidas las herramientas de auditoría de seguridad
|||         
            
<br />          

## <a name="windows-event-log"></a>Registro de eventos de Windows            
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio administra los eventos y registros de eventos. Admite eventos de registro, eventos, suscribirse a eventos, el archivado de registros de eventos y administrar metadatos de evento de consulta. Puede mostrar eventos en formato XML y texto sin formato. Detener este servicio puede comprometer la seguridad y confiabilidad del sistema.
|   **Nombre de servicio**    |   Registro de eventos
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Firewall de Windows         
| | |           
|---|---|   
|   **Descripción del servicio** |   Firewall de Windows te ayuda a proteger su equipo si impide que los usuarios no autorizados tengan acceso al equipo a través de Internet o a una red.
|   **Nombre de servicio**    |   MpsSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Servicio de caché de fuentes de Windows      
| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza el rendimiento de las aplicaciones al almacenar en caché los datos de fuente más usados. Las aplicaciones iniciarán este servicio si no se está ejecutando. Se puede deshabilitar, pero por lo tanto, se degradará el rendimiento de la aplicación.
|   **Nombre de servicio**    |   FontCache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Adquisición de imágenes de Windows (WIA)          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de adquisición de imágenes para escáneres y cámaras
|   **Nombre de servicio**    |   stisvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Servicio de Windows Insider     
| | |           
|---|---|   
|   **Descripción del servicio** |   wisvc
|   **Nombre de servicio**    |   wisvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servidor no admite la distribución de paquetes piloto, por lo que es una operación no en el servidor. También se puede deshabilitar característica a través de GP.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Descripción del servicio** |   Agrega, modifica y quita aplicaciones proporcionadas como un paquete de Windows Installer (*.msi, *.msp). Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   MSIServer
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Servicio de administrador de licencias de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura de Microsoft Store.  Este servicio se inicia a petición y si está deshabilitada, contenido adquirido a través de Microsoft Store no funcionará correctamente.
|   **Nombre de servicio**    |   LicenseManager
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Instrumental de administración de Windows       
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un modelo común de interfaz y un objeto para acceder a información de administración sobre el sistema operativo, dispositivos, aplicaciones y servicios. Si se detiene el servicio, la mayoría del software basado en Windows no funcionará correctamente. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   WinMgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Servicio de la zona Internet móvil de Windows          
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la capacidad de compartir una conexión de datos con otro dispositivo.
|   **Nombre de servicio**    |   icssvc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Instalador de módulos de Windows        
| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita la instalación, modificación y eliminación de las actualizaciones de Windows y los componentes opcionales. Si este servicio está deshabilitado, instalar o desinstalar de Windows que se pueden producir un error de actualizaciones para este equipo.
|   **Nombre de servicio**    |   TrustedInstaller
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>El servicio del sistema de notificaciones de inserción de Windows            
| | |           
|---|---|
|   **Descripción del servicio** |   Este servicio se ejecuta en sesión 0 y hospeda el proveedor de plataforma y la conexión de notificación que controla la conexión entre el dispositivo y el servidor WNS.
|   **Nombre de servicio**    |   WpnService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Necesario para iconos dinámicos y otras características
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>El servicio de usuario de las notificaciones de inserción de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio hospeda plataforma de notificaciones de Windows que proporciona compatibilidad para local y notificaciones de inserción. Notificaciones admitidas son el icono, notificación del sistema y sin procesar.
|   **Nombre de servicio**    |   WpnUserService
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Administración remota de Windows (WS-Management)            
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de administración remota de Windows (WinRM) implementa el protocolo de administración de WS para la administración remota. Administración de WS es un protocolo de servicios web estándar utilizado para la administración remota de software y hardware. El servicio WinRM escucha en la red para las solicitudes de WS-administración y las procese. El WinRM Service debe estar configurados con un agente de escucha mediante la herramienta de línea de comandos winrm.cmd o a través de la directiva de grupo para que pueda escuchar a través de la red. El servicio WinRM proporciona acceso a datos WMI y permite la recopilación de eventos. Colección de eventos y la suscripción a eventos requieren que se está ejecutando el servicio. WinRM mensajes usan HTTP y HTTPS como transportes. El servicio WinRM no depende de IIS, pero está preconfigurado para compartir un puerto con IIS en el mismo equipo.  El servicio WinRM reserva el prefijo de URL /wsman. Para evitar conflictos con IIS, los administradores deben asegurarse de que los sitios Web hospedados en IIS no uses el prefijo de URL /wsman.
|   **Nombre de servicio**    |   WinRM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Es necesario para la administración remota
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la indización de contenido, el almacenamiento en caché de propiedad y resultados de búsqueda para archivos, correo electrónico y otros contenidos.
|   **Nombre de servicio**    |   WSearch
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Deshabilitado
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Hora de Windows        
| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene la sincronización de fecha y hora en todos los clientes y servidores de la red. Si se detiene el servicio, no estará disponible la sincronización de fecha y hora. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   W32Time
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la detección, la descarga y la instalación de actualizaciones de Windows y otros programas. Si este servicio está deshabilitada, los usuarios de este equipo no podrán usar Windows Update o la característica Actualizaciones automáticas y programas no podrán usar la API de Windows Update Agent (WUA).
|   **Nombre de servicio**    |   wuauserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servicio de detección automática de Proxy Web WinHTTP         
| | |           
|---|---|   
|   **Descripción del servicio** |   WinHTTP implementa la pila de cliente HTTP y proporciona a los desarrolladores con una API de Win32 y un componente de automatización COM para enviar solicitudes HTTP y recibir respuestas. Además, WinHTTP proporciona compatibilidad para la detección automática de una configuración de proxy mediante la implementación del protocolo de detección automática de Proxy Web (WPAD).
|   **Nombre de servicio**    |   WinHttpAutoProxySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Algo que use la pila de red puede tener una dependencia funcional en este servicio. Muchas organizaciones dependen de esta opción para configurar a proxy HTTP de sus redes internas enrutamiento.  Sin ella originados internamente HTTP las conexiones a Internet se todos producirá un error.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Redes cableadas         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio cableadas (DOT3SVC) es responsable de realizar IEEE 802.1X autenticación en las interfaces de Ethernet. Si la actual implementación de red con cable exige la autenticación 802.1X, el servicio de DOT3SVC debe configurarse para ejecutar para establecer conectividad de nivel 2 o proporcionar acceso a recursos de red. Redes cableadas que no se aplican la autenticación 802.1X no se ven afectadas por el servicio de DOT3SVC.
|   **Nombre de servicio**    |   dot3svc
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Adaptador de rendimiento de WMI          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona información de la biblioteca de rendimiento de los proveedores de Instrumental de administración de Windows (WMI) a los clientes en la red. Este servicio solo se ejecuta cuando se activa la aplicación auxiliar de datos de rendimiento.
|   **Nombre de servicio**    |   wmiApSrv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | Ninguna directriz       
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="workstation"></a>Estación de trabajo          
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y mantiene las conexiones de red de cliente a servidores remotos mediante el protocolo SMB. Si se detiene el servicio, estas conexiones no estará disponibles. Si este servicio se deshabilita, todos los servicios que dependen explícitamente no podrán iniciarse.
|   **Nombre de servicio**    |   LanmanWorkstation
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | Ninguna directriz       
|   **Comentarios**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Administrador de Xbox Live de autenticación           
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de autenticación y autorización para interactuar con Xbox Live. Si se detiene el servicio, es podrán que algunas aplicaciones no funcione correctamente.
|   **Nombre de servicio**    |   XblAuthManager
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Guardar de juego de Xbox Live          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este se sincroniza de servicio guarda datos de Xbox Live guardar juegos habilitados.  Si se detiene el servicio, el juego para guardar datos no para descarga o carga de Xbox Live.
|   **Nombre de servicio**    |   XblGameSave
|   **Instalación**    |   Solo en Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitado
|   **Comentarios**    |   Este se sincroniza de servicio guarda datos de Xbox Live guardar juegos habilitados.  Si se detiene el servicio, el juego para guardar datos no para descarga o carga de Xbox Live.
|||         
                
<br /> 
<br /> 

