---
title: Directrices de seguridad para los servicios del sistema en Windows Server 2016
description: Directrices de seguridad para deshabilitar los servicios en Windows Server 2016 con la experiencia de escritorio
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 323985cf316bda2fa6ab6a1721e2b6316450391a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844476"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Orientación acerca de cómo deshabilitar los servicios del sistema en Windows Server 2016 con experiencia de escritorio

Se aplica a: Windows Server 2016

El sistema operativo de Windows incluye muchos servicios del sistema que proporcionan una funcionalidad importante. Servicios diferentes tienen directivas de inicio predeterminada diferente: algunas se inician de forma predeterminada (automático), algunos cuando sea necesario (manuales), y algunos están deshabilitados de forma predeterminada y deben habilitarse explícitamente antes de poder ejecutarlos. Estos valores predeterminados se eligieron cuidadosamente para que cada servicio equilibrar el rendimiento, funcionalidad y seguridad para clientes.

Sin embargo, algunos clientes empresariales que prefiera un equilibrio más especializado en seguridad para sus equipos de Windows y servidores, uno que reduce su ataque expuesta al mínimo absoluto y, por lo tanto, quizás desee deshabilitar totalmente todos los servicios que no son necesarios en su específico entornos. Para los clientes, Microsoft® proporciona las instrucciones que lo acompaña respecto a qué servicios se pueden deshabilitar con seguridad para este propósito.

Las instrucciones son solo para Windows Server 2016 con experiencia de escritorio (a menos que se usa como un sustituto de sobremesa a los usuarios finales). A partir de Windows Server 2019, estas instrucciones se configuran de forma predeterminada. Cada servicio en el sistema se clasifica por categorías como sigue:

-   **Debe deshabilitar:** Una empresa especializado en seguridad probablemente preferirán deshabilitar este servicio y Prescinda de su funcionalidad (vea los detalles adicionales a continuación).
- **Clic en Aceptar para deshabilitar:** Este servicio proporciona la funcionalidad que es útil para algunos, pero no todas las empresas y las empresas centrados en la seguridad que no se usa lo pueden deshabilitar sin riesgos.
- **No deshabilite:** Si se deshabilita este servicio se afecta a la funcionalidad esencial o impedir que funcione correctamente las características o roles específicos. Por lo tanto no debe deshabilitarse.
-  **(No hay instrucciones):** Las consecuencias de deshabilitar estos servicios no se ha evaluado totalmente. Por lo tanto, no debe cambiar la configuración predeterminada de estos servicios.


Los clientes pueden configurar sus equipos de Windows y los servidores para deshabilitar los servicios seleccionados mediante las plantillas de seguridad en sus directivas de grupo o mediante la automatización de PowerShell. En algunos casos, la guía incluye valores específicos de directiva de grupo que deshabilita la funcionalidad del servicio directamente, como alternativa al deshabilitar el propio servicio.

Microsoft recomienda que los clientes deshabiliten los siguientes servicios y sus respectivas tareas programadas en Windows Server 2016 con experiencia de escritorio:

Servicios: 
1. Administrador de Xbox Live Auth
2. Guardar juego de Xbox Live

Tareas programadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(También puede tener acceso a la información en todos los servicios descritos en este artículo mediante la visualización de la hoja de cálculo de Microsoft Excel adjunto: [Orientación acerca de cómo deshabilitar los servicios del sistema en Windows Server 2016 con experiencia de escritorio](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>Deshabilitación de los servicios que no se instala de forma predeterminada

Microsoft recomienda no aplicar directivas para deshabilitar los servicios que no se instalan de forma predeterminada.
-  El servicio suele ser necesario si está instalada la característica. Instalar el servicio o la característica requiere derechos administrativos. No permitir la instalación de características, no el inicio del servicio.
-  Bloquea el servicio de Microsoft Windows no detiene un administrador (o sin derechos administrativos en algunos casos) de instalación de un equivalente de terceros similar, quizás uno con un mayor riesgo de seguridad.
-  Una línea base o un banco de pruebas que se deshabilita un servicio de Windows no predeterminado (por ejemplo, W3SVC) proporcionará a algunos auditores la impresión equivocada de que la tecnología (por ejemplo, IIS) es inherentemente insegura y nunca debe utilizarse.
-  Si nunca se instalen la característica (y el servicio), esto agrega simplemente masiva innecesario a la línea base y al trabajo de comprobación.

<br />
Para todos los servicios del sistema aparecen en este documento, las dos tablas siguientes ofrecen una explicación de las columnas y las recomendaciones de Microsoft para habilitar y deshabilitar servicios del sistema en Windows Server 2016 con experiencia de escritorio: 
 
<br />

### <a name="explanation-of-columns"></a>Explicación de las columnas

| | |
|---|---|
|**Descripción del servicio**|   La descripción del servicio, desde qdescription sc.exe.|
|**Name** |Nombre de la clave (interno) del servicio|
|**Instalación** |Siempre se instala: Servicio está en Server Core y Server con experiencia de escritorio  <br /> Solo con la experiencia de escritorio: Servicio en Windows Server 2016 con experiencia de escritorio, pero es ***no*** en Server Core |
|**StartType**  |Tipo de inicio en Windows Server 2016|
|**Recomendación** |Recomendación de Microsoft o aviso acerca de cómo deshabilitar este servicio en Windows Server 2016 en una implementación empresarial típico y bien administrado y donde el servidor no se se utiliza como un reemplazo de escritorio del usuario final.|
|**Comentarios** |Explicación adicional|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explicación de las recomendaciones de Microsoft

| | |
|---|---|
|**No deshabilite** |No se debe deshabilitar este servicio|
|**Aceptar para deshabilitar**| Este servicio puede deshabilitarse si no se utiliza la característica que admite.|
|**Ya se ha deshabilitado**|  Este servicio está deshabilitado de forma predeterminada; no es necesario aplicar la directiva|
|**Debe estar deshabilitada** |Este servicio no debe habilitarse nunca en un sistema empresariales bien administrados.|

<br />

Las tablas siguientes ofrecen instrucciones de Microsoft acerca de cómo deshabilitar los servicios del sistema en Windows Server 2016 con experiencia de escritorio:

<br />

##  <a name="activex-installer-axinstsv"></a>Instalador de ActiveX (AxInstSV)

| | |
|---|---|
|   **Descripción del servicio** |   Proporciona la validación de Control de cuentas de usuario para la instalación de controles ActiveX de Internet y permite la administración de la instalación del control ActiveX según la configuración de directiva de grupo. Este servicio se inicia a petición y si deshabilita la instalación de controles ActiveX se comportará según la configuración predeterminada del explorador.    |
|   **Nombre del servicio**    |   AxInstSV    |
|   **Instalación**    |   Solo con la experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Aceptar para deshabilitar   |
|   **Comentarios**    |   Aceptar para deshabilitar si no necesita la característica |


<br />

## <a name="alljoyn-router-service"></a>Servicio AllJoyn Router   
| | |
|---|---|
|   **Descripción del servicio** |   Enruta los mensajes de AllJoyn para los clientes de AllJoyn locales. Si se detiene este servicio los clientes de AllJoyn que no tienen sus propios enrutadores integrados no se pueden ejecutar. |
|   **Nombre del servicio**    |   AJRouter    |
|   **Instalación**    |   Solo con la experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  | No hay instrucciones       |
|   **Comentarios**    |       |
| | |

<br />

## <a name="app-readiness"></a>Preparación de la aplicación
| | |
|---|---|
**Descripción del servicio** |   Obtiene las aplicaciones para su uso la primera vez que un usuario inicia sesión en este equipo y al agregar nuevas aplicaciones.
**Nombre del servicio**    |   AppReadiness
**Instalación**    |   Solo con la experiencia de escritorio
**StartType**   |   Manual
**Recomendación**  |   No deshabilite
**Comentarios**    |   
| | |

<br />

##  <a name="application-identity"></a>Identidad de aplicación
| | |       
|---|---|   
**Descripción del servicio** |   Determina y comprueba la identidad de una aplicación. Si se deshabilita este servicio impedirá que AppLocker que se apliquen.
**Nombre del servicio**    |   AppIDSvc
**Instalación**    |   Siempre instalado
**StartType**   |   Manual
**Recomendación**  |No hay instrucciones    
**Comentarios**    |   
|||     

<br />

##  <a name="application-information"></a>Información de la aplicación 
| | |       
|---|---|   
|   **Descripción del servicio** |   Facilita la ejecución de aplicaciones interactivas con privilegios administrativos adicionales.  Si se detiene este servicio, los usuarios no podrán iniciar aplicaciones con los privilegios administrativos adicionales que puedan necesitar para realizar tareas de usuario deseado.
|   **Nombre del servicio**    |   Appinfo
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   Admite la elevación de UAC mismo escritorio
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Servicio de puerta de enlace de capa de aplicación       
| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona compatibilidad para complementos de protocolo de terceros para la conexión compartida a Internet
|   **Nombre del servicio**    |   ALG
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |No hay instrucciones    
|   **Comentarios**    |   
|||     

<br />

##  <a name="application-management"></a>Administración de aplicaciones      
| | |           
|---|---|       
|   **Descripción del servicio** |   Procesa las solicitudes de instalación, eliminación y enumeración para el software implementado a través de la directiva de grupo. Si el servicio está deshabilitado, los usuarios no podrán instalar, quitar o enumerar el software implementado a través de la directiva de grupo. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   AppMgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Servicios de implementación de AppX (AppXSVC)       
| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura para implementar aplicaciones de Store. Este servicio se inicia a petición y, si las aplicaciones de Store deshabilitadas no se implementará en el sistema y es posible que no funcionen correctamente.
|   **Nombre del servicio**    |   AppXSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Actualizador de zona horaria automática           
| | |           
|---|---|           
|   **Descripción del servicio** |   Establece automáticamente la zona horaria del sistema.
|   **Nombre del servicio**    |   tzautoupdate
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Servicio de transferencia inteligente en segundo plano          
| | |           
|---|---|   
|   **Descripción del servicio** |   Las transferencias de archivos en segundo plano mediante el ancho de banda de red inactivo. Si el servicio está deshabilitado, las aplicaciones que dependen de BITS, como Windows Update o MSN Explorer, podrá descargar automáticamente programas y otra información.
|   **Nombre del servicio**    |   BITS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Servicio de infraestructura de tareas en segundo plano      
| | |           
|---|---|   
|   **Descripción del servicio** |   Puede ejecutar el servicio de infraestructura de Windows que controla qué tareas en segundo plano en el sistema.
|   **Nombre del servicio**    |   BrokerInfrastructure
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Motor de filtrado de base            
| | |           
|---|---|       
|   **Descripción del servicio** |   Base Filtering Engine (BFE) es un servicio que administra el firewall y directivas de seguridad (IPsec) de protocolo de Internet e implementa el filtrado de modo de usuario. Detener o deshabilitar el servicio BFE reducirá significativamente la seguridad del sistema. También se producirá un comportamiento imprevisible en aplicaciones de servidor de seguridad y administración de IPsec.
|   **Nombre del servicio**    |   BFE
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Servicio de soporte técnico de Bluetooth            
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de Bluetooth admite descubrimiento y la asociación de dispositivos Bluetooth remotos.  Detener o deshabilitar este servicio puede provocar los dispositivos Bluetooth ya instalados pueden no funcionar correctamente e impedir nuevos dispositivos detectados o asociados.
|   **Nombre del servicio**    |   bthserv
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Aceptar para deshabilitar si no usa. Otro mecanismo de deshabilitación: https://technet.microsoft.com/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio de usuario se utiliza para escenarios de la plataforma de dispositivos conectados
|   **Nombre del servicio**    |   CDPUserSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagación de certificados     
| | |           
|---|---|
|   **Descripción del servicio** |   Copia los certificados de usuario y los certificados raíz de las tarjetas inteligentes en el almacén de certificados del usuario actual, detecta cuando se inserta una tarjeta inteligente en un lector de tarjeta inteligente y si es necesario, instala al Minicontrolador de tarjetas inteligentes Plug and Play.
|   **Nombre del servicio**    |   CertPropSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Servicio de licencias de cliente (ClipSVC)        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad de infraestructura con la Microsoft Store. Este servicio se inicia a petición y si las aplicaciones deshabilitadas compraron utilizando Microsoft Store no funcione correctamente.
|   **Nombre del servicio**    |   ClipSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Aislamiento de claves CNG
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de aislamiento de claves CNG se hospeda en el proceso LSA. El servicio proporciona aislamiento de procesos clave para las claves privadas y las operaciones criptográficas asociadas según sea necesario el criterio común. El servicio almacena y usa claves de larga duración en un proceso seguro que cumpla los requisitos de criterio común.
|   **Nombre del servicio**    |   KeyIso
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema de eventos COM +       
| | |           
|---|---|       
|   **Descripción del servicio** |   Es compatible con el sistema Event Notification Service (SENS), que proporciona la distribución automática de eventos a los componentes de modelo de objetos componentes (COM). Si el servicio está detenido, SENS se cerrará y no podrá proporcionar notificaciones de inicio de sesión y cierre de sesión. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   EventSystem
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Aplicación del sistema COM +     
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la configuración y el seguimiento de los componentes de + basado en modelo de objetos componentes (COM). Si el servicio se detiene, la mayoría de COM +-basado en componentes no funcionarán correctamente. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   COMSysApp
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Examinador de equipos        
| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene una lista actualizada de los equipos de la red y proporciona esta lista a los equipos designados como exploradores. Si se detiene este servicio, esta lista no se actualiza o se mantienen. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   Browser
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Servicio de plataforma de dispositivos conectados       
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio se utiliza para escenarios de dispositivos conectados y vidrio Universal
|   **Nombre del servicio**    |   CDPSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Telemetría y experiencias del usuario conectado     
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de telemetría y experiencias del usuario conectado habilita características que admiten las experiencias de usuario en la aplicación y está conectado. Además, este servicio administra la colección orientada a eventos y la transmisión de información de diagnóstico y uso (que se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando está habilitados los diagnósticos y la configuración de opciones de privacidad de uso bajo Comentarios y diagnósticos.
|   **Nombre del servicio**    |   DiagTrack
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Póngase en contacto con los datos        
| | |           
|---|---|       
|   **Descripción del servicio** |   Los índices de los datos de contacto para la búsqueda rápida de contacto. Si detiene o deshabilita este servicio, es posible que falte contactos desde los resultados de búsqueda.
|   **Nombre del servicio**    |   PimIndexMaintenanceSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Descripción del servicio** |   Administra la comunicación entre los componentes del sistema.
|   **Nombre del servicio**    |   CoreMessagingRegistrar
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Administrador de credenciales           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un almacenamiento seguro y la recuperación de credenciales a los usuarios, aplicaciones y paquetes de servicio de seguridad.
|   **Nombre del servicio**    |   VaultSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Servicios criptográficos           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona tres servicios de administración: Servicio de base de datos de catálogo, que confirma las firmas de archivos de Windows y permite a los nuevos programas a instalarse; Servicio de raíz, que agrega y quita los certificados de entidad de certificación raíz de confianza de este equipo; protegida y el servicio de actualización automática de certificados de raíz, que recupera los certificados raíz de Windows Update y habilitar en escenarios tales como SSL. Si se detiene este servicio, estos servicios de administración no funcionará correctamente. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   CryptSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Servicio de uso compartido de datos         
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona datos de intermediación entre aplicaciones.
|   **Nombre del servicio**    |   DsSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCP (recopilación de datos y publicación) es compatible con aplicaciones propias para cargar datos en la nube.
|   **Nombre del servicio**    |   DcpSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Iniciador de procesos de servidor DCOM         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio DCOMLAUNCH inicia servidores COM y DCOM en respuesta a las solicitudes de activación de objeto. Si este servicio está detenido o deshabilitado, los programas que utilizan COM o DCOM no funcionará correctamente. Se recomienda encarecidamente que tienen el servicio en ejecución DCOMLAUNCH.
|   **Nombre del servicio**    |   DcomLaunch
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |No hay instrucciones    
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Servicio de asociación de dispositivos      
| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita el emparejamiento entre el sistema y los dispositivos de red cableados o inalámbricos.
|   **Nombre del servicio**    |   DeviceAssociationService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Servicio de instalación de dispositivos      
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite a un equipo reconocer y adaptarse a los cambios de hardware con poca o ninguna intervención del usuario. Detener o deshabilitar este servicio dará como resultado inestabilidad del sistema.
|   **Nombre del servicio**    |   DeviceInstall
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Servicio de inscripción de administración de dispositivos        
| | |           
|---|---|       
|   **Descripción del servicio** |   Realiza actividades de inscripción de dispositivos para administración de dispositivos
|   **Nombre del servicio**    |   DmEnrollmentSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Administrador del programa de instalación de dispositivos         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la detección, descarga e instalación de software relacionados con el dispositivo. Si se deshabilita este servicio, los dispositivos pueden configurarse con software obsoleto y no funcionen correctamente.
|   **Nombre del servicio**    |   DsmSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Agente de detección DevQuery en segundo plano         
| | |           
|---|---|           
|   **Descripción del servicio** |   Permite que las aplicaciones detectar dispositivos con una tarea en segundo plano
|   **Nombre del servicio**    |   DevQueryBroker
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Cliente DHCP          
| | |           
|---|---|       
|   **Descripción del servicio** |   Registra y actualiza las direcciones IP y los registros DNS para este equipo. Si se detiene este servicio, este equipo no recibirá direcciones IP dinámicas y las actualizaciones de DNS. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   DHCP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Servicio de directivas de diagnóstico            
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio directiva de diagnóstico se habilita la detección de problemas, solución de problemas y resolución para los componentes de Windows.  Si se detiene este servicio, diagnóstico dejará de funcionar.
|   **Nombre del servicio**    |   DPS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host de servicio de diagnóstico     
| | |           
|---|---|       
|   **Descripción del servicio** |   El Host de servicio de diagnóstico se usa el servicio directiva de diagnóstico para diagnósticos de host que se deben ejecutar en un contexto de servicio Local.  Si se detiene este servicio, los diagnósticos que dependen de él dejará de funcionar.
|   **Nombre del servicio**    |   WdiServiceHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host de sistema de diagnóstico           
| | |           
|---|---|       
|   **Descripción del servicio** |   El Host del sistema de diagnóstico se usa el servicio directiva de diagnóstico para diagnósticos de host que se deben ejecutar en un contexto de sistema Local.  Si se detiene este servicio, los diagnósticos que dependen de él dejará de funcionar.
|   **Nombre del servicio**    |   WdiSystemHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Cliente de seguimiento de vínculos distribuidos            
| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene los vínculos entre los archivos NTFS en un equipo o en varios equipos en una red.
|   **Nombre del servicio**    |   TrkWks
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Coordinador de transacciones distribuidas     
| | |           
|---|---|   
|   **Descripción del servicio** |   Coordina las transacciones que abarcan varios administradores de recursos, como las bases de datos, las colas de mensajes y los sistemas de archivos. Si se detiene este servicio, se producirá un error en estas transacciones. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   MSDTC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de enrutamiento de mensajes de inserción WAP
|   **Nombre del servicio**    |   dmwappushservice
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servicio necesario en los dispositivos cliente para Intune, MDM y tecnologías similares de administración y para Unified Write Filter. No es necesario para el servidor.
|||         
            
<br />      

##  <a name="dns-client"></a>Cliente DNS      
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio Cliente DNS (dnscache) almacena en caché nombres del Sistema de nombres de dominio (DNS) y registra el nombre completo del equipo para este equipo. Si el servicio se para, los nombres DNS seguirán resolviéndose. Sin embargo, los resultados de consultas de nombres DNS no se almacenarán en caché y no se registrará el nombre del equipo. Si el servicio se deshabilita, cualquier servicio que dependa explícitamente de él no se podrá iniciar.
|   **Nombre del servicio**    |   Dnscache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Administrador de mapas descargado     
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de Windows para el acceso a las aplicaciones a maps descargados. Este servicio se inicia a petición mediante la aplicación acceso a descarga asignaciones. Si se deshabilita este servicio impedirá que las aplicaciones acceso a los mapas.
|   **Nombre del servicio**    |   MapsBroker
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Deshabilitación de las aplicaciones de saltos que se basan en el servicio; Aceptar deshabilitar si no depender de aplicaciones
|||         
            
<br />          

## <a name="embedded-mode"></a>Modo incrustado            
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio en modo incrustado habilita escenarios relacionados con las aplicaciones en segundo plano.  Si se deshabilita este servicio impedirá que las aplicaciones en segundo plano desde que se está activando.
|   **Nombre del servicio**    |   embeddedmode
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Sistema de archivos cifrados (EFS)
| | |                   
|---|---|           
|   **Descripción del servicio** | Proporciona la tecnología de cifrado de archivos principal utilizada para almacenar archivos cifrados en volúmenes de sistema de archivos NTFS. Si este servicio está detenido o deshabilitado, las aplicaciones no se pueden obtener acceso a archivos cifrados.            
|   **Nombre del servicio**  |  EFS            
|   **Instalación**  |  Siempre instalado           
|   **StartType**   |  Manual           
|   **Recomendación**  | No hay instrucciones           
|   **Comentarios**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Servicio de administración de aplicación empresarial            
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la administración de aplicaciones empresariales.
|   **Nombre del servicio**    |   EntAppSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Protocolo de autenticación extensible           
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de protocolo de autenticación Extensible (EAP) proporciona autenticación de red en escenarios como 802.1X con cable e inalámbricos, VPN y Network Access Protection (NAP).  EAP también proporciona interfaces de programación de aplicaciones (API) que se usan los clientes de acceso de red, incluidos los clientes inalámbricos y VPN, durante el proceso de autenticación.  Si deshabilita este servicio, se impide que este equipo tengan acceso a redes que requieren autenticación de EAP.
|   **Nombre del servicio**    |   EapHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host de proveedor de detección de funciones         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio FDPHOST hospeda los proveedores de detección de red de detección de función (FD). Estos proveedores FD proporcionan servicios de detección de red para el protocolo Simple de detección de servicios (SSDP) y servicios Web - Protocolo Discovery (WS-D). Detener o deshabilitar el servicio FDPHOST deshabilitará la detección de redes para estos protocolos al usar FD. Cuando este servicio no está disponible, servicios de red mediante FD y uso de estos protocolos de detección no se pueden encontrar los dispositivos de red o de recursos.
|   **Nombre del servicio**    |   fdPHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Publicación de recurso de detección de función      
| | |           
|---|---|       
|   **Descripción del servicio** |   Publica este equipo y los recursos conectados a este equipo, por lo que se pueden detectar a través de la red.  Si se detiene este servicio, ya no se va a publicar los recursos de red y no se detectarán otros equipos de la red.
|   **Nombre del servicio**    |   FDResPub
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Servicio de geolocalización          
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio supervisa la ubicación actual del sistema y administra las geovallas (una ubicación geográfica con eventos asociados).  Si desactiva este servicio, las aplicaciones no se pueden usar o recibir notificaciones de geolocalización o las geovallas.
|   **Nombre del servicio**    |   lfsvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Deshabilitación de las aplicaciones de saltos que se basan en el servicio; Aceptar deshabilitar si no depender de aplicaciones
|||         
            
<br />          

##  <a name="group-policy-client"></a>Cliente de directiva de grupo     
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio es responsable de aplicar la configuración establecida por los administradores del equipo y los usuarios a través del componente de directiva de grupo. Si el servicio está deshabilitado, no se aplicará la configuración y las aplicaciones y componentes no se pueden administrables a través de la directiva de grupo. Los componentes o aplicaciones que dependen del componente de directiva de grupo no sea funcionales si el servicio está deshabilitado.
|   **Nombre del servicio**    |   gpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Servicio de dispositivos de interfaz humana           
| | |           
|---|---|       
|   **Descripción del servicio** |   Activa y mantiene el uso de botones de acceso frecuente de teclados, controles remotos y otros dispositivos multimedia. Se recomienda que mantenga este servicio se está ejecutando.
|   **Nombre del servicio**    |   hidserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Servicio de Host HV     
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para el hipervisor de Hyper-V proporcionar los contadores de rendimiento por partición para el sistema operativo host.
|   **Nombre del servicio**    |   HvHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Potenciadores para las máquinas virtuales invitadas. No se usa hoy en día, excepto para explícitamente rellena las máquinas virtuales, pero se usará en la protección de aplicaciones
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Servicio de intercambio de datos de Hyper-V        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona un mecanismo para intercambiar datos entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre del servicio**    |   vmickvpexchange
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interfaz de servicio de invitado de Hyper-V          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona una interfaz para el host de Hyper-V interactuar con servicios específicos que se ejecutan dentro de la máquina virtual.
|   **Nombre del servicio**    |   vmicguestinterface
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Servicio de cierre de invitado de Hyper-V           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un mecanismo para apagar el sistema operativo de esta máquina virtual desde las interfaces de administración en el equipo físico.
|   **Nombre del servicio**    |   vmicshutdown
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Servicio de latido de Hyper-V            
| | |           
|---|---|           
|   **Descripción del servicio** |   Supervisa el estado de esta máquina virtual mediante la notificación de un latido a intervalos regulares. Este servicio le ayuda a identificar las máquinas virtuales en ejecución que ha dejado de responder.
|   **Nombre del servicio**    |   vmicheartbeat
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Servicio directo de PowerShell de Hyper-V            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un mecanismo para administrar la máquina virtual con PowerShell a través de la sesión de máquina virtual sin una red virtual.
|   **Nombre del servicio**    |   vmicvmsession
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servicio de virtualización de escritorio remoto de Hyper-V            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona una plataforma para la comunicación entre la máquina virtual y el sistema operativo que se ejecuta en el equipo físico.
|   **Nombre del servicio**    |   vmicrdv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Servicio de sincronización de hora de Hyper-V         
| | |           
|---|---|       
|   **Descripción del servicio** |   Sincroniza la hora del sistema de esta máquina virtual con la hora del sistema del equipo físico.
|   **Nombre del servicio**    |   vmictimesync
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de instantáneas de volumen de Hyper-V         
| | |           
|---|---|           
|   **Descripción del servicio** |   Coordina las comunicaciones que son necesarios para usar el servicio de instantáneas de volumen para realizar una copia de seguridad de aplicaciones y datos en esta máquina virtual desde el sistema operativo en el equipo físico.
|   **Nombre del servicio**    |   vmicvss
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Consulte HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>Módulos de creación de claves de IPsec para IKE y AuthIP          
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio IKEEXT hospeda el intercambio de claves de Internet (IKE) y protocolo de Internet autenticado (AuthIP) módulos de incrustación. Estos módulos de generación de claves se utilizan para la autenticación y el intercambio de claves de seguridad de protocolo Internet (IPsec). Detener o deshabilitar el servicio IKEEXT deshabilitará el intercambio de claves IKE y AuthIP con equipos del mismo nivel. IPsec normalmente está configurado para usar IKE o AuthIP; por lo tanto, detener o deshabilitar el servicio IKEEXT puede dar lugar a un error de IPsec y podría poner en peligro la seguridad del sistema. Se recomienda encarecidamente que tendrá que ejecutar el servicio IKEEXT.
|   **Nombre del servicio**    |   IKEEXT
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Detección de servicios interactivos           
| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita la notificación de usuario del usuario de entrada para los servicios interactivos, lo que permite el acceso a los cuadros de diálogo creados por servicios interactivos cuando aparecen. Si se detiene este servicio, las notificaciones de cuadros de diálogo nuevo servicio interactivo dejarán de funcionar y no habrá acceso a los cuadros de diálogo servicio interactivo. Si se deshabilita este servicio, las notificaciones de y acceso a cuadros de diálogo nuevo servicio interactivo dejará de funcionar.
|   **Nombre del servicio**    |   UI0Detect
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Conexión compartida a Internet (ICS)            
| | |           
|---|---|           
|   **Descripción del servicio** |   Proporciona la traducción de direcciones de red, direccionamiento de servicios de prevención de intrusiones o de resolución de nombre de una red doméstica o de pequeña oficina.
|   **Nombre del servicio**    |   SharedAccess
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Se requiere para los clientes que se utiliza como puntos de conexión Wi-Fi y también en ambos extremos de la proyección de Miracast. ICS puede bloquearse con la configuración de GPO, "Prohibir el uso de la conexión compartida a Internet en su red de dominio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Aplicación auxiliar IP            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona conectividad de túnel mediante tecnologías de transición IPv6 (6to4, Teredo, ISATAP y puerto de Proxy) e IP-HTTPS. Si se detiene este servicio, el equipo no tendrán las ventajas de una conectividad mejorada que ofrecen estas tecnologías.
|   **Nombre del servicio**    |   iphlpsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente de directiva IPsec      
| | |           
|---|---|       
|   **Descripción del servicio** |   Protocolo de Internet (IPsec) de seguridad admite la autenticación de nivel de red del mismo nivel, autenticación del origen de datos, integridad de los datos, confidencialidad (cifrado) de datos y protección contra la reproducción.  Este servicio aplica directivas de IPsec creadas mediante el complemento Directivas de seguridad IP o la herramienta de línea de comandos "netsh ipsec".  Si se detiene este servicio, puede experimentar problemas de conectividad de red si la directiva requiere que las conexiones usen IPsec.  Además, administración remota de Firewall de Windows no está disponible cuando se detiene este servicio.
|   **Nombre del servicio**    |   PolicyAgent
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Servicio de servidor Proxy KDC (KPS)      
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de servidor Proxy KDC ejecuta en los servidores perimetrales al proxy Kerberos mensajes de protocolo para los controladores de dominio en la red corporativa.
|   **Nombre del servicio**    |   KPSSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KTMRM para DTC (Coordinador de transacciones distribuidas)            
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina las transacciones entre el Coordinador de transacciones distribuidas (MSDTC) y el Administrador de transacciones de Kernel (KTM). Si no es necesario, se recomienda que este servicio siguen detenido. Si es necesario, MSDTC y KTM iniciará este servicio automáticamente. Si se deshabilita este servicio, se producirá un error en cualquier transacción de MSDTC interactuar con un administrador de recursos del núcleo y podrán iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   KtmRm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Asignador de detección de topología de nivel de vínculo        
| | |       
|---|---|       
|   **Descripción del servicio** |   Crea un mapa de red, que consta de PC e información de topología (conectividad) de dispositivo y los metadatos que describen cada PC y dispositivos.  Si se deshabilita este servicio, el mapa de red no funcionará correctamente.
|   **Nombre del servicio**    |   lltdsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Aceptar deshabilitar si ninguna dependencia en el mapa de red
|||         
            
<br />

## <a name="local-session-manager"></a>Administrador de sesión local                    
| | |                   
|---|---|   
|   **Descripción del servicio** |   Servicio de Windows Core que administra las sesiones de usuario local. Detener o deshabilitar este servicio dará como resultado inestabilidad del sistema.    
|   **Nombre del servicio**    |   LSM |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Automático   |
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Concentrador de diagnósticos Microsoft (R) recolector estándar         
| | |           
|---|---|           
|   **Descripción del servicio** |   Servicio de Windows Core que administra las sesiones de usuario local. Detener o deshabilitar este servicio dará como resultado inestabilidad del sistema.
|   **Nombre del servicio**    |   LSM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Ayudante para el inicio de sesión de cuenta de Microsoft          
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite el inicio de sesión de usuario a través de servicios de identidad de cuenta de Microsoft. Si se detiene este servicio, los usuarios no podrán iniciar sesión en el equipo con su cuenta de Microsoft.
|   **Nombre del servicio**    |   wlidsvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Microsoft Accounts son N/D en Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft App-V Client      
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra los usuarios de App-V y las aplicaciones virtuales
|   **Nombre del servicio**    |   AppVClient
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Servicio del iniciador iSCSI de Microsoft            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las sesiones de Internet SCSI (iSCSI) desde este equipo a dispositivos de destino iSCSI remoto. Si se detiene este servicio, este equipo no podrá iniciar sesión o tener acceso a los destinos iSCSI. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   MSiSCSI
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Nuestros datos de diagnóstico indican que se usa en el cliente, así como el servidor. Ningún beneficio para deshabilitarlo.
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona aislamiento de procesos para las claves criptográficas utilizadas para autenticar a los proveedores de identidad asociada de un usuario. Si se deshabilita este servicio, todos los usos y la administración de estas claves no estará disponibles, que incluye el inicio de sesión de máquina y el inicio de sesión único en para aplicaciones y sitios Web. Este servicio se inicia y detiene automáticamente. Se recomienda que no vuelva a configurar este servicio.
|   **Nombre del servicio**    |   NgcSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Es necesario para los inicios de sesión PIN/Hello, que no se admiten en el servidor
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contenedor de Microsoft Passport         
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las claves de identidad de usuario local utilizadas para autenticar el usuario a los proveedores de identidades, así como las tarjetas inteligentes virtuales de TPM. Si se deshabilita este servicio, las claves de identidad de usuario local y las tarjetas inteligentes virtuales de TPM no será accesibles. Se recomienda que no vuelva a configurar este servicio.
|   **Nombre del servicio**    |   NgcCtnrSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Proveedor de instantáneas de Software de Microsoft          
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las instantáneas de volumen basado en software tomadas por el servicio de instantáneas de volumen. Si se detiene este servicio, no puede administrar instantáneas de volumen basado en software. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   swprv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>SMP de espacios de almacenamiento de Microsoft         
| | |           
|---|---|       
|   **Descripción del servicio** |   Servicio de host para el proveedor de administración de espacios de almacenamiento de Microsoft. Si este servicio está detenido o deshabilitado, no puede administrar espacios de almacenamiento.
|   **Nombre del servicio**    |   smphost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Las API de administración de almacenamiento producirá un error sin este servicio. Por ejemplo: "Get-WmiObject-clase MSFT_Disk - Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Servicio de uso compartido de puertos Net.Tcp         
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la capacidad de compartir puertos TCP a través del protocolo de net.tcp.
|   **Nombre del servicio**    |   NetTcpPortSharing
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Netlogon         
| | |           
|---|---|           
|   **Descripción del servicio** |   Mantiene un canal seguro entre el equipo y el controlador de dominio para autenticar usuarios y servicios. Si se detiene este servicio, el equipo no puede autenticar a los usuarios y servicios y el controlador de dominio no pueden registrar los registros DNS. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   Netlogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Agente de conexión de red            
| | |           
|---|---|       
|   **Descripción del servicio** |   Conexiones de agentes que permitir que las aplicaciones de Microsoft Store recibir notificaciones de internet.
|   **Nombre del servicio**    |   NcbService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Conexiones de red         
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra los objetos en la carpeta Conexiones de red y acceso telefónico, en el que puede ver la red de área local y las conexiones remotas.
|   **Nombre del servicio**    |   Netman
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Asistente de conectividad de red      
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona notificación de estado de DirectAccess para los componentes de interfaz de usuario
|   **Nombre del servicio**    |   NcaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Servicio de lista de red        
| | |           
|---|---|   
|   **Descripción del servicio** |   Identifica las redes a la que el equipo se ha conectado, recopila y almacena las propiedades de estas redes y notifica a las aplicaciones cuando cambian estas propiedades.
|   **Nombre del servicio**    |   netprofm
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Reconocimiento de ubicación de red           
| | |           
|---|---|       
|   **Descripción del servicio** |   Recopila y almacena la información de configuración de la red y notifica a los programas cuando se modifica esta información. Si se detiene este servicio, la información de configuración podría no estar disponible. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   NlaSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Servicio de configuración de red       
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio de configuración de red administra la instalación de controladores de red y permite la configuración de red de bajo nivel.  Si se detiene este servicio, se pueden cancelar las instalaciones de controlador que están en curso.
|   **Nombre del servicio**    |   NetSetupSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Servicio Network Store Interface      
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio ofrece notificaciones de la red (por ejemplo, interfaz Adición/eliminación etcetera) a los clientes de modo de usuario. Si detiene este servicio hará que la pérdida de conectividad de red. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de este servicio.
|   **Nombre del servicio**    |   nsi
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="offline-files"></a>Archivos sin conexión            
| | |           
|---|---|       
|   **Descripción del servicio** |   Los archivos sin conexión de servicio realiza actividades de mantenimiento en la caché de archivos sin conexión, responde a eventos de inicio de sesión y cierre de sesión de usuario, implementa los aspectos internos de la API pública, y envía eventos a los interesantes que está interesado en los archivos sin conexión las actividades y cambios de estado de la memoria caché.
|   **Nombre del servicio**    |   CscService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Optimizar unidades          
| | |           
|---|---|   
|   **Descripción del servicio** |   Ayuda a que el equipo ejecute de forma más eficaz mediante la optimización de archivos en unidades de almacenamiento.
|   **Nombre del servicio**    |   defragsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL de contador de rendimiento         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite a los usuarios remotos y los procesos de 64 bits consultar los contadores de rendimiento proporcionados por archivos DLL de 32 bits. Si se detiene este servicio, solo los usuarios locales y los procesos de 32 bits podrá consultar los contadores de rendimiento proporcionados por archivos DLL de 32 bits.
|   **Nombre del servicio**    |   PerfHost
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Alertas y registros de rendimiento            
| | |           
|---|---|   
|   **Descripción del servicio** |   Los registros de rendimiento y alertas recopila datos de rendimiento de los equipos locales o remotos según los parámetros de programación preconfigurada, a continuación, escribe los datos en un registro o desencadena una alerta. Si se detiene este servicio, no se recopilará información sobre el rendimiento. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   pla
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Servicio de teléfono       
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el estado de telefonía en el dispositivo
|   **Nombre del servicio**    |   PhoneSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Utilizado por aplicaciones modernas de VoIP
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug and Play       
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a un equipo reconocer y adaptarse a los cambios de hardware con poca o ninguna intervención del usuario. Detener o deshabilitar este servicio dará como resultado inestabilidad del sistema.
|   **Nombre del servicio**    |   PlugPlay
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Servicio de enumerador de dispositivo portátil           
| | |           
|---|---|       
|   **Descripción del servicio** |   Aplica la directiva de grupo para los dispositivos de almacenamiento extraíbles. Permite que las aplicaciones, como Windows Media Player y Asistente para importación de imágenes para transferir y sincronizar el contenido con dispositivos de almacenamiento extraíbles.
|   **Nombre del servicio**    |   WPDBusEnum
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="power"></a>Alimentación            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra la directiva de energía y entrega de notificaciones de directiva de energía.
|   **Nombre del servicio**    |   Alimentación
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Print Spooler            
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio se pone en cola los trabajos de impresión y administra la interacción con la impresora.  Si desactiva este servicio, no podrá imprimir o ver las impresoras.
|   **Nombre del servicio**    |   Cola de impresión
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar si no un servidor de impresión o un controlador de dominio
|   **Comentarios**    |   En un controlador de dominio, la instalación del rol de controlador de dominio agrega un subproceso para el servicio de cola que es responsable de realizar la eliminación de impresión: quitar los objetos de la cola de impresión obsoletos de Active Directory.  Si el servicio de cola no se está ejecutando en al menos un controlador de dominio en cada sitio, el anuncio no tiene ningún medio para quitar colas antiguas que ya no existen. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Las notificaciones y las extensiones de impresora        
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio abre los cuadros de diálogo personalizados de impresora y controla las notificaciones desde un servidor de impresión remoto o una impresora. Si desactiva este servicio, no podrá ver las extensiones de la impresora o notificaciones.
|   **Nombre del servicio**    |   PrintNotify
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar si no es un servidor de impresión
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Informes de problemas y soporte técnico de Panel de Control de soluciones     
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio proporciona soporte técnico para ver, enviar y la eliminación de informes de problemas de nivel de sistema para el panel de control informes de problemas y soluciones.
|   **Nombre del servicio**    |   wercplsupport
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Servicio de compatibilidad de programas     
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio proporciona compatibilidad con el Asistente para compatibilidad de programas (PCA).  PCA supervisa los programas instalado y ejecutado por el usuario y detecta problemas de compatibilidad conocidos. Si se detiene este servicio, el PCA no funcionará correctamente.
|   **Nombre del servicio**    |   PcaSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Windows Audio Video Experience (qWAVE)      
| | |           
|---|---|   
|   **Descripción del servicio** |   Experiencia de vídeo de calidad de Audio de Windows (qWave) es una plataforma de red para Audio vídeo (AV), transmisión de aplicaciones en redes domésticas IP. qWave mejora el rendimiento y confiabilidad mediante la red calidad de servicio (QoS) para aplicaciones de AV de transmisión de AV. Proporciona un mecanismo para el control de admisión, ejecutar la supervisión en tiempo y cumplimiento, comentarios de la aplicación y la priorización del tráfico.
|   **Nombre del servicio**    |   QWAVE
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servicio de calidad de servicio de cliente
|||         
            
<br />          

##      <a name="radio-management-service"></a>Servicio de administración de radio        
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de modo de avión y administración de radio
|   **Nombre del servicio**    |   RmSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Administrador de conexión automática de acceso remoto            
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea una conexión a una red remota siempre que un programa hace referencia a un nombre DNS o NetBIOS o la dirección remota.
|   **Nombre del servicio**    |   RasAuto
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Administrador de conexiones de acceso remoto         
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra las conexiones de red privada virtual y de acceso telefónico (VPN) desde este equipo a Internet o a otras redes remotas. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   RasMan
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configuración de Escritorio remoto         
| | |           
|---|---|   
|   **Descripción del servicio** |   Servicio de configuración de escritorio remoto (RDC) es responsable de la configuración relacionada con todos los servicios de escritorio remoto y escritorio remoto y actividades de mantenimiento de la sesión que requieren el contexto del sistema. Estos incluyen carpetas temporales por sesión, los temas de escritorio remoto y los certificados de escritorio remoto.
|   **Nombre del servicio**    |   SessionEnv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Servicios de Escritorio remoto          
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los usuarios conectarse de forma interactiva a un equipo remoto. Escritorio remoto y servidor de Host de sesión de escritorio remoto dependen de este servicio.  Para evitar el uso remoto de este equipo, desactive las casillas de verificación en la ficha acceso remoto del elemento de panel de control de las propiedades del sistema.
|   **Nombre del servicio**    |   TermService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector de puerto de modo de usuario de servicios de escritorio remoto        
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite la redirección de impresoras unidades o los puertos para las conexiones RDP
|   **Nombre del servicio**    |   UmRdpService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Admite las redirecciones del servidor de la conexión.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>Llamada a procedimiento remoto (RPC)          
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio RPCSS es el Administrador de Control de servicio para servidores COM y DCOM. Realiza las solicitudes de activaciones de objetos, las soluciones exportador de objeto y colección de elementos no utilizados distribuida para los servidores COM y DCOM. Si este servicio está detenido o deshabilitado, los programas que utilizan COM o DCOM no funcionará correctamente. Se recomienda encarecidamente que tiene la ejecución del servicio RPCSS.
|   **Nombre del servicio**    |   RpcSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localizador de procedimiento remoto (RPC) de la llamada             
| | |               
|---|---|   
|   **Descripción del servicio** |   En Windows 2003 y versiones anteriores de Windows, el servicio de ubicación de la llamada a procedimiento remoto (RPC) administra la base de datos del servicio de nombres RPC. En Windows Vista y versiones posteriores de Windows, este servicio no proporciona ninguna funcionalidad y está presente para compatibilidad de aplicaciones.   |
|   **Nombre del servicio**    |   RpcLocator  |
|   **Instalación**    |   Solo con la experiencia de escritorio    |
|   **StartType**   |   Manual  |
|   **Recomendación**  | No hay instrucciones   |
|   **Comentarios**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro remoto          
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los usuarios remotos modificar la configuración del registro en este equipo. Si se detiene este servicio, se puede modificar el registro solo por usuarios en este equipo. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   RemoteRegistry
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Conjunto resultante de proveedor de directivas            
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un servicio de red que procesa las solicitudes para simular la aplicación de la configuración de directiva de grupo para un usuario de destino o el equipo en diferentes situaciones y calcula los valores del conjunto resultante de directivas.
|   **Nombre del servicio**    |   RSoPProv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |No hay instrucciones    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Enrutamiento y acceso remoto            
| | |           
|---|---|   
|   **Descripción del servicio** |   Ofrece servicios de enrutamiento a las empresas en entornos de red de área extensa y de área local.
|   **Nombre del servicio**    |   RemoteAccess
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   Ya se ha deshabilitado
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Asignador de extremos de RPC          
| | |           
|---|---|   
|   **Descripción del servicio** |   Resuelve los identificadores de las interfaces RPC a puntos de conexión de transporte. Si este servicio está detenido o deshabilitado, los programas que utilizan servicios de llamada a procedimiento remoto (RPC) no funcionará correctamente.
|   **Nombre del servicio**    |   RpcEptMapper
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Inicio de sesión secundario     
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite iniciar procesos con credenciales alternativas. Si se detiene este servicio, este tipo de acceso de inicio de sesión estará disponible. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   seclogon
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Servicio de protocolo de túnel de sockets seguros            
| | |               
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad para la Secure Socket de protocolo de túnel (SSTP) para conectarse a equipos remotos a través de VPN. Si se deshabilita este servicio, los usuarios no podrán usar SSTP para acceder a servidores remotos.    |
|   **Nombre del servicio**    |   SstpSvc |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   No deshabilite  |
|   **Comentarios**    |   Deshabilitar los saltos de RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Administrador de cuentas de seguridad            
| | |           
|---|---|       
|   **Descripción del servicio** |   El inicio de este servicio indica que el Administrador de cuentas de seguridad (SAM) está listo para aceptar las solicitudes a otros servicios.  Si se deshabilita este servicio impedirá que otros servicios en el sistema que se le notifique cuando esté listo SAM, lo que a su vez puede producir esos servicios no se pueda iniciar correctamente. No se debe deshabilitar este servicio.
|   **Nombre del servicio**    |   SamSs
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No deshabilite
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Servicio de datos del sensor  
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona datos desde una variedad de sensores
|   **Nombre del servicio**    |   SensorDataService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Servicio de supervisión del sensor            
| | |           
|---|---|       
|   **Descripción del servicio** |   Supervisa varios sensores con el fin de exponer los datos y se adaptan al estado de usuario y del sistema.  Si este servicio está detenido o deshabilitado, el brillo de la pantalla no se pueden adaptar a las condiciones de iluminación. Si detiene este servicio puede afectar a otras funciones del sistema y también las características.
|   **Nombre del servicio**    |   SensrSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Servicio del sensor           
| | |           
|---|---|       
|   **Descripción del servicio** |   Un servicio para los sensores que administra la funcionalidad de diferentes de los sensores. Administra la orientación de dispositivo Simple (SDO) y el historial para los sensores. Carga el sensor SDO que informa de los cambios de orientación del dispositivo.  Si este servicio está detenido o deshabilitado, no se cargará el sensor SDO y por lo que no se producirá la rotación automática. También se detendrá la colección de historiales de sensores.
|   **Nombre del servicio**    |   SensorService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="server"></a>Servidor           
| | |           
|---|---|   
|   **Descripción del servicio** |   Es compatible con archivos, impresión y canalización con nombre comparten a través de la red para este equipo. Si se detiene este servicio, estas funciones no estará disponibles. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   LanmanServer
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Es necesario para la administración remota, IPC$, uso compartido de archivos SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Detección de Hardware shell             
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona notificaciones de eventos de hardware de reproducción automática.
|   **Nombre del servicio**    |   ShellHWDetection
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Tarjeta inteligente           
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra el acceso a las tarjetas inteligentes leídos por este equipo. Si se detiene este servicio, este equipo no se puede leer las tarjetas inteligentes. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   SCardSvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Servicio de enumeración de dispositivos de tarjeta inteligente                    
| | |               
|---|---|       
|   **Descripción del servicio** |   Crea los nodos de dispositivo de software para todos los lectores de tarjetas inteligentes puede tener acceso a una sesión determinada. Si se deshabilita este servicio, WinRT APIs no podrán enumerar los lectores de tarjetas inteligentes.   |
|   **Nombre del servicio**    |   ScDeviceEnum    |
|   **Instalación**    |   Siempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendación**  |   Aceptar para deshabilitar   |
|   **Comentarios**    |   Es necesario casi exclusivamente para las aplicaciones de WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Directiva de eliminación de tarjeta inteligente        
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que el sistema deberá estar configurado para bloquear el escritorio del usuario tras la eliminación de la tarjeta inteligente.
|   **Nombre del servicio**    |   SCPolicySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Captura de SNMP            
| | |           
|---|---|       
|   **Descripción del servicio** |   Recibe mensajes de captura generados por los agentes de Protocolo Simple de administración de redes (SNMP) local o remoto y reenvía los mensajes a los programas de administración SNMP que se ejecutan en este equipo. Si se detiene este servicio, los programas basados en SNMP en este equipo no recibirá mensajes de captura SNMP. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   SNMPTRAP
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Protección de software             
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite la descarga, instalación y cumplimiento de licencias digitales para Windows y Windows aplicaciones. Si el servicio está deshabilitado, el sistema operativo y las aplicaciones con licencia pueden ejecutar en un modo de notificación. Se recomienda que deshabilite el servicio de protección de Software.
|   **Nombre del servicio**    |   sppsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Ayudante de la consola de administración especial        
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los administradores tener acceso remoto a un símbolo del sistema mediante los servicios de administración de emergencia.
|   **Nombre del servicio**    |   sacsvr
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Comprobador puntual            
| | |           
|---|---|   
|   **Descripción del servicio** |   Comprueba posibles daños del sistema de archivos.
|   **Nombre del servicio**    |   svsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Descubrimiento SSDP           
| | |           
|---|---|   
|   **Descripción del servicio** |   Detecta los dispositivos de red y los servicios que utilizan el protocolo de descubrimiento SSDP, como los dispositivos UPnP. También anuncia dispositivos SSDP y servicios que se ejecutan en el equipo local. Si se detiene este servicio, no se detectarán los dispositivos basados en SSDP. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   SSDPSRV
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Servicio de estado del repositorio         
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad con la infraestructura necesaria para el modelo de aplicación.
|   **Nombre del servicio**    |   StateRepository
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Eventos de adquisición de imagen fija
| | |           
|---|---|   
|   **Descripción del servicio** |   Inicia las aplicaciones asociadas a eventos de adquisición de imagen todavía.
|   **Nombre del servicio**    |   WiaRpc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Servicio de almacenamiento          
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona servicios de habilitación para la configuración de almacenamiento y la expansión de almacenamiento externo
|   **Nombre del servicio**    |   StorSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Administración de capas de almacenamiento        
| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza la colocación de datos en niveles de almacenamiento de todos los espacios de almacenamiento en capas en el sistema.
|   **Nombre del servicio**    |   TieringEngineService
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Descripción del servicio** |   Mantiene y mejora el rendimiento del sistema con el tiempo.
|   **Nombre del servicio**    |   SysMain
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host de sincronización            
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio sincroniza el correo electrónico, contactos, calendario y varios otros datos de usuario. Correo electrónico y otras aplicaciones que dependen de esta funcionalidad no funcionará correctamente cuando no se está ejecutando este servicio.
|   **Nombre del servicio**    |   OneSyncSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Servicio de notificación de eventos del sistema            
| | |           
|---|---|       
|   **Descripción del servicio** |   Supervisa los eventos del sistema y notifica a los suscriptores al sistema de eventos COM + de estos eventos.
|   **Nombre del servicio**    |   SENS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Agente de eventos del sistema             
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina la ejecución del trabajo en segundo plano para aplicaciones de WinRT. Si este servicio está detenido o deshabilitado, trabajo en segundo plano no es posible que se desencadene.
|   **Nombre del servicio**    |   SystemEventsBroker
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   A pesar del hecho de que su descripción implica es únicamente para las aplicaciones de WinRT, que es necesario para el programador de tareas, servicio de infraestructura de agente y otros componentes internos.
|||         
            
<br />          

## <a name="task-scheduler"></a>Programador de tareas           
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite que un usuario configurar y programar tareas automatizadas en este equipo. El servicio también hospeda varias tareas críticas del sistema de Windows. Si este servicio está detenido o deshabilitado, estas tareas no se ejecutará en sus programada veces. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   Programa
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>Aplicación auxiliar de NetBIOS de TCP/IP            
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad con NetBIOS a través del servicio TCP/IP (NetBT) y resolución de nombres NetBIOS para clientes de la red, por lo tanto, permitiendo a los usuarios compartir archivos, imprimir e iniciar sesión en la red. Si se detiene este servicio, estas funciones podrían no estar disponibles. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   lmhosts
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telefonía           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad con la API de telefonía (TAPI) para programas que controlan dispositivos de telefonía en el equipo local y, a través de la LAN, en servidores que también estén ejecutando el servicio.
|   **Nombre del servicio**    |   TapiSrv
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Deshabilitar los saltos de RRAS
|||         
            
<br />          

## <a name="themes"></a>Temas           
| | |           
|---|---|
|   **Descripción del servicio** |   Proporciona administración de temas de experiencia de usuario.
|   **Nombre del servicio**    |   Temas
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   No se puede establecer los temas de accesibilidad cuando se deshabilita este servicio
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Servidor de mosaicos de modelo de datos           
| | |           
|---|---|   
|   **Descripción del servicio** |   Los iconos del servidor de actualizaciones de icono.
|   **Nombre del servicio**    |   tiledatamodelsvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Iniciar los saltos de menú si se deshabilita este servicio
|||         
            
<br />          

##  <a name="time-broker"></a>Broker de tiempo     
| | |           
|---|---|       
|   **Descripción del servicio** |   Coordina la ejecución del trabajo en segundo plano para aplicaciones de WinRT. Si este servicio está detenido o deshabilitado, trabajo en segundo plano no es posible que se desencadene.
|   **Nombre del servicio**    |   TimeBrokerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   A pesar del hecho de que su descripción implica es únicamente para las aplicaciones de WinRT, que es necesario para el programador de tareas, servicio de infraestructura de agente y otros componentes internos.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Teclado táctil y el servicio del Panel de escritura a mano         
| | |           
|---|---|   
|   **Descripción del servicio** |   Habilita la funcionalidad de lápiz y entrada manuscrita teclado táctil y el Panel de escritura a mano
|   **Nombre del servicio**    |   TabletInputService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Servicio de actualización de Orchestrator para Windows Update           
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra las actualizaciones de Windows. Si se detiene, los dispositivos no pueda descargar e instalar las actualizaciones más recientes.
|   **Nombre del servicio**    |   UsoSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Descripción del servicio que faltaba en v1607; Actualización de Windows (incluido WSUS) depende de este servicio.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Host de dispositivo UPnP         
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite a los dispositivos UPnP hospedarse en este equipo. Si se detiene este servicio, los dispositivos UPnP hospedados dejará de funcionar y no se puede agregar ningún dispositivo hospedado adicional. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   UPnPHost
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Servicio de registro de acceso de usuarios          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio registra las solicitudes de acceso de cliente único, en forma de direcciones IP y los nombres de usuario de roles en el servidor local y productos instalados. Esta información se puede consultar, a través de Powershell, los administradores que necesitan cuantificar la demanda del cliente de software de servidor para la administración de licencias de acceso de cliente (CAL) sin conexión. Si el servicio está deshabilitado, las solicitudes de cliente no se registrarán y no podrá recuperarse mediante consultas de Powershell. Deteniendo el servicio no afectará a la consulta de datos históricos (consulte la documentación para conocer los pasos eliminar los datos históricos correspondiente). El administrador del sistema local debe consultar los términos de licencia de Windows Server, sonará sus, para determinar el número de CAL que son necesarios para la licencia adecuada; el software de servidor utilizar el servicio UAL y datos no modifica esta obligación.
|   **Nombre del servicio**    |   UALSVC
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Acceso a datos de usuario        
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona acceso a datos de usuario estructurado, incluida la información de contacto, calendarios, los mensajes y otro contenido. Si detiene o deshabilita este servicio, las aplicaciones que usan estos datos podrían no funcionar correctamente.
|   **Nombre del servicio**    |   UserDataSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="user-data-storage"></a>Almacenamiento de datos de usuario            
| | |           
|---|---|       
|   **Descripción del servicio** |   Controla el almacenamiento de datos de usuario estructurado, incluida la información de contacto, calendarios, los mensajes y otro contenido. Si detiene o deshabilita este servicio, las aplicaciones que usan estos datos podrían no funcionar correctamente.
|   **Nombre del servicio**    |   UnistoreSvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Servicio de virtualización de experiencia de usuario           
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona compatibilidad con aplicaciones y configuraciones del sistema operativo móvil
|   **Nombre del servicio**    |   UevAgentService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Administrador de usuarios        
| | |           
|---|---|   
|   **Descripción del servicio** |   El Administrador de usuarios proporciona los componentes de tiempo de ejecución necesarios para la interacción de varios usuario.  Si se detiene este servicio, es posible que algunas aplicaciones no funcionan correctamente.
|   **Nombre del servicio**    |   UserManager
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Servicio de perfil de usuario         
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio es responsable de la carga y descarga de perfiles de usuario. Si este servicio está detenido o deshabilitado, los usuarios ya no podrán iniciar sesión correctamente, o cierre la sesión, aplicaciones podrían tener problemas para obtener datos de los usuarios y los componentes registrados para recibir notificaciones de eventos de perfil no recibirán.
|   **Nombre del servicio**    |   ProfSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtual             
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de administración de discos, volúmenes, los sistemas de archivos y las matrices de almacenamiento.
|   **Nombre del servicio**    |   vds
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No hay instrucciones
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Instantáneas de volumen           
| | |           
|---|---|   
|   **Descripción del servicio** |   Administra e implementa instantáneas de volumen utilizadas para la copia de seguridad y otros fines. Si se detiene este servicio, no estará disponibles para la copia de seguridad de instantáneas y la copia de seguridad puede producir un error. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   VSS
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No hay instrucciones
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descripción del servicio** |   Objetos de hosts usados por los clientes de wallet
|   **Nombre del servicio**    |   WalletService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Audio de Windows            
| | |           
|---|---|       
|   **Descripción del servicio** |   Administra el audio para programas basados en Windows.  Si se detiene este servicio, los dispositivos de audio y efectos no funcionará correctamente.  Si se deshabilita este servicio, se producirá un error en todos los servicios que dependan explícitamente de él iniciar
|   **Nombre del servicio**    |   Audiosrv
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Generador de extremo de Audio de Windows           
| | |           
|---|---|
|   **Descripción del servicio** |   Administra dispositivos de audio para el servicio Audio de Windows.  Si se detiene este servicio, los dispositivos de audio y efectos no funcionará correctamente.  Si se deshabilita este servicio, se producirá un error en todos los servicios que dependan explícitamente de él iniciar
|   **Nombre del servicio**    |   AudioEndpointBuilder
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Servicio biométrico de Windows            
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio biométrico de Windows ofrece la capacidad de capturar, comparar, manipular y almacenar datos biométricos sin acceder directamente a cualquier hardware biométrico o samples de las aplicaciones cliente. El servicio se hospeda en un proceso SVCHOST privilegiado.
|   **Nombre del servicio**    |   WbioSrvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Servidor de marco de cámara de Windows         
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite que varios clientes accedan a fotogramas de vídeo desde dispositivos de cámara.
|   **Nombre del servicio**    |   FrameServer
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Administrador de conexiones de Windows           
| | |           
|---|---|   
|   **Descripción del servicio** |   Hace automática para conectar o desconectar decisiones según las opciones de conectividad de red disponibles actualmente para el equipo y permite la administración de conectividad de red según la configuración de directiva de grupo.
|   **Nombre del servicio**    |   Wcmsvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Servicio de inspección de red de Windows Defender          
| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a protegerse de intentos de intrusión destinadas a vulnerabilidades conocidas y recién detectadas en los protocolos de red
|   **Nombre del servicio**    |   WdNisSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones    
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Servicio de Windows Defender         
| | |           
|---|---|       
|   **Descripción del servicio** |   Ayuda a protege a los usuarios contra malware y otro software potencialmente no deseado
|   **Nombre del servicio**    |   WinDefend
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation: marco de controlador en modo de usuario           
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y administra los procesos de controlador de modo de usuario. No se puede detener este servicio.
|   **Nombre del servicio**    |   wudfsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Servicio de Host del proveedor de cifrado de Windows     
| | |           
|---|---|   
|   **Descripción del servicio** |   Cifrado de los agentes de servicio de Host de proveedor de cifrado de Windows relacionadas con funcionalidades de proveedores de cifrado de terceros para los procesos que necesitan para evaluar y aplicar directivas de EAS. Si se detiene pondrá en peligro las comprobaciones de cumplimiento EAS que se han establecido las cuentas de correo conectado
|   **Nombre del servicio**    |   WEPHOSTSVC
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Servicio Informe de errores de Windows          
| | |           
|---|---|       
|   **Descripción del servicio** |   Permite errores cuando los programas dejan de funcionar o de responder y para entregar las soluciones existentes. También permite que los registros que se generará para el diagnóstico y reparación de servicios. Si se detiene este servicio, informes de errores podrían no funcionar correctamente y no se muestren los resultados de servicios de diagnóstico y reparación.
|   **Nombre del servicio**    |   WerSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Recopila y envía datos de bloqueo o utilizados por Microsoft y terceros ISV o IHV. Los datos se usan para diagnosticar errores inducción de bloqueo, que pueden incluir errores de seguridad. También es necesario para el informe de errores corporativo
|||         
            
<br />          

## <a name="windows-event-collector"></a>Recopilador de eventos de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio administra suscripciones persistentes a eventos de orígenes remotos que admiten el protocolo WS-Management. Esto incluye los registros de eventos de Windows Vista, hardware y orígenes de eventos con IPMI. El servicio almacena los eventos reenviados en un registro de eventos local. Si este servicio está detenido o deshabilitado no se puede crear suscripciones a eventos y eventos reenviados no se puede aceptar.
|   **Nombre del servicio**    |   Wecsvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Recopila eventos ETW (incluidos los eventos de seguridad) para la administración de diagnósticos.  Una gran cantidad de características y herramientas de terceros que se basa en ella, incluidas las herramientas de auditoría de seguridad
|||         
            
<br />          

## <a name="windows-event-log"></a>Registro de sucesos de Windows            
| | |           
|---|---|       
|   **Descripción del servicio** |   Este servicio administra los eventos y registros de eventos. Es compatible con sucesos de registro, consultar eventos, suscribirse a eventos, archivar los registros de eventos y administración de metadatos del evento. Puede mostrar los eventos en formato XML y texto sin formato. Puede poner en peligro al detener este servicio de seguridad y confiabilidad del sistema.
|   **Nombre del servicio**    |   Registro de eventos
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Firewall de Windows         
| | |           
|---|---|   
|   **Descripción del servicio** |   Firewall de Windows ayuda a proteger el equipo al impedir que los usuarios no autorizados obtengan acceso al equipo a través de Internet o una red.
|   **Nombre del servicio**    |   MpsSvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Servicio de caché de fuente de Windows      
| | |           
|---|---|   
|   **Descripción del servicio** |   Optimiza el rendimiento de las aplicaciones almacenando en caché datos de fuente más usados. Las aplicaciones iniciarán este servicio si no se está ejecutando. Se puede deshabilitar, aunque si lo hace por lo que afectará negativamente al rendimiento de la aplicación.
|   **Nombre del servicio**    |   FontCache
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Adquisición de imágenes de Windows (WIA)          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de adquisición de imágenes para escáneres y cámaras
|   **Nombre del servicio**    |   stisvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Servicio de Windows Insider     
| | |           
|---|---|   
|   **Descripción del servicio** |   wisvc
|   **Nombre del servicio**    |   wisvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Servidor no admite la distribución de paquetes piloto, por lo que es una operación inefectiva en servidor. También se puede deshabilitar característica a través de la directiva de grupo.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Descripción del servicio** |   Agrega, modifica y quita aplicaciones proporcionadas como un paquete de Windows Installer (*.msi, *.msp). Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   msiserver
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Servicio de administrador de licencias de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona compatibilidad de infraestructura con la Microsoft Store.  Este servicio se inicia a petición y si está deshabilitada, el contenido adquirido a través de la Microsoft Store no funcionará correctamente.
|   **Nombre del servicio**    |   LicenseManager
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Instrumental de administración de Windows       
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona un modelo común de interfaz y el objeto de obtener acceso a información de administración sobre el sistema operativo, dispositivos, aplicaciones y servicios. Si se detiene este servicio, la mayoría del software basado en Windows no funcionará correctamente. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   WinMgmt
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Servicio de zona con cobertura inalámbrica móvil de Windows          
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la capacidad de compartir una conexión de datos móviles con otro dispositivo.
|   **Nombre del servicio**    |   icssvc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Instalador de módulos de Windows        
| | |           
|---|---|   
|   **Descripción del servicio** |   Permite la instalación, modificación y eliminación de las actualizaciones de Windows y componentes opcionales. Si se deshabilita este servicio, instalar o desinstalar de Windows podrían producir un error de las actualizaciones para este equipo.
|   **Nombre del servicio**    |   TrustedInstaller
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Servicio de sistema de notificaciones de inserción de Windows            
| | |           
|---|---|
|   **Descripción del servicio** |   Este servicio se ejecuta en la sesión 0 y hospeda el proveedor de conexión y la plataforma de notificación que controla la conexión entre el dispositivo y el servidor WNS.
|   **Nombre del servicio**    |   WpnService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Automático
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Es necesario para los iconos dinámicos y otras características
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Servicio de usuario de las notificaciones de inserción de Windows          
| | |           
|---|---|   
|   **Descripción del servicio** |   Este servicio hospeda la plataforma de notificación de Windows que proporciona compatibilidad para local y envío de notificaciones push. Notificaciones admitidas son icono, del sistema y sin formato.
|   **Nombre del servicio**    |   WpnUserService
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Aceptar para deshabilitar
|   **Comentarios**    |   Plantilla de servicio de usuario
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Administración remota de Windows (WS-Management)            
| | |           
|---|---|   
|   **Descripción del servicio** |   El servicio de administración remota de Windows (WinRM) implementa el protocolo WS-Management para la administración remota. WS-Management es un protocolo estándar de servicios web para la administración remota de software y hardware. El servicio WinRM está a la escucha en la red de las solicitudes de WS-Management y las procesa. El servicio WinRM debe configurarse con un agente de escucha mediante la herramienta de línea de comandos winrm.cmd o mediante la directiva de grupo para que esté a la escucha en la red. El servicio WinRM proporciona acceso a datos WMI y permite la recopilación de eventos. La recopilación de eventos y la suscripción a eventos requieren que se ejecute el servicio. Los mensajes de WinRM usan HTTP y HTTPS como transportes. El servicio WinRM no depende de IIS, sino que está configurado previamente para compartir un puerto con IIS en el mismo equipo.  El servicio WinRM se reserva el prefijo de dirección URL /wsman. Para evitar conflictos con IIS, los administradores deben asegurarse de que ningún sitio web hospedado en IIS utilice el prefijo de dirección URL /wsman.
|   **Nombre del servicio**    |   WinRM
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Es necesario para la administración remota
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Descripción del servicio** |   Proporciona la indización de contenido, el almacenamiento en caché de propiedad y los resultados de búsqueda para archivos, correo electrónico y otros contenidos.
|   **Nombre del servicio**    |   WSearch
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Deshabilitada
|   **Recomendación**  |   Ya se ha deshabilitado
|   **Comentarios**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Hora de Windows        
| | |           
|---|---|   
|   **Descripción del servicio** |   Mantiene la sincronización de fecha y hora en todos los clientes y servidores de la red. Si se detiene este servicio, la sincronización de fecha y hora no estará disponible. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   W32Time
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **Descripción del servicio** |   Habilita la detección, la descarga y la instalación de actualizaciones de Windows y otros programas. Si se deshabilita este servicio, los usuarios de este equipo no podrán usar su característica de actualización automática o de actualización de Windows y programas no podrá usar la API de Windows Update Agent (WUA).
|   **Nombre del servicio**    |   wuauserv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servicio de detección automática de Proxy Web de WinHTTP         
| | |           
|---|---|   
|   **Descripción del servicio** |   WinHTTP implementa la pila HTTP de cliente y proporciona a los desarrolladores con una API de Win32 y un componente de automatización COM para enviar solicitudes HTTP y recibir respuestas. Además, WinHTTP proporciona compatibilidad con detección automática de una configuración de proxy a través de su implementación del protocolo de detección automática de Proxy Web (WPAD).
|   **Nombre del servicio**    |   WinHttpAutoProxySvc
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  |   No deshabilite
|   **Comentarios**    |   Todo lo que usa la pila de red puede tener una dependencia funcional en este servicio. Muchas organizaciones confían en esta opción para configurar el proxy HTTP de sus redes internas de enrutamiento.  Sin él, que se originan internamente las conexiones HTTP a Internet todo producirá un error.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Redes cableadas         
| | |           
|---|---|       
|   **Descripción del servicio** |   El servicio cableadas (DOT3SVC) es responsable de IEEE 802.1X de realizar la autenticación en las interfaces Ethernet. Si su implementación actual de la red cableada exige la autenticación 802.1X, el servicio DOT3SVC debe configurarse para ejecutarse para establecer conectividad de capa 2 o proporcionar acceso a recursos de red. No se ven afectadas por el servicio DOT3SVC redes conectadas por cable que no exigen la autenticación 802.1X.
|   **Nombre del servicio**    |   dot3svc
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones   
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Adaptador de rendimiento de WMI          
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona información de la biblioteca de rendimiento de los proveedores de Instrumental de administración de Windows (WMI) a los clientes en la red. Este servicio se ejecuta solo cuando se activa la aplicación auxiliar de datos de rendimiento.
|   **Nombre del servicio**    |   wmiApSrv
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Manual
|   **Recomendación**  | No hay instrucciones       
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="workstation"></a>estación de trabajo          
| | |           
|---|---|   
|   **Descripción del servicio** |   Crea y mantiene conexiones de red de cliente a servidores remotos mediante el protocolo SMB. Si se detiene este servicio, estas conexiones no estará disponibles. Si se deshabilita este servicio, no podrá iniciar los servicios que dependan explícitamente de él.
|   **Nombre del servicio**    |   LanmanWorkstation
|   **Instalación**    |   Siempre instalado
|   **StartType**   |   Automático
|   **Recomendación**  | No hay instrucciones       
|   **Comentarios**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Administrador de Xbox Live Auth           
| | |           
|---|---|   
|   **Descripción del servicio** |   Proporciona servicios de autenticación y autorización para interactuar con Xbox Live. Si se detiene este servicio, es posible que algunas aplicaciones no funcionan correctamente.
|   **Nombre del servicio**    |   XblAuthManager
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitada
|   **Comentarios**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Guardar juego de Xbox Live          
| | |           
|---|---|   
|   **Descripción del servicio** |   Las sincronizaciones de este servicio guardar datos de Xbox Live guardar juegos habilitados.  Si se detiene este servicio, juego guardar datos no se cargue en o descargar de Xbox Live.
|   **Nombre del servicio**    |   XblGameSave
|   **Instalación**    |   Solo con la experiencia de escritorio
|   **StartType**   |   Manual
|   **Recomendación**  |   Debe estar deshabilitada
|   **Comentarios**    |   Las sincronizaciones de este servicio guardar datos de Xbox Live guardar juegos habilitados.  Si se detiene este servicio, juego guardar datos no se cargue en o descargar de Xbox Live.
|||         
                
<br /> 
<br /> 

