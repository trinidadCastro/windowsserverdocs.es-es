---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: "Actualizaciones necesarias para los servicios de federación de Active Directory (AD FS)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Actualizaciones necesarias de servicios de federación de Active Directory (AD FS) y el Proxy de aplicación Web (WAP)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1

A partir de octubre de 2016, todas las actualizaciones de todos los componentes de Windows Server se liberan solo a través de Windows Update (WU).  Hay ningún más revisiones o descargas individuales.
Esto se aplica a Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 SP1.

Esta página indican los paquetes acumulativos de actualizaciones de especial interés para AD FS y WAP, así como la lista de historial de actualizaciones de revisión recomendada para AD FS y WAP.

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Actualizaciones de AD FS y WAP en Windows Server 2016
Las actualizaciones de Windows Server 2016 se entregan mensualmente mediante Windows Update y son acumulativas. El paquete de actualización que se indican a continuación se recomienda para todos los servidores de AD FS y WAP 2016 e incluye todas las actualizaciones necesarias anteriormente, así como las correcciones más recientes.

|KB # |Descripción|Fecha de lanzamiento
|----- | ----- |-----
|[4041688 (compilación del SO 14393.1794)](https://support.microsoft.com/kb/4041688)|Esta corrección resuelve un problema que intermitentemente misdirects solicitudes de anuncios autoridad al proveedor de identidad incorrecto debido a un comportamiento almacenamiento en caché incorrecto. Esto puede afectar a las características de autenticación como Multi Factor de autenticación. </br></br>Agregado la capacidad de AAD conectar salud al informe de estado del servidor ADFS con fidelidad correcto (uso de auditoría detallada) en conjuntos de WS2012R2 y ADFS WS2016 mixed.</br></br>Corrige un problema que durante la actualización de 2012 R2 ADFS conjunto a ADFS 2016, el cmdlet de powershell para aumentar el nivel de comportamiento de granja produce un error con un tiempo de espera cuando hay muchas confianzas de terceros de confianza.</br></br>Resuelve un problema que hace que errores de autenticación de AD FS modificando el valor del parámetro wct mientras federación las solicitudes a otro servidor Token de seguridad (STS).|Octubre de 2017|
|[4038801 (compilación del SO 14393.1737)](https://support.microsoft.com/kb/4038801)|Compatibilidad con una sesión OIDC usando LDPs federados. Esto permitirá "Escenarios de pantalla completa", donde varios usuarios pueden iniciar sesión en serie en un solo dispositivo donde hay federación con una LDP.</br></br>Corregido un problema donde certificados basados en CEP/estas no funcionan con cuentas gMSA de WinHello.</br></br>Corrige un problema que genera un error en el Windows Internal Database (WID) en los servidores de Windows Server 2016 ADFS sincronizar algunas opciones de configuración, como las columnas ApplicationGroupId de tablas IdentityServerPolicy.Scopes y IdentityServerPolicy.Clients) debido a una clave externa restricción. Estos errores de sincronización puede provocar la reclamación diferentes, reclamar experiencias de proveedor y la aplicación entre los servidores ADFS primario a secundario. Además, si la función principal de WID se mueve a un nodo secundario, grupos de la aplicación dejará de estar administrables en la administración de ADFS experiencia del usuario.</br></br>Esta actualización se ha solucionado una problemas donde Multi Factor de autenticación no funciona correctamente con dispositivos móviles que usan las definiciones de referencia cultural personalizada|Septiembre de 2017|
|[4034661 (compilación del SO 14393.1613)](https://support.microsoft.com/kb/4034661)|Corrige un problema donde la dirección IP del llamador es nog registrado por 411 eventos en el registro de eventos de seguridad de ADFS 4.0 \ servidores de Windows Server 2016 RS1 ADFS incluso después de habilitar "las auditorías de aciertos" y "las auditorías de errores".</br></br>Esta corrección resuelve un problema con la autenticación de Factor de Multi de Azure (AMF) cuando se configura un servidor ADFX para usar a un servidor HTTP Proxy.</br></br>"Solucionaba un problema que presentar un certificado expirado o revocado en el servidor Proxy ADFS no devuelve un error al usuario".|Agosto de 2017|
|[4034658 (compilación del SO 14393.1593)](https://support.microsoft.com/kb/4034658)|Corregir servidor FS para anuncios de 2016 para poder admitir la inscripción de certificados MFA para Windows Hello para empresas para en las implementaciones locales|Agosto de 2017| 
|[4025334 (compilación del SO 14393.1532)](https://support.microsoft.com/kb/4025334)|Resuelve un problema en el controlador de token PkeyAuth falle una autenticación si la solicitud de pkeyauth contiene datos incorrectos. La autenticación sigue sin realizar la autenticación de dispositivo|Julio de 2017|
|[4022723 (compilación del SO 14393.1378)](https://support.microsoft.com/kb/4022723)|[Proxy de aplicación web] Valor de propiedad de la configuración de DisableHttpOnlyCookieProtection no recoge WAP 2016 en la implementación mixto 2012R2/2016 </br></br>[Proxy de aplicación web] No se puede obtener el token de acceso de usuario de AD FS en escenarios EAS Pre-autenticación.</br></br>AD FS de 2016: WSFED cierre da lugar a una excepción|Junio de 2017 
|[3213986](https://support.microsoft.com/kb/3213986)|Actualización acumulativa para Windows Server 2016 x64 64 sistemas (KB3213986)| Enero de 2017

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Actualizaciones de AD FS y WAP en Windows Server 2012 R2
A continuación te mostramos la lista de paquetes acumulativos de actualizaciones de las revisiones y actualizaciones que se han publicado para los servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2.

|KB # |Descripción|Fecha de lanzamiento
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|Resuelve un problema de AD FS donde MSISConext cookies en encabezados de solicitud finalmente pueden desborde el límite de tamaño de los encabezados y provocar errores al autenticar con código de estado HTTP 400 "Incorrecto solicitud – encabezado demasiado".</br></br>Corrige un problema que ADFS ya no puede omitir el apartado "= símbolo del sistema de inicio de sesión" durante la autenticación. Se ha agregado una opción de "Disabled" para restaurar los escenarios que se usa la autenticación sin contraseña.|Vista previa de octubre de 2017 del paquete acumulativo de actualizaciones|
|[4019217](https://support.microsoft.com/kb/4019217)|Los clientes mediante el agente de token no funcionan al usar un servidor Server 2012 R2 AD FS de carpetas de trabajo|Paquete acumulativo de actualizaciones de vista previa de mayo de 2017| 
|[4015550](https://support.microsoft.com/kb/4015550)|Hemos corregido un problema con AD FS no autenticar los usuarios externos y AD FS WAP aleatoriamente que no reenviar solicitud|Paquete acumulativo de actualizaciones de abril de 2017| 
|[4015547](https://support.microsoft.com/kb/4015547)|Hemos corregido un problema con AD FS no autenticar los usuarios externos y AD FS WAP aleatoriamente que no reenviar solicitud|Actualización de seguridad de abril de 2017| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 esta actualización de seguridad resuelve una vulnerabilidad en servicios de federación de directorio Active (ADFS). La vulnerabilidad podría permitir la divulgación de información si un atacante envía una solicitud especialmente diseñada para un servidor de AD FS, lo que permite al atacante leer información confidencial sobre el sistema de destino.|Paquete acumulativo de actualizaciones de marzo de 2017| 
|[3179574](https://support.microsoft.com/kb/3179574)|Se ha solucionado un problema con la actualización de contraseñas extranet de AD FS. |Paquete acumulativo de actualizaciones de agosto de 2016
|[3172614](https://support.microsoft.com/kb/3172614)|Petición introdujo = inicio de sesión [admiten](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7), se ha solucionado un problema con la consola de administración de AD FS y la configuración de AlwaysRequireAuthentication. |Paquete acumulativo de actualizaciones de julio de 2016
|[3163306](https://support.microsoft.com/kb/3163306)|Los servicios de federación de Active Directory (AD FS) 3.0 no se puede conectar a los almacenes de atributo de protocolo ligero de acceso a directorios (LDAP) que están configurados para usar el puerto de la capa de Sockets seguros (SSL) 636 o 3269 en la cadena de conexión. |Paquete acumulativo de actualizaciones de junio de 2016
|[3148533](https://support.microsoft.com/kb/3148533)|Se produce un error de autenticación de reserva de MFA a través del Proxy ADFS en Windows Server 2012 R2 |Mayo de 2016
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS registros no contienen la dirección IP del cliente para escenarios de bloqueo de cuenta en Windows Server 2012 R2 |Febrero de 2016
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020: Actualización de seguridad para los servicios de federación de Active Directory para una denegación de dirección del servicio: 9 de febrero de 2016|Febrero de 2016
|[3105881](https://support.microsoft.com/kb/3105881)|No puede acceder a las aplicaciones cuando se habilita la autenticación de dispositivo en el servidor basado en Windows Server 2012 R2 AD FS|Octubre de 2015
|[3092003](https://support.microsoft.com/kb/3092003)|Página se carga de forma repetida y la autenticación se produce un error cuando los usuarios usar MFA en Windows Server 2012 R2 AD FS|Agosto de 2015
|[3080778](https://support.microsoft.com/kb/3080778)|AD FS no llama a OnError al adaptador MFA produce una excepción en Windows Server 2012 R2|Julio de 2015
|[3075610](https://support.microsoft.com/kb/3075610)|Relaciones de confianza se pierden en el servidor de AD FS secundario después de agregar o quitar el proveedor de notificaciones en Windows Server 2012 R2|Julio de 2015
|[3070080](https://support.microsoft.com/kb/3070080)|Descubrir territorio principal no funcionaran correctamente para no reclamaciones cuenta confiar terceros de confianza|Junio de 2015
|[3052122](https://support.microsoft.com/kb/3052122)|Update agrega compatibilidad para compuestos reclamaciones de Id. de tokens de AD FS en Windows Server 2012 R2|Mayo de 2015
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Una vulnerabilidad en los servicios de federación de Active Directory podría permitir la divulgación de información|Abril de 2015
|[3042127](https://support.microsoft.com/kb/3042127)|"HTTP 400: solicitud incorrecta" error al abrir un buzón mediante WAP compartido en Windows Server 2012 R2|Marzo de 2015
|[3042121](https://support.microsoft.com/kb/3042121)|Protección de reproducción de token de AD FS para tokens de autenticación de Proxy de aplicación Web en Windows Server 2012 R2|Marzo de 2015
|[3035025](https://support.microsoft.com/kb/3035025)|Característica de revisión para actualizar la contraseña para que los usuarios no deben usar el dispositivo registrado en Windows Server 2012 R2|Enero de 2015
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS no puede procesar la respuesta SAML en Windows Server 2012 R2|Enero de 2015
|[3025080](https://support.microsoft.com/kb/3025080)|Operación produce un error al intentar guardar un archivo de Office a través del Proxy de aplicación Web en Windows Server 2012 R2|Enero de 2015
|[3025078](https://support.microsoft.com/kb/3025078)|No se le de nombre de usuario nuevo cuando utilizas una contraseña no son correctos para iniciar sesión en Windows Server 2012 R2|Enero de 2015
|[3020813](https://support.microsoft.com/kb/3020813)|Se te pide autenticación al ejecutar una aplicación web en Windows Server 2012 R2 AD FS|Enero de 2015
|[3020773](https://support.microsoft.com/kb/3020773)|Errores de tiempo de espera después de la implementación inicial del servicio de registro del dispositivo de Windows Server 2012 R2|Enero de 2015
|[3018886](https://support.microsoft.com/kb/3018886)|Se te pide un nombre de usuario y una contraseña dos veces cuando obtienes acceso a servidor de Windows Server 2012 R2 AD FS de intranet|Enero de 2015
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 actualización acumulados|Diciembre de 2014
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 actualización acumulados|Noviembre de 2014
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 actualización acumulados|Agosto de 2014
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 actualización acumulados|Julio de 2014
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 actualización acumulados|Junio de 2014
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 actualización acumulados|Mayo de 2014
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 actualización acumulados|Abril de 2014

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Actualizaciones de AD FS en Windows Server 2012 (AD FS 2.1) y AD FS 2.0
A continuación te mostramos la lista de paquetes acumulativos de actualizaciones de las revisiones y actualizaciones que se han publicado de AD FS 2.0 y 2.1.

|KB # |Descripción|Fecha de lanzamiento|Se aplica a:
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|Se produce un error de autenticación a través del proxy en Windows Server 2012 (es decir, la versión de revisión 3094446 general)|Paquete acumulativo de actualizaciones de calidad de noviembre de 2016|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|Se produce un error de autenticación a través del proxy en Windows Server 2008 R2 SP1 (es decir, la versión de revisión 3094446 general)|Paquete acumulativo de actualizaciones de calidad de noviembre de 2016|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|Se produce un error de autenticación a través del proxy en Windows Server 2012 o Windows Server 2008 R2 SP1|Septiembre de 2015|AD FS 2.0 y 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|AD FS 2.1 inicia una excepción cuando se autentica con un certificado de cifrado en Windows Server 2012|Julio de 2015|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062: Una vulnerabilidad en servicios de federación de Active Directory podría permitir la elevación de privilegios|Junio de 2015|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Vulnerabilidad en los servicios de federación de Active Directory podría permitir la divulgación de información: 14 de abril de 2015|Noviembre de 2014|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|Uso de memoria de servidor de federación de AD FS mantiene aumenta cuando muchos de los usuarios inician sesión en una aplicación web en Windows Server 2012|Julio de 2014|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|La confianza de terceros de confianza en AD FS se detiene cuando se realiza una solicitud a AD FS para obtener un token delegado|Mayo de 2014|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|Implementación de un conjunto ADFS SQL se produce un error si no tienes permisos de SQL|Octubre de 2014|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713) o [2989956](https://support.microsoft.com/kb/2989956)|Actualización está disponible para resolver varios problemas después de instalar la actualización de seguridad 2843638 en un servidor de AD FS|Noviembre de 2013</br></br>Septiembre de 2014|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|Update te permite usar un certificado de varios fabricantes Relying confía en un AD FS 2.1 granja|Octubre de 2013|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|REVISIÓN: Se produce un error al utilizar un HSM y CSP de terceros y, a continuación, configurar una confianza de proveedor de reclamaciones en Update 3 del paquete acumulativo de actualizaciones de AD FS 2.0 en Windows Server 2008 R2 Service Pack 1|Septiembre de 2013|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|Una coma en el nombre del sujeto del certificado de cifrado produce una excepción en Windows Server 2008 R2 SP1|Agosto de 2013|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Security] La vulnerabilidad en servicios de federación de Active Directory podría permitir la divulgación de información|Noviembre de 2013|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066: Descripción de la actualización de seguridad para los servicios de federación de Active Directory 2.0: 13 de agosto de 2013|Agosto de 2013|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Archivo federationmetadata.XML no contiene la información del extremo de intercambio de metadatos para los extremos de WS-Trust y WS-federación en Windows Server 2012|Mayo de 2013|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|Descripción del paquete acumulativo de actualizaciones 3 para servicios de federación de Active Directory (AD FS) 2.0|Marzo de 2013|AD FS 2.0




