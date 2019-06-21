---
title: Solucionar problemas relacionados con la autenticación
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08dd6822cc30135506d82041cfbeab0bc1a058ab
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282356"
---
# <a name="troubleshooting-authentication-issues"></a>Solucionar problemas relacionados con la autenticación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para solucionar problemas relacionados con problemas de los usuarios pueden tener cuando intenta conectarse a DirectAccess con autenticación OTP. DirectAccerss OTP relacionados con los eventos se registran en el equipo cliente en el Visor de eventos en **aplicaciones y los registros de servicios/Microsoft/Windows/OtpCredentialProvider**. Asegúrese de que este registro está habilitado al solucionar problemas con OTP de DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>No se pudo obtener acceso a la entidad de certificación que emite certificados OTP  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). Inscripción de certificado OTP para el usuario <username> error en el servidor de CA < CA_name >, con errores, posibles causas del error de solicitud: No se puede resolver el nombre del servidor de CA, el servidor de CA no se puede acceder a través del primer túnel de DirectAccess o no se puede establecer la conexión al servidor de CA.  
  
**Causa**  
  
El usuario proporcionó una contraseña temporal válida y el servidor de DirectAccess iniciado la solicitud de certificado Sin embargo, el equipo cliente no puede ponerse en contacto con la entidad de certificación que emite certificados OTP para finalizar el proceso de inscripción.  
  
**Solución**  
  
En el servidor de DirectAccess, ejecute los siguientes comandos de Windows PowerShell:  
  
1.  Obtener la lista de OTP configurada la CA emisora y compruebe el valor de 'CAServidor': `Get-DAOtpAuthentication`  
  
2.  Asegúrese de que las entidades de certificación se configuran como los servidores de administración: `Get-DAMgmtServer -Type All`  
  
3.  Asegúrese de que el equipo cliente ha establecido el túnel de infraestructura: En el Firewall de Windows con la consola de seguridad avanzada, expanda **las asociaciones de seguridad/supervisión**, haga clic en **modo principal**y asegúrese de que aparezcan las asociaciones de seguridad de IPsec con el elemento remoto correcto direcciones para la configuración de DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemas de conectividad del servidor de DirectAccess  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente)  
  
Uno de los siguientes errores:  
  
-   No se puede establecer una conexión al servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y < OTP_authentication_port > del puerto. Código de error: < internal_error_code >.  
  
-   Las credenciales de usuario no pueden enviarse al servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y < OTP_authentication_port > del puerto. Código de error: < internal_error_code >.  
  
-   No se recibió una respuesta del servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y < OTP_authentication_port > del puerto. Código de error: < internal_error_code >.  
  
**Causa**  
  
El equipo cliente no puede tener acceso al servidor de DirectAccess a través de Internet, debido a problemas de cualquier red o a un servidor IIS mal configurado en el servidor de DirectAccess.  
  
**Solución**  
  
Asegúrese de que funciona la conexión a Internet en el equipo cliente y asegúrese de que el servicio de DirectAccess está en ejecución y accesible a través de Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>No se pudo inscribir el certificado de inicio de sesión de OTP de DirectAccess  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). Error de inscripción de certificado de CA < CA_name >. La solicitud no estaba firmada según lo previsto, el certificado de firma de OTP o el usuario no tiene permiso para inscribirse.  
  
**Causa**  
  
La contraseña temporal proporcionada por el usuario era correcta, pero la rechazó la entidad de certificación (CA) emisora emitir el certificado de inicio de sesión OTP. La solicitud de certificado no puede estar firmada correctamente con el EKU correcto (directiva de aplicación de autoridad de registro OTP) o el usuario no tiene el permiso de "Inscripción" en la plantilla de OTP de DA.  
  
**Solución**  
  
Asegúrese de que los usuarios de OTP de DirectAccess tengan permiso para inscribir el certificado de inicio de sesión de OTP de DirectAccess y que se incluye la correcta "Directiva de aplicación" en la autoridad de registro de OTP DA plantilla firma. Además, asegúrese de que el certificado de autoridad de registro de DirectAccess en el servidor de acceso remoto es válido. Consulte 3.2 Planear la plantilla de certificado OTP y 3.3 planear el certificado de autoridad de registro.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificado de cuenta de equipo que falta o no es válido  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente).  Autenticación de OTP no se puede completar porque no se encuentra el certificado de equipo necesario para OTP en el almacén de certificados equipo local.  
  
**Causa**  
  
Autenticación de OTP de DirectAccess requiere un certificado de equipo cliente para establecer una conexión SSL con el servidor de DirectAccess Sin embargo, el certificado del equipo cliente no se encontró o no es válido, por ejemplo, si el certificado ha expirado.  
  
**Solución**  
  
Asegúrese de que el certificado de equipo existe y es válido:  
  
1.  En el equipo cliente, en la consola de certificados MMC, para la cuenta de equipo Local, abra **Personal/certificados**.  
  
2.  Asegúrese de que haya emitido un certificado que coincide con el nombre del equipo y haga doble clic en el certificado.  
  
3.  En el **certificado** cuadro de diálogo el **ruta de acceso del certificado** ficha **estado del certificado**, asegúrese de que dice "este certificado es correcto."  
  
Si no se encuentra un certificado válido, elimine el certificado no válido (si existe) y volver a inscribir el certificado de equipo por la que se ejecuten `gpupdate /Force` desde un símbolo del sistema con privilegios elevados o reiniciar el equipo cliente.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Falta la entidad de certificación que emite certificados OTP  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). Autenticación de OTP no se puede completar porque el servidor DA no devolvió una dirección de una CA emisora.  
  
**Causa**  
  
No hay ninguna CA que emiten certificados OTP configurados o todas las CA configuradas que emiten certificados OTP son no responde.  
  
**Solución**  
  
1.  Use el comando siguiente para obtener la lista de entidades emisoras de certificados que emiten certificados OTP (se muestra el nombre de entidad de certificación en CAServidor): `Get-DAOtpAuthentication`.  
  
2.  Si no se configura ninguna CA:  
  
    1.  Use el comando `Set-DAOtpAuthentication` o la consola de administración de acceso remoto para configurar las entidades de certificación que emiten certificados de inicio de sesión de la OTP de DirectAccess.  
  
    2.  Aplicar la nueva configuración y forzar a los clientes para actualizar la configuración de GPO de DirectAccess mediante la ejecución de `gpupdate /Force` desde un símbolo del sistema con privilegios elevados o reiniciar el equipo cliente.  
  
3.  Si no hay entidades emisoras de certificados configurados, asegúrese de que estén en línea y que responde a las solicitudes de inscripción.  
  
## <a name="misconfigured-directaccess-server-address"></a>Dirección del servidor de DirectAccess mal configurado  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). No se puede completar la autenticación de OTP según lo previsto. No se puede determinar el nombre o la dirección del servidor de acceso remoto.  Código de error: < error_code >. Configuración de DirectAccess debe validarse por el administrador del servidor.  
  
**Causa**  
  
La dirección del servidor de DirectAccess no está configurada correctamente.  
  
**Solución**  
  
Comprobación de la configurada mediante dirección de DirectAccess server `Get-DirectAccess` y corrija la dirección, si lo está mal configurada.  
  
Asegúrese de que la configuración más reciente se implementa en el equipo cliente mediante la ejecución de `gpupdate /force` desde un símbolo del sistema con privilegios elevados o reiniciar el equipo cliente.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>No se pudo generar la solicitud de certificado de inicio de sesión OTP  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). No se puede inicializar la solicitud de certificado para la autenticación de OTP. No se puede generar una clave privada, o usuario <username> no puede tener acceso a la plantilla de certificado < OTP_template_name > en el controlador de dominio.  
  
**Causa**  
  
Hay dos causas posibles para este error:  
  
-   El usuario no tiene permiso para leer la plantilla de inicio de sesión OTP.  
  
-   El equipo del usuario no puede acceder el controlador de dominio debido a problemas de red.  
  
**Solución**  
  
-   Revise la configuración en la plantilla de inicio de sesión OTP de permisos y asegúrese de que todos los usuarios aprovisionados para OTP de DirectAccess han "permiso de lectura'.  
  
-   Asegúrese de que el controlador de dominio está configurado como un servidor de administración y que el equipo cliente puede alcanzar el controlador de dominio a través del túnel de infraestructura. Ver Plan 3,2 la plantilla de certificado OTP.  
  
## <a name="no-connection-to-the-domain-controller"></a>Ninguna conexión con el controlador de dominio  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). No se puede establecer una conexión con el controlador de dominio con el propósito de autenticación de OTP. Código de error: < error_code >.  
  
**Causa**  
  
Hay dos causas posibles para este error:  
  
-   El equipo del usuario no tiene ninguna conectividad de red.  
  
-   El controlador de dominio no es accesible a través del túnel de infraestructura.  
  
**Solución**  
  
-   Asegúrese de que el controlador de dominio está configurado como un servidor de administración ejecutando el comando siguiente desde un símbolo del sistema de PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Asegúrese de que el equipo cliente puede alcanzar el controlador de dominio a través del túnel de infraestructura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Proveedor de la OTP requiere desafío/respuesta  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). Autenticación de OTP con el servidor de acceso remoto (< DirectAccess_server_name >) para el usuario (<username>) necesitado un desafío del usuario.  
  
**Causa**  
  
El proveedor OTP utilizado requiere al usuario que proporcione credenciales adicionales en forma de un intercambio de desafío/respuesta RADIUS, que no es compatible con Windows Server 2012 DirectAccess OTP.  
  
**Solución**  
  
Configurar el proveedor OTP que no se requiere en cualquier escenario de desafío/respuesta.  
  
## <a name="incorrect-otp-logon-template-used"></a>Plantilla de inicio de sesión incorrecto OTP utilizada  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). La plantilla de entidad emisora de certificados de usuario que <username> solicita un certificado no está configurado para emitir certificados OTP.  
  
**Causa**  
  
Se ha reemplazado la plantilla de inicio de sesión de OTP de DirectAccess y el equipo cliente esté intentando autenticarse mediante una plantilla anterior.  
  
**Solución**  
  
Asegúrese de que el equipo cliente usa la configuración de OTP más reciente mediante la realización de una de las siguientes:  
  
-   Forzar una actualización de directiva de grupo ejecutando el comando siguiente desde un símbolo del sistema con privilegios elevados: `gpupdate /Force`.  
  
-   Reinicia el equipo cliente.  
  
## <a name="missing-otp-signing-certificate"></a>Falta el certificado de firma de OTP  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente). No se encuentra un certificado de firma de OTP. No se puede firmar la solicitud de inscripción de certificado OTP.  
  
**Causa**  
  
No se encuentra el certificado de firma de OTP de DirectAccess en el servidor de acceso remoto; por lo tanto, no se puede firmar la solicitud de certificado de usuario por el servidor de acceso remoto. No hay ningún certificado de firma o el certificado de firma ha expirado y no se renueva.  
  
**Solución**  
  
Siga estos pasos en el servidor de acceso remoto.  
  
1.  Compruebe el nombre de plantilla de certificado firmado de OTP configurado ejecutando el cmdlet de PowerShell `Get-DAOtpAuthentication` e inspeccionar el valor de `SigningCertificateTemplateName`.  
  
2.  Use el complemento MMC certificados para asegurarse de que existe un certificado válido inscrito desde esta plantilla en el equipo.  
  
3.  Si no existe ningún certificado de este tipo, elimine el certificado que ha expirado (si existe) e inscribirse en un nuevo certificado basado en esta plantilla.  
  
Para crear la firma de OTP plantilla de certificado ver Plan 3.3 el certificado de autoridad de registro.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>UPN/DN falta o es incorrecto para el usuario  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (registro de eventos de cliente)  
  
Uno de los siguientes errores:  
  
-   Usuario <username> no se puede autenticar con OTP. Asegúrese de que se ha definido un UPN para el nombre de usuario en Active Directory. Código de error: < error_code >.  
  
-   Usuario <username> no se puede autenticar con OTP. Asegúrese de que se ha definido un nombre completo del nombre de usuario en Active Directory. Código de error: < error_code >.  
  
**Recibidas el error** (registro de eventos de servidor)  
  
El nombre de usuario <username> especificado para la autenticación de OTP no existe.  
  
**Causa**  
  
El usuario no tiene el nombre Principal de usuario (UPN) o los atributos de nombre distintivo (DN) se han establecido adecuadamente en la cuenta de usuario, estas propiedades son necesarias para el funcionamiento correcto de OTP de DirectAccess.  
  
**Solución**  
  
Use la consola de equipos y usuarios de Active Directory en el controlador de dominio para comprobar que ambos atributos están establecidas correctamente para el usuario autenticado.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Certificado OTP no es de confianza para el inicio de sesión  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Causa**  
  
La entidad de certificación que emite certificados OTP no está en el almacén NTAuth de empresa; por lo tanto, no se puede usar certificados de inscripción para el inicio de sesión. Esto puede ocurrir en entornos de dominios y bosques múltiples de múltiple donde no se establece entre dominios de confianza de CA.  
  
**Solución**  
  
Asegúrese de que está instalado el certificado de la raíz de la jerarquía de CA que emite certificados OTP en la empresa almacenar certificado NTAuth del dominio al que intenta autenticar al usuario.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows no podrían comprobar las credenciales de usuario  
**Escenario**. Usuario no puede autenticarse con el OTP con el error: "Error de autenticación debido a un error interno"  
  
**Recibidas el error** (equipo cliente). Algo ha ido mal mientras Windows estaban comprobando las credenciales. Vuelva a intentarlo, o pida al administrador para obtener ayuda.  
  
**Causa**  
  
El protocolo de autenticación Kerberos no funciona cuando el certificado de inicio de sesión de OTP de DirectAccess no incluye una CRL. El certificado de inicio de sesión de OTP de DirectAccess no incluye una CRL porque ambos:  
  
-   La plantilla de inicio de sesión de OTP de DirectAccess se ha configurado con la opción **no incluyen información de revocación de certificados emitidos**.  
  
-   La entidad de certificación está configurado para no publicar CRL.  
  
**Solución**  
  
1.  Para confirmar la causa de este error, en la consola de administración de acceso remoto, en **paso 2: servidor de acceso remoto**, haga clic en **editar**y, a continuación, en el **instalación del servidor de acceso remoto** el asistente, haga clic en **plantillas de certificado de OTP**. Tome nota de la plantilla de certificado utilizada para la inscripción de certificados emitidos para la autenticación de OTP. Abrir la consola de entidad de certificación, en el panel izquierdo, haga clic **plantillas de certificado**, haga doble clic en el certificado OTP de inicio de sesión para ver las propiedades de plantilla de certificado.  
  
    Para solucionar este problema, configure un certificado para el certificado OTP de inicio de sesión y no se selecciona el **no incluyen información de revocación de certificados emitidos** casilla de verificación en la **Server** ficha de la plantilla cuadro de diálogo de propiedades.  
  
2.  En el servidor de CA, abra el MMC de entidad de certificación, haga clic con el botón secundario del mouse en la CA emisora y haga clic en **propiedades**. En el **extensiones** ficha Asegúrese de que la publicación de CRL está configurada correctamente.  
  


