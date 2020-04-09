---
title: Implementar el acceso remoto con autenticación OTP
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 47a92db6c451b2e1e9bb44393ab987f242cc0ef5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858258"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Implementar el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y el servicio de enrutamiento y acceso remoto \(RRAS\) VPN en un solo rol de acceso remoto.   

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descripción del escenario  
En este escenario, se configura un servidor de acceso remoto con DirectAccess habilitado para autenticar a los usuarios del cliente de DirectAccess con dos\-factor de contraseña de un solo vez \(OTP\) la autenticación, además de las credenciales de Active Directory estándar.  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) debe implementarse antes de implementar OTP.  
  
-   Los clientes de Windows 7 deben usar DCA 2,0 para admitir OTP.  
  
-   OTP no admite cambiar el PIN.  
  
-   Hay que implementar una infraestructura de clave pública.  
  
    Para obtener más información, vea: [Minimódulo de la guía de laboratorio de pruebas: PKI básica para Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   No se admite el cambio de directivas fuera de la consola de administración de DirectAccess o de los cmdlets de Windows PowerShell.  
  
## <a name="in-this-scenario"></a>En este escenario  
El escenario de autenticación OTP incluye varios pasos:  
  
1.  [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Se debe implementar un solo servidor de acceso remoto antes de configurar OTP. La planificación e implementación de un solo servidor incluye la designación y configuración de una topología de red, la planificación e implementación de certificados, la configuración de DNS y Active Directory, la configuración del servidor de acceso remoto, la implementación de los clientes de DirectAccess y la preparación de los servicios de intranet.  
  
2.  [Planear el acceso remoto con autenticación OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Además de la planificación necesaria para un solo servidor, OTP requiere la planeación de una entidad de certificación de Microsoft \(CA\) y plantillas de certificado para OTP; y un servidor OTP habilitado para\-RADIUS. La planeación también puede incluir un requisito para los grupos de seguridad para excluir a usuarios específicos de la autenticación \(OTP o la tarjeta inteligente\). Para obtener información sobre la configuración de OTP en un entorno de varios\-bosques, consulte [configuración de una implementación de varios bosques](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configure DirectAccess con la autenticación OTP](/configure/Configure-RA-with-OTP-Authentication.md). La implementación de OTP consta de varios pasos de configuración, como la preparación de la infraestructura para la autenticación OTP, la configuración del servidor OTP, la configuración de las opciones de OTP en el servidor de acceso remoto y la actualización de la configuración de cliente de DirectAccess.  
  
4.  [Solución de problemas de una implementación de OTP] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Esta sección de solución de problemas describe una serie de los errores más comunes que se pueden producir al implementar el acceso remoto con la autenticación de OTP.  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas  
Aumentar la seguridad: el uso de OTP aumenta la seguridad de la implementación de DirectAccess. Un usuario requiere credenciales de OTP para obtener acceso a la red interna. Un usuario proporciona credenciales de OTP a través de las conexiones de área de trabajo disponibles en las conexiones de red en el equipo cliente de Windows 10 o Windows 8, o mediante el Asistente de conectividad de DirectAccess \(DCA\) en equipos cliente que ejecutan Windows 7. El proceso de autenticación de OTP funciona de la siguiente manera:  
  
1.  El cliente de DirectAccess entra en las credenciales de dominio para tener acceso a los servidores de infraestructura de DirectAccess \(a través del túnel de infraestructura\).  Si no se encuentra disponible ninguna conexión a la red interna, debido a un error específico de IKE, la conexión del área de trabajo del equipo de cliente notifica al usuario que se requieren credenciales. En los equipos cliente que ejecutan Windows 7, aparece un pop\-la solicitud de credenciales de tarjeta inteligente.  
  
2.  Una vez especificadas las credenciales de OTP, estas se envían a través de SSL al servidor de acceso remoto, junto con una solicitud de un certificado de inicio de sesión de tarjeta inteligente de corto\-término.  
  
3.  El servidor de acceso remoto inicia la validación de las credenciales de OTP con el servidor OTP basado en\-RADIUS.  
  
4.  Si se realiza correctamente, el servidor de acceso remoto firma la solicitud de certificado mediante su certificado de entidad de registro y la vuelve a enviar al equipo cliente de DirectAccess  
  
5.  El equipo cliente de DirectAccess reenvía la solicitud de certificado firmada a la CA y almacena el certificado inscrito para que lo use el SSP de Kerberos\/AP.  
  
6.  Con este certificado, el equipo cliente realiza una autenticación Kerberos de tarjeta inteligente estándar en forma transparente.  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  
  
|Característica de\/de roles|Compatibilidad con este escenario|  
|---------|-----------------|  
|*Rol de administración de acceso remoto*|El rol se instala y desinstala mediante la consola del Administrador del servidor. Este rol incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto, que antes eran un servicio de rol en los servicios de acceso y directivas de redes \(NPAS\) rol de servidor. El rol de acceso remoto consta de dos componentes:<p>1. los servicios de enrutamiento y acceso remoto \(RRAS\) VPN: DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />2. enrutamiento RRAS: las características de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<p>El rol de acceso remoto depende de las siguientes características del servidor:<p>-Internet Information Services \(servidor Web\) IIS: esta característica es necesaria para configurar el servidor de ubicación de red, para el uso de la autenticación OTP y para configurar el sondeo Web predeterminado.<br />-Windows Internal Database: se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<p>-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecute el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<p>La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<p>-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<p>Las dependencias incluyen:<p>-Consola de administración de directivas de grupo<br />-Kit de administración del administrador de conexiones RAS \(CMAK\)<br />-Windows PowerShell 3,0<br />-Infraestructura y herramientas de administración de gráficos|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Un equipo que cumpla los requisitos de hardware para Windows Server 2016 o Windows Server 2012.  
  
-   Para probar el escenario, se requiere al menos un equipo que ejecute Windows 10, Windows 8 o Windows 7 configurado como cliente de DirectAccess.  
  
-   Un servidor de OTP que admita PAP a través de RADIUS.  
  
-   Un token de software o hardware de OTP.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  
  
1.  Requisitos de software para la implementación de un solo servidor. Para obtener más información, vea [implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Además de los requisitos de software para un solo servidor, hay varios requisitos específicos de\-OTP:  
  
    1.  CA para la autenticación IPsec: en una implementación de OTP, DirectAccess debe implementarse mediante certificados de equipos de IPsec emitidos por una CA. La autenticación IPsec mediante el servidor de acceso remoto como un proxy Kerberos no se admite en una implementación de OTP. Se requiere una CA interna.  
  
    2.  CA para la autenticación de OTP: se requiere una entidad de certificación empresarial de Microsoft \(que se ejecute en Windows 2003 Server o posterior\) para emitir el certificado de cliente de OTP. Puede usarse la misma CA que se usa para emitir certificados para la autenticación IPsec. El servidores de CA tiene que estar accesible a través del primer túnel de infraestructura.  
  
    3.  Grupo de seguridad: para excluir a los usuarios de la autenticación segura, se requiere un grupo de seguridad de Active Directory que contenga estos usuarios.  
  
    4.  Requisitos de\-de cliente: para equipos cliente de Windows 10 y Windows 8, el Asistente de conectividad de red \(servicio NCA\) se usa para detectar si se requieren credenciales de OTP. Si es así, el administrador de medios de DirectAccess solicitará las credenciales.  NCA se incluye en el sistema operativo y no se requiere ninguna instalación o implementación. En el caso de los equipos cliente de Windows 7, se requiere el Asistente de conectividad de DirectAccess \(DCA\) 2,0. Esto se puede descargar en el [Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Observe lo siguiente:  
  
        1.  La autenticación de OTP puede usarse en paralelo con la tarjeta inteligente y Módulo de plataforma segura \(TPM\)la autenticación basada en \-. Habilitar la autenticación de OTP en la consola de Administración de acceso remoto también permite el uso de la autenticación de tarjeta inteligente.  
  
        2.  Durante la configuración de acceso remoto, los usuarios de un grupo de seguridad especificado pueden estar exentos de dos\-factor Authentication y, por tanto, autenticarse solo con el nombre de usuario\/contraseña.  
  
        3.  Los modos del nuevo PIN y el siguiente código de token de OTP no son compatibles  
  
        4.  En una implementación multisitio de acceso remoto, la configuración de OTP es global y se identifica para todos los puntos de entrada. Si se configuran varios servidores RADIUS o CA para OTP, se ordenan para cada servidor de acceso remoto de acuerdo con la disponibilidad y proximidad.  
  
        5.  Al configurar OTP en un entorno de bosque multi\-de acceso remoto, las CA de OTP solo deben ser del bosque de recursos y la inscripción de certificados se debe configurar en las confianzas de bosque. Si desea obtener más información, consulte el tema sobre [AD CS: inscripción del certificado entre bosques con Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Los usuarios que usan un token de OTP de KEY FOB deben insertar el PIN seguido por el código de token \(sin separadores\) en el cuadro de diálogo de OTP de DirectAccess. Los usuarios que están usando el token PIN PAD OTP solo deben insertar el código de token en el cuadro de diálogo.  
  
        7.  Cuando se habilita WEBDAV no se debe habilitar OTP.  
  
## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conocidos  
Los problemas que se mencionan a continuación son problemas conocidos de la configuración de un escenario de OTP:  
  
-   El acceso remoto usa un mecanismo de sondeo para comprobar la conectividad a los servidores OTP basados en\-RADIUS. En algunos casos esto puede provocar un error que se va a emitir en el servidor OTP. Para evitar este problema, haz lo siguiente en el servidor OTP:  
  
    -   Crea una cuenta de usuario que coincida con el nombre de usuario y la contraseña configurados en el servidor de acceso remoto para el mecanismo de sondeo. El nombre de usuario no debe definir un usuario de Active Directory.  
  
        De forma predeterminada, el nombre de usuario del servidor de acceso remoto es DAProbeUser y la contraseña es DAProbePass. Estos valores predeterminados se pueden modificar usando los siguientes valores en el registro del servidor de acceso remoto:  
  
        -   HKEY\_equipo LOCAL de\_\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_equipo LOCAL de\_\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Si cambias el certificado raíz de IPsec en una implementación de DirectAccess configurada y en ejecución, OTP dejará de funcionar. Para resolver este problema, en cada servidor de DirectAccess, en un símbolo del sistema de Windows PowerShell, ejecute el comando: `iisreset`  
  
