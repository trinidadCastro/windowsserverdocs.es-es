---
title: Solucionar problemas relacionados con la autenticación
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e3aa3bf89a73f944b2922fcf06ac60df2874a221
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853658"
---
# <a name="troubleshooting-authentication-issues"></a>Solucionar problemas relacionados con la autenticación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para la solución de problemas relacionados con los problemas que los usuarios pueden tener al intentar conectarse a DirectAccess mediante la autenticación de OTP. Los eventos relacionados con OTP de DirectAccerss se registran en el equipo cliente en Visor de eventos en **registros de aplicaciones y servicios/Microsoft/Windows/OtpCredentialProvider**. Asegúrese de que este registro esté habilitado al solucionar problemas con OTP de DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>No se pudo obtener acceso a la entidad de certificación que emite certificados OTP  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se pudo realizar la inscripción de certificados OTP para el usuario <username> en el servidor de CA < CA_name >, error en la solicitud, posibles causas del error: no se puede resolver el nombre del servidor CA, no se puede tener acceso al servidor CA a través del primer túnel de DirectAccess o no se puede establecer la conexión con el servidor de CA.  
  
**Causa**  
  
El usuario proporcionó una contraseña de un solo tiempo válida y el servidor de DirectAccess firmó la solicitud de certificado. sin embargo, el equipo cliente no puede ponerse en contacto con la entidad de certificación que emite certificados OTP para finalizar el proceso de inscripción.  
  
**Solución**  
  
En el servidor de DirectAccess, ejecute los siguientes comandos de Windows PowerShell:  
  
1.  Obtiene la lista de entidades de certificación de emisión de OTP configuradas y comprueba el valor de "CAServer": `Get-DAOtpAuthentication`  
  
2.  Asegúrese de que las CA estén configuradas como servidores de administración: `Get-DAMgmtServer -Type All`  
  
3.  Asegúrese de que el equipo cliente ha establecido el túnel de infraestructura: en la consola Firewall de Windows con seguridad avanzada, expanda **supervisión/asociaciones de seguridad**, haga clic en **modo principal**y asegúrese de que las asociaciones de seguridad de IPSec aparecen con las direcciones remotas correctas para la configuración de DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemas de Conectividad del servidor de DirectAccess  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente)  
  
Uno de los siguientes errores:  
  
-   No se puede establecer una conexión con el servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y el puerto < OTP_authentication_port >. Código de error: > de internal_error_code de <.  
  
-   Las credenciales de usuario no se pueden enviar al servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y el puerto < OTP_authentication_port >. Código de error: > de internal_error_code de <.  
  
-   No se recibió una respuesta del servidor de acceso remoto < DirectAccess_server_hostname > mediante la ruta de acceso base < OTP_authentication_path > y el puerto < OTP_authentication_port >. Código de error: > de internal_error_code de <.  
  
**Causa**  
  
El equipo cliente no puede tener acceso al servidor de DirectAccess a través de Internet, debido a problemas de red o a un servidor IIS mal configurado en el servidor de DirectAccess.  
  
**Solución**  
  
Asegúrese de que la conexión a Internet en el equipo cliente funciona y asegúrese de que el servicio de DirectAccess se está ejecutando y es accesible a través de Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>No se pudo inscribir el certificado de inicio de sesión OTP de DirectAccess  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se pudo realizar la inscripción de certificados desde la CA < CA_name >. La solicitud no se firmó según lo esperado por el certificado de firma de OTP o el usuario no tiene permiso para inscribirse.  
  
**Causa**  
  
La contraseña de un solo uso que proporciona el usuario era correcta, pero la entidad de certificación emisora (CA) rechazó la emisión del certificado de inicio de sesión de OTP. Es posible que la solicitud de certificado no esté firmada correctamente con el EKU correcto (Directiva de aplicación de la autoridad de registro de OTP) o que el usuario no tenga el permiso "inscribir" en la plantilla de OTP de DA.  
  
**Solución**  
  
Asegúrese de que los usuarios OTP de DirectAccess tengan permiso para inscribirse en el certificado de inicio de sesión OTP de DirectAccess y que la "Directiva de aplicación" adecuada esté incluida en la plantilla de firma de autoridad de registro de OTP de DA. Asegúrese también de que el certificado de la entidad de registro de DirectAccess en el servidor de acceso remoto sea válido. Consulte 3,2 planeación de la plantilla de certificado OTP y 3,3 planear el certificado de la entidad de registro.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Falta el certificado de cuenta de equipo o no es válido  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente).  No se puede completar la autenticación de OTP porque el certificado de equipo necesario para OTP no se encuentra en el almacén de certificados de la máquina local.  
  
**Causa**  
  
La autenticación OTP de DirectAccess requiere un certificado de equipo cliente para establecer una conexión SSL con el servidor de DirectAccess. sin embargo, el certificado de equipo cliente no se encontró o no es válido, por ejemplo, si el certificado expiró.  
  
**Solución**  
  
Asegúrese de que el certificado de equipo existe y es válido:  
  
1.  En el equipo cliente, en la consola de certificados MMC, para la cuenta de equipo local, Abra **personal/certificados**.  
  
2.  Asegúrese de que haya un certificado emitido que coincida con el nombre del equipo y haga doble clic en el certificado.  
  
3.  En el cuadro de diálogo **certificado** , en la pestaña **ruta de acceso del certificado** , en estado del **certificado**, asegúrese de que indica "este certificado es correcto".  
  
Si no se encuentra un certificado válido, elimine el certificado no válido (si existe) y vuelva a inscribirse en el certificado de equipo ejecutando `gpupdate /Force` desde un símbolo del sistema con privilegios elevados o reiniciando el equipo cliente.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Falta una entidad de certificación que emite certificados OTP  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se puede completar la autenticación de OTP porque el servidor de DA no devolvió una dirección de una CA emisora.  
  
**Causa**  
  
No hay ninguna CA que emita certificados OTP configurados o todas las CA configuradas que emiten certificados OTP no responden.  
  
**Solución**  
  
1.  Use el siguiente comando para obtener la lista de entidades de certificación que emiten certificados OTP (el nombre de la entidad de certificación se muestra en CAServer): `Get-DAOtpAuthentication`.  
  
2.  Si no hay ninguna CA configurada:  
  
    1.  Use el `Set-DAOtpAuthentication` de comandos o la consola de administración de acceso remoto para configurar las CA que emiten el certificado de inicio de sesión OTP de DirectAccess.  
  
    2.  Aplique la nueva configuración y fuerce a los clientes a actualizar la configuración de GPO de DirectAccess ejecutando `gpupdate /Force` desde un símbolo del sistema con privilegios elevados o reiniciando el equipo cliente.  
  
3.  Si hay CA configuradas, asegúrese de que están en línea y responde a las solicitudes de inscripción.  
  
## <a name="misconfigured-directaccess-server-address"></a>Dirección de servidor de DirectAccess configurada incorrectamente  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). La autenticación de OTP no se puede completar según lo esperado. No se puede determinar el nombre o la dirección del servidor de acceso remoto.  Código de error: > de error_code de <. El administrador del servidor debe validar la configuración de DirectAccess.  
  
**Causa**  
  
La dirección del servidor de DirectAccess no está configurada correctamente.  
  
**Solución**  
  
Compruebe la dirección de servidor de DirectAccess configurada mediante `Get-DirectAccess` y corrija la dirección si está mal configurada.  
  
Asegúrese de que la configuración más reciente está implementada en el equipo cliente. para ello, ejecute `gpupdate /force` desde un símbolo del sistema con privilegios elevados o reinicie el equipo cliente.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>No se pudo generar la solicitud de certificado de inicio de sesión OTP  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se puede inicializar la solicitud de certificado para la autenticación de OTP. No se puede generar una clave privada o el usuario <username> no puede tener acceso a la plantilla de certificado < OTP_template_name > en el controlador de dominio.  
  
**Causa**  
  
Hay dos causas posibles de este error:  
  
-   El usuario no tiene permiso para leer la plantilla de inicio de sesión de OTP.  
  
-   El equipo del usuario no puede tener acceso al controlador de dominio debido a problemas de red.  
  
**Solución**  
  
-   Revise la configuración de permisos en la plantilla de inicio de sesión de OTP y asegúrese de que todos los usuarios aprovisionados para OTP de DirectAccess tienen permiso de lectura.  
  
-   Asegúrese de que el controlador de dominio está configurado como un servidor de administración y que el equipo cliente puede tener acceso al controlador de dominio a través del túnel de infraestructura. Consulte 3,2 planeamiento de la plantilla de certificado OTP.  
  
## <a name="no-connection-to-the-domain-controller"></a>No hay conexión con el controlador de dominio  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se puede establecer una conexión con el controlador de dominio para la autenticación de OTP. Código de error: > de error_code de <.  
  
**Causa**  
  
Hay dos causas posibles de este error:  
  
-   El equipo del usuario no tiene conectividad de red.  
  
-   No se puede tener acceso al controlador de dominio a través del túnel de infraestructura.  
  
**Solución**  
  
-   Asegúrese de que el controlador de dominio está configurado como un servidor de administración; para ello, ejecute el siguiente comando desde un símbolo del sistema de PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Asegúrese de que el equipo cliente puede tener acceso al controlador de dominio a través del túnel de infraestructura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>El proveedor de OTP requiere desafío/respuesta  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). La autenticación de OTP con el servidor de acceso remoto (< DirectAccess_server_name >) para el usuario (<username>) requiere un desafío del usuario.  
  
**Causa**  
  
El proveedor de OTP usado requiere que el usuario proporcione credenciales adicionales en forma de intercambio de desafío/respuesta RADIUS, que no es compatible con OTP de DirectAccess de Windows Server 2012.  
  
**Solución**  
  
Configure el proveedor de OTP para que no requiera desafío/respuesta en ningún escenario.  
  
## <a name="incorrect-otp-logon-template-used"></a>Plantilla de inicio de sesión OTP incorrecta usada  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). La plantilla de CA desde la que el usuario <username> solicitó un certificado no está configurado para emitir certificados OTP.  
  
**Causa**  
  
Se ha reemplazado la plantilla de inicio de sesión OTP de DirectAccess y el equipo cliente está intentando autenticarse mediante una plantilla anterior.  
  
**Solución**  
  
Asegúrese de que el equipo cliente usa la configuración de OTP más reciente; para ello, realice una de las siguientes acciones:  
  
-   Fuerce una actualización de directiva de grupo ejecutando el siguiente comando desde un símbolo del sistema con privilegios elevados: `gpupdate /Force`.  
  
-   Reinicia el equipo cliente.  
  
## <a name="missing-otp-signing-certificate"></a>Falta el certificado de firma de OTP  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente). No se encuentra un certificado de firma de OTP. No se puede firmar la solicitud de inscripción de certificado OTP.  
  
**Causa**  
  
No se encuentra el certificado de firma OTP de DirectAccess en el servidor de acceso remoto. por lo tanto, el servidor de acceso remoto no puede firmar la solicitud de certificado de usuario. O bien no hay ningún certificado de firma o el certificado de firma ha expirado y no se ha renovado.  
  
**Solución**  
  
Siga estos pasos en el servidor de acceso remoto.  
  
1.  Compruebe el nombre de la plantilla de certificado de firma de OTP configurada mediante la ejecución del cmdlet de PowerShell `Get-DAOtpAuthentication` e inspeccione el valor de `SigningCertificateTemplateName`.  
  
2.  Use el complemento MMC certificados para asegurarse de que existe un certificado válido inscrito a partir de esta plantilla en el equipo.  
  
3.  Si no existe tal certificado, elimine el certificado expirado (si existe) e inscriba un nuevo certificado basado en esta plantilla.  
  
Para crear la plantilla de certificado de firma de OTP, consulte 3,3 planear el certificado de la entidad de registro.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>Falta un UPN o un DN incorrecto para el usuario  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (registro de eventos de cliente)  
  
Uno de los siguientes errores:  
  
-   La <username> de usuario no se puede autenticar con OTP. Asegúrese de que se ha definido un UPN para el nombre de usuario en Active Directory. Código de error: > de error_code de <.  
  
-   La <username> de usuario no se puede autenticar con OTP. Asegúrese de que se ha definido un DN para el nombre de usuario en Active Directory. Código de error: > de error_code de <.  
  
**Error recibido** (registro de eventos de servidor)  
  
No existe el nombre de usuario <username> especificado para la autenticación de OTP.  
  
**Causa**  
  
El usuario no tiene los atributos nombre principal de usuario (UPN) o nombre distintivo (DN) establecidos correctamente en la cuenta de usuario; estas propiedades son necesarias para el funcionamiento correcto de OTP de DirectAccess.  
  
**Solución**  
  
Use la consola de usuarios y equipos de Active Directory en el controlador de dominio para comprobar que ambos atributos están establecidos correctamente para el usuario que realiza la autenticación.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>El certificado OTP no es de confianza para el inicio de sesión  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Causa**  
  
La CA que emite certificados OTP no está en el almacén Enterprise NTAuth. por lo tanto, los certificados inscritos no se pueden usar para el inicio de sesión. Esto puede ocurrir en entornos de varios dominios y multibosque en los que no se establece la confianza de CA entre dominios.  
  
**Solución**  
  
Asegúrese de que el certificado de la raíz de la jerarquía de CA que emite certificados OTP está instalado en el almacén de certificados Enterprise NTAuth del dominio en el que el usuario está intentando autenticarse.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows no pudo comprobar las credenciales de usuario  
**Escenario**. El usuario no se puede autenticar mediante OTP con el error: "error de autenticación debido a un error interno"  
  
**Error recibido** (equipo cliente). Se produjo un error mientras Windows estaba comprobando sus credenciales. Vuelva a intentarlo o pida ayuda al administrador.  
  
**Causa**  
  
El protocolo de autenticación Kerberos no funciona cuando el certificado de inicio de sesión OTP de DirectAccess no incluye una CRL. El certificado de inicio de sesión OTP de DirectAccess no incluye una CRL porque:  
  
-   La plantilla de inicio de sesión OTP de DirectAccess se configuró con la opción **no incluir información de revocación en los certificados emitidos**.  
  
-   La CA está configurada para no publicar CRL.  
  
**Solución**  
  
1.  Para confirmar la causa de este error, en la consola de administración de acceso remoto, en **paso 2 servidor de acceso remoto**, haga clic en **Editar**y, a continuación, en el Asistente para la **instalación del servidor de acceso remoto** , haga clic en **plantillas de certificado de OTP**. Tome nota de la plantilla de certificado que se usa para la inscripción de certificados que se emiten para la autenticación de OTP. Abra la consola entidad de certificación, en el panel izquierdo, haga clic en **plantillas de certificado**y, a continuación, haga doble clic en el certificado de inicio de sesión OTP para ver las propiedades de la plantilla de certificado.  
  
    Para solucionar este problema, configure un certificado para el certificado de inicio de sesión de OTP y no active la casilla no **incluir información de revocación en los certificados emitidos** en la pestaña **servidor** del cuadro de diálogo Propiedades de la plantilla.  
  
2.  En el servidor de CA, abra el MMC de la entidad de certificación, haga clic con el botón secundario en la CA emisora y haga clic en **propiedades**. En la pestaña **extensiones** , asegúrese de que la publicación de CRL está configurada correctamente.  
  


