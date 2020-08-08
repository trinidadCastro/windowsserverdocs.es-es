---
title: Paso 3 configurar el servidor de acceso remoto para OTP
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e0b03bfd06c693674e10e9eb9fba0a0dfd30b167
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969082"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Paso 3 configurar el servidor de acceso remoto para OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Una vez configurado el servidor RADIUS con los tokens de distribución de software, los puertos de comunicación están abiertos, se ha creado un secreto compartido, se han creado las cuentas de usuario correspondientes a Active Directory en el servidor RADIUS y el servidor de acceso remoto se ha configurado como un agente de autenticación RADIUS y, a continuación, el servidor de acceso remoto debe configurarse para admitir OTP.

|Tarea|Descripción|
|----|--------|
|[3,1 excluir a los usuarios de la autenticación OTP (opcional)](#BKMK_Exempt)|Si se va a eximir a usuarios específicos de DirectAccess con autenticación OTP, siga estos pasos preliminares.|
|[3,2 configurar el servidor de acceso remoto para que admita OTP](#BKMK_Config)|En el servidor de acceso remoto, actualice la configuración de acceso remoto para admitir la autenticación de dos factores de OTP.|
|[3,3 tarjetas inteligentes para la autorización adicional](#BKMK_Smartcard)|Información adicional sobre el uso de tarjetas inteligentes.|

> [!NOTE]
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [Uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="31-exempt-users-from-otp-authentication-optional"></a><a name="BKMK_Exempt"></a>3,1 excluir a los usuarios de la autenticación OTP (opcional)
Si se va a excluir a usuarios específicos de la autenticación de OTP, estos pasos se deben realizar antes de la configuración de acceso remoto:

> [!NOTE]
> Debe esperar a que se complete la replicación entre dominios al configurar el grupo de exenciones de OTP.

#### <a name="create-user-exemption-security-group"></a>Crear grupo de seguridad de exención de usuario

1.  Cree un grupo de seguridad en Active Directory para la exención de OTP.

2.  Agregue todos los usuarios que se van a excluir de la autenticación de OTP en el grupo de seguridad.

    > [!NOTE]
    > Asegúrese de incluir solo las cuentas de usuario y no las cuentas de equipo en el grupo de seguridad de exención de OTP.

## <a name="32-configure-the-remote-access-server-to-support-otp"></a><a name="BKMK_Config"></a>3,2 configurar el servidor de acceso remoto para que admita OTP
Para configurar el acceso remoto para que use la autenticación en dos fases y OTP con el servidor RADIUS y la implementación de certificados de las secciones anteriores, siga estos pasos:

#### <a name="configure-remote-access-for-otp"></a>Configurar el acceso remoto para OTP

1.  Abra **Administración de acceso remoto** y haga clic en **configuración**.

2.  En la ventana **configuración de DirectAccess** , en **paso 2: servidor de acceso remoto**, haga clic en **Editar**.

3.  Haga clic en **siguiente** tres veces y, en la sección **autenticación** , seleccione **dos factores de autenticación** y **uso de OTP**y asegúrese de que la opción **usar certificados de equipo** está activada.

    > [!NOTE]
    > Una vez que se haya habilitado OTP en el servidor de acceso remoto, si se deshabilita OTP mediante la anulación de la selección de **usar OTP**, se desinstalarán las extensiones ISAPI y CGI en el servidor.

4.  Si se requiere compatibilidad con Windows 7, active la casilla **permitir que los equipos cliente de Windows 7 se conecten a través de DirectAccess** . Nota: como se describe en la sección planeación, los clientes de Windows 7 deben tener DCA 2,0 instalado para admitir DirectAccess con OTP.

5.  Haga clic en **Next**.

6.  En la sección **servidor RADIUS OTP** , haga doble clic en el campo **nombre del servidor** en blanco.

7.  En el cuadro de diálogo **Agregar un servidor RADIUS** , escriba el nombre del servidor RADIUS en el campo **nombre del servidor** . Haga clic en **cambiar** junto al campo **secreto compartido** y escriba la misma contraseña que usó al configurar el servidor RADIUS en los campos **nuevo secreto** y **confirmar nuevo secreto** . Haga clic en **Aceptar** dos veces y haga clic en **siguiente**.

    > [!NOTE]
    > Si el servidor RADIUS está en un dominio diferente al del servidor de acceso remoto, el campo **nombre del servidor** debe especificar el FQDN del servidor RADIUS.

8.  En la sección **servidores de CA de OTP** , seleccione los servidores de CA que se usarán para la inscripción de certificados de autenticación de cliente OTP y haga clic en **Agregar**. Haga clic en **Next**.

9. En la sección **plantillas de certificado de OTP** , haga clic en **examinar** para seleccionar la plantilla de certificado que se usa para la inscripción de certificados que se emiten para la autenticación de OTP.

    > [!NOTE]
    > La plantilla de certificado para los certificados OTP emitidos por la entidad de certificación corporativa debe estar configurada sin la opción "no incluir información de revocación en los certificados emitidos". Si esta opción se selecciona durante la creación de la plantilla de certificado, los equipos cliente de OTP no podrán iniciar sesión correctamente.

    Haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para inscribir el certificado usado por el servidor de acceso remoto para firmar las solicitudes de inscripción de certificado de OTP. Haga clic en **OK**. Haga clic en **Next**.

10. Si se requiere la exención de usuarios específicos de DirectAccess con OTP, en la sección **exenciones de OTP** , seleccione no **requerir que los usuarios del grupo de seguridad especificado se autentiquen con la autenticación en dos fases**. Haga clic en **grupo de seguridad** y seleccione el grupo de seguridad que se creó para las exenciones de OTP.

11. En la página **instalación del servidor de acceso remoto** , haga clic en **Finalizar**.

12. En la ventana **configuración de DirectAccess** , en el **paso 3: servidores de infraestructura**, haga clic en **Editar**.

13. Haga clic en **siguiente** dos veces y, en la sección **Administración** , haga doble clic en el campo **servidores de administración** .

14. Escriba el **nombre de equipo** o la **Dirección** del servidor de CA que está configurado para emitir certificados OTP y haga clic en **Aceptar**.

15. En las ventanas **instalación de acceso remoto** , haga clic en **Finalizar**.

16. Haga clic en **Finalizar** en el **Asistente para el experto de DirectAccess**.

17. En el cuadro de diálogo **revisión de acceso remoto** , haga clic en **aplicar**, espere a que se actualice la Directiva de DirectAccess y haga clic en **cerrar**.

18. En la pantalla **Inicio** , escriba**powershell.exe**, haga clic con el botón derecho en **PowerShell**, haga clic en **Opciones avanzadas**y haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

19. En la ventana de Windows PowerShell, escriba **gpupdate/force** y presione Entrar.

Para configurar el acceso remoto para OTP mediante comandos de PowerShell:

![](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Comandos equivalentes** de Windows PowerShell Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Para configurar el acceso remoto para usar la autenticación en dos fases en una implementación que actualmente usa la autenticación de certificado de equipo:

```
Set-DAServer -UserAuthentication TwoFactor
```

Para configurar el acceso remoto para usar la autenticación de OTP con la siguiente configuración:

-   Un servidor OTP llamado OTP.corp.contoso.com.

-   Un servidor de CA denominado APP1. Corp. contoso. com\corp-APP1-CA1.

-   Una plantilla de certificado denominada DAOTPLogon usada para la inscripción de certificados que se emiten para la autenticación de OTP.

-   Una plantilla de certificado denominada DAOTPRA usada para inscribir el certificado de la autoridad de registro usado por el servidor de acceso remoto para firmar las solicitudes de inscripción de certificado de OTP.

```
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$
```

Después de ejecutar los comandos de PowerShell, complete los pasos 12-19 del procedimiento anterior para configurar el servidor de acceso remoto para que admita OTP.

> [!NOTE]
> Asegúrese de comprobar que ha aplicado la configuración de OTP en el servidor de acceso remoto antes de agregar un punto de entrada.

## <a name="33-smart-cards-for-additional-authorization"></a><a name="BKMK_Smartcard"></a>3,3 tarjetas inteligentes para la autorización adicional
En la página autenticación del paso 2 del Asistente para la instalación de acceso remoto, puede requerir el uso de tarjetas inteligentes para el acceso a la red interna. Cuando se selecciona esta opción, el Asistente para la instalación de acceso remoto configura la regla de seguridad de conexión IPsec para el túnel de intranet en el servidor de DirectAccess para requerir la autorización del modo de túnel con tarjetas inteligentes. La autorización del modo de túnel le permite especificar que solo los equipos o usuarios autorizados pueden establecer un túnel entrante.

Para usar tarjetas inteligentes con la autorización del modo de túnel IPsec para el túnel de intranet, debe implementar una infraestructura de clave pública (PKI) con infraestructura de tarjetas inteligentes.

Dado que los clientes de DirectAccess están usando tarjetas inteligentes para el acceso a la intranet, también puede usar la seguridad del mecanismo de autenticación, una característica de Windows Server 2008 R2, para controlar el acceso a los recursos, como archivos, carpetas e impresoras, en función de si el usuario ha iniciado sesión con un certificado basado en tarjeta inteligente. La garantía del mecanismo de autenticación requiere un nivel funcional de dominio de Windows Server 2008 R2.

### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Permitir el acceso a usuarios con tarjetas inteligentes inutilizables
Para permitir el acceso temporal a los usuarios con tarjetas inteligentes inutilizables, haga lo siguiente:

1.  Cree un grupo de seguridad de Active Directory para contener las cuentas de los usuarios que no pueden usar temporalmente sus tarjetas inteligentes.

2.  Para el objeto de directiva de grupo de servidor de DirectAccess, configure las opciones globales de IPsec para la autorización de túnel IPsec y agregue el grupo de seguridad Active Directory a la lista de usuarios autorizados.

Para conceder acceso a un usuario que no puede usar su tarjeta inteligente, agregue temporalmente su cuenta de usuario al grupo de seguridad de Active Directory. Quite la cuenta de usuario del grupo cuando la tarjeta inteligente sea utilizable.

### <a name="under-the-covers-smart-card-authorization"></a>En segundo plano: autorización mediante tarjeta inteligente
La autorización mediante tarjeta inteligente funciona habilitando la autorización de modo de túnel en la regla de seguridad de conexión del túnel de intranet del servidor de DirectAccess para un identificador de seguridad (SID) basado en Kerberos determinado. Para la autorización mediante tarjeta inteligente, es el SID conocido (S-1-5-65-1), que se asigna a los inicio de sesión basados en tarjeta inteligente. Este SID está presente en el token de Kerberos de un cliente de DirectAccess y se conoce como "este certificado de organización" cuando se configura en la configuración de autorización del modo de túnel IPsec global.

Al habilitar la autorización mediante tarjeta inteligente en el paso 2 del Asistente para configuración de DirectAccess, el Asistente para configuración de DirectAccess configura la configuración de autorización del modo de túnel IPsec global con este SID para el servidor de DirectAccess directiva de grupo objeto. Para ver esta configuración en el complemento Firewall de Windows con seguridad avanzada para el servidor de DirectAccess directiva de grupo objeto, haga lo siguiente:

1.  Haga clic con el botón secundario en Firewall de Windows con seguridad avanzada y, a continuación, haga clic en Propiedades.

2.  En la pestaña Configuración IPsec, en autorización de túnel IPsec, haga clic en personalizar.

3.  Haga clic en la pestaña usuarios. Debería ver "NT Authority\este Organization Certificate" como usuario autorizado.



