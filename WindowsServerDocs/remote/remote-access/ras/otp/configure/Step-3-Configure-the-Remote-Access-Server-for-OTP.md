---
title: Paso 3 configurar el servidor de acceso remoto de OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 093877657f19006bba2b80c10b92db1fb3b40fde
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280862"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Paso 3 configurar el servidor de acceso remoto de OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Una vez que se ha configurado el servidor RADIUS con tokens de distribución de software, están abiertos los puertos de comunicación, se ha creado un secreto compartido, se han creado las cuentas de usuario correspondientes a Active Directory en el servidor RADIUS y el servidor de acceso remoto tiene se ha configurado como un agente de autenticación RADIUS, el servidor de acceso remoto debe configurarse para admitir la OTP.  
  
|Tarea|Descripción|  
|----|--------|  
|[3.1 los usuarios exentos de la autenticación de OTP (opcional)](#BKMK_Exempt)|Si determinados usuarios quedarán excluidos de DirectAccess con autenticación OTP, a continuación, siga estos pasos preliminares.|  
|[3.2 configurar el servidor de acceso remoto para admitir la OTP](#BKMK_Config)|En el servidor de acceso remoto, actualice la configuración de acceso remoto para admitir la autenticación OTP en dos fases.|  
|[3.3 las tarjetas inteligentes para la autorización adicional](#BKMK_Smartcard)|Información adicional sobre el uso de tarjetas inteligentes.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3.1 los usuarios exentos de la autenticación de OTP (opcional)  
Si los usuarios específicos que deben excluirse de la autenticación de OTP, se deben realizar estos pasos antes de configurar el acceso remoto:  
  
> [!NOTE]  
> Debe esperar para la replicación entre dominios para completar al configurar el grupo de exención de OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Crear grupo de seguridad de exención de usuario  
  
1.  Crear un grupo de seguridad en Active Directory con el fin de exención de OTP.  
  
2.  Agregue todos los usuarios estén exentos de autenticación de OTP para el grupo de seguridad.  
  
    > [!NOTE]  
    > Asegúrese de incluir solo las cuentas de usuario y no cuentas de equipo, en el grupo de seguridad de exención de OTP.  
  
## <a name="BKMK_Config"></a>3.2 configurar el servidor de acceso remoto para admitir la OTP  
Para configurar el acceso remoto para usar la autenticación en dos fases y OTP con el servidor RADIUS y la implementación de certificados de las secciones anteriores, siga estos pasos:  
  
#### <a name="configure-remote-access-for-otp"></a>Configurar el acceso remoto para OTP  
  
1.  Abra **administración de acceso remoto** y haga clic en **configuración**.  
  
2.  En el **instalación de DirectAccess** ventana, en **paso 2: servidor de acceso remoto**, haga clic en **editar**.  
  
3.  Haga clic en **siguiente** tres veces y, en el **autenticación** sección seleccionar ambos **autenticación en dos fases** y **usar OTP**y asegúrese de que **usar certificados de equipo** está activada.  
  
    > [!NOTE]  
    > Cuando haya habilitado la OTP en el servidor de acceso remoto, si deshabilita la OTP anulando la selección de **usar OTP**, se van a desinstalar las extensiones ISAPI y CGI en el servidor.  
  
4.  Si se requiere la compatibilidad de Windows 7, seleccione el **los equipos cliente de habilitar Windows 7 se conecten mediante DirectAccess** casilla de verificación. Nota: Como se describe en la sección de planeación, los clientes de Windows 7 deben tener DCA 2.0 instalados para admitir DirectAccess con OTP.  
  
5.  Haz clic en **Siguiente**.  
  
6.  En el **servidor RADIUS de OTP** sección, haga doble clic en el espacio en blanco **nombre del servidor** campo.  
  
7.  En el **agregar un servidor RADIUS** cuadro de diálogo, escriba el nombre del servidor RADIUS en el **nombre del servidor** campo. Haga clic en **cambio** junto a la **secreto compartido** campo y escriba la misma contraseña que usó al configurar el servidor RADIUS en el **secreto nuevo** y  **Confirmar secreto nuevo** campos. Haga clic en **Aceptar** dos veces y haga clic en **siguiente**.  
  
    > [!NOTE]  
    > Si el servidor RADIUS está en un dominio diferente del servidor de acceso remoto, el **nombre del servidor** campo debe especificar el FQDN del servidor RADIUS.  
  
8.  En el **servidores de CA OTP** sección Seleccione los servidores de CA que se usa para la inscripción de certificados de autenticación de cliente OTP y haga clic en **agregar**. Haz clic en **Siguiente**.  
  
9. En el **plantillas de certificado de OTP** sección, haga clic **examinar** para seleccionar la plantilla de certificado usada para la inscripción de certificados emitidos para la autenticación de OTP.  
  
    > [!NOTE]  
    > Sin la opción "No incluyen información de revocación de certificados emitidos", se debe configurar la plantilla de certificado para OTP los certificados emitidos por la CA de empresa. Si se selecciona esta opción durante la creación de la plantilla de certificado, a continuación, los equipos cliente OTP se producirá un error al iniciar sesión correctamente.  
  
    Haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para inscribir el certificado utilizado por el servidor de acceso remoto para firmar solicitudes de inscripción de certificado OTP. Haga clic en **Aceptar**. Haz clic en **Siguiente**.  
  
10. Si es necesario, la exención de usuarios específicos de DirectAccess con OTP, a continuación, en el **exenciones de OTP** sección Seleccione **no requieren los usuarios del grupo de seguridad especificados para autenticar mediante autenticación en dos fases** . Haga clic en **grupo de seguridad** y seleccione el grupo de seguridad que se creó para exenciones de OTP.  
  
11. En el **instalación del servidor de acceso remoto** página haga clic en **finalizar**.  
  
12. En el **instalación de DirectAccess** ventana, en **paso 3: servidores de infraestructura**, haga clic en **editar**.  
  
13. Haga clic en **siguiente** dos veces y, en el **administración** sección haga doble clic en el **servidores de administración** campo.  
  
14. Escriba el **nombre_equipo** o **dirección** del servidor de CA que está configurado para emitir certificados OTP y haga clic en **Aceptar**.  
  
15. En el **instalación de acceso remoto** windows, haga clic en **finalizar**.  
  
16. Haga clic en **finalizar** en el **Asistente de DirectAccess experto**.  
  
17. En el **revisión de acceso remoto** haga clic en el cuadro de diálogo **aplicar**, espere para que se puede actualizar la directiva de DirectAccess y haga clic en **cerrar**.  
  
18. En el **iniciar** , escriba**powershell.exe**, haga clic en **powershell**, haga clic en **avanzadas**y haga clic en **ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
19. En la ventana de Windows PowerShell, escriba **gpupdate /force** y presione ENTRAR.  
  
Para configurar el acceso remoto para OTP mediante comandos de PowerShell:  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**comandos equivalentes de Windows PowerShell**  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para configurar el acceso remoto para usar la autenticación en dos fases en una implementación que actualmente usa la autenticación de certificado de equipo:  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Para configurar el acceso remoto para utilizar la autenticación de OTP con las siguientes opciones:  
  
-   Un servidor OTP denominado OTP.corp.contoso.com.  
  
-   Un servidor de CA denominado APP1.corp.contoso.com\corp-APP1-CA1.  
  
-   Una plantilla de certificado denominado DAOTPLogon utilizado para la inscripción de certificados emitidos para la autenticación de OTP.  
  
-   Una plantilla de certificado denominada DAOTPRA se usa para inscribir el certificado de autoridad de registro usado por el servidor de acceso remoto para firmar solicitudes de inscripción de certificado OTP.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Después de ejecutar los comandos de PowerShell, complete los pasos 12-19 del procedimiento anterior para configurar el servidor de acceso remoto para admitir la OTP.  
  
> [!NOTE]  
> Asegúrese de que ha aplicado la configuración de OTP en el servidor de acceso remoto antes de agregar un punto de entrada.  
  
## <a name="BKMK_Smartcard"></a>3.3 las tarjetas inteligentes para la autorización adicional  
En la página de autenticación de paso 2 del Asistente para instalación de acceso remoto, puede requerir el uso de tarjetas inteligentes para el acceso a la red interna. Cuando se selecciona esta opción, el Asistente para instalación de acceso remoto configura la regla de seguridad de conexión de IPsec para el túnel de intranet en el servidor de DirectAccess para requerir autorización del modo de túnel con tarjetas inteligentes. Autorización del modo de túnel permite especificar que sólo los equipos autorizados o los usuarios pueden establecer un túnel entrante.  
  
Para usar tarjetas inteligentes con la autorización del modo de túnel IPsec para el túnel de intranet, debe implementar una infraestructura de clave pública (PKI) con infraestructura de tarjetas inteligentes.  
  
Dado que los clientes de DirectAccess usa tarjetas inteligentes para el acceso a la intranet, también puede usar la comprobación del mecanismo de autenticación, una característica de Windows Server 2008 R2, para controlar el acceso a recursos, como archivos, carpetas e impresoras, en función de si el usuario ha iniciado sesión con un certificado basado en tarjeta inteligente. Comprobación del mecanismo de autenticación requiere un nivel funcional del dominio de Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Permitir el acceso a usuarios con tarjetas inteligentes inutilizables  
Para permitir el acceso temporal a los usuarios con tarjetas inteligentes inutilizables, haga lo siguiente:  
  
1.  Cree un grupo de seguridad de Active Directory para contener las cuentas de los usuarios que no pueden usar temporalmente sus tarjetas inteligentes.  
  
2.  Para el objeto de directiva de grupo del servidor de DirectAccess, configure las opciones globales de IPsec para la autorización de túnel IPsec y agregue el grupo de seguridad de Active Directory a la lista de los usuarios autorizados.  
  
Para conceder acceso a un usuario que no puede usar su tarjeta inteligente, agregue temporalmente su cuenta de usuario al grupo de seguridad de Active Directory. Quite la cuenta de usuario del grupo cuando la tarjeta inteligente sea utilizable.  
  
### <a name="under-the-covers-smart-card-authorization"></a>Información adicional: Autorización mediante tarjeta inteligente  
La autorización mediante tarjeta inteligente funciona habilitando la autorización de modo de túnel en la regla de seguridad de conexión del túnel de intranet del servidor de DirectAccess para un identificador de seguridad (SID) basado en Kerberos determinado. Para la autorización mediante tarjeta inteligente, es el SID conocido (S-1-5-65-1), que se asigna a los inicio de sesión basados en tarjeta inteligente. Este SID está presente en el token de Kerberos de un cliente de DirectAccess y se conoce como "Este certificado de organización" cuando se configura en el global de IPsec del túnel Configuración del modo de autorización.  
  
Al habilitar la autorización mediante tarjeta inteligente en el paso 2 del Asistente para la instalación de DirectAccess, el Asistente para configuración de DirectAccess configura la configuración de autorización del modo de túnel de IPsec global con este SID para el objeto de directiva de grupo del servidor de DirectAccess. Para ver esta configuración en el Firewall de Windows con complemento de seguridad avanzada para el objeto de directiva de grupo del servidor de DirectAccess, haga lo siguiente:  
  
1.  A la derecha, haga clic en Firewall de Windows con seguridad avanzada y, a continuación, haga clic en Propiedades.  
  
2.  En la pestaña Configuración de IPsec, en la autorización de túnel IPsec, haga clic en Personalizar.  
  
3.  Haga clic en la pestaña usuarios. Debería ver "NT authority\este certificado de la organización" como un usuario autorizado.  
  


