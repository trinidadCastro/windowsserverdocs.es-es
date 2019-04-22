---
title: Implementar el acceso remoto con autenticación OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811996"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Implementar el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y enrutamiento y servicio de acceso remoto \(RRAS\) VPN en un solo rol de acceso remoto.   

## <a name="BKMK_OVER"></a>Descripción del escenario  
En este escenario de acceso remoto server con DirectAccess habilitado se configura para autenticar usuarios de clientes de DirectAccess con dos\-una contraseña de factor \(OTP\) autenticación, además de activo estándar Credenciales de directorio.  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deben implementarse antes de implementar OTP.  
  
-   Los clientes de Windows 7 debe usar DCA 2.0 para admitir la OTP.  
  
-   OTP no admite cambiar el PIN.  
  
-   Hay que implementar una infraestructura de clave pública.  
  
    Para obtener más información, consulta: [Minimódulo de guía de laboratorio de pruebas: PKI básica para Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   No se admite cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  
  
## <a name="in-this-scenario"></a>En este escenario  
El escenario de autenticación OTP incluye varios pasos:  
  
1.  [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Debe implementarse un solo servidor de acceso remoto antes de configurar la OTP. La planificación e implementación de un solo servidor incluye la designación y configuración de una topología de red, la planificación e implementación de certificados, la configuración de DNS y Active Directory, la configuración del servidor de acceso remoto, la implementación de los clientes de DirectAccess y la preparación de los servicios de intranet.  
  
2.  [Planeación de acceso remoto con autenticación OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Además de la planeación necesaria para un único servidor, la OTP requiere planeamiento de una entidad de certificación de Microsoft \(CA\) y plantillas de certificado para OTP; y un radio\-servidor OTP habilitado. Planeación también podría incluir un requisito para grupos de seguridad eximir a usuarios específicos strong \(OTP o tarjeta inteligente\) autenticación. Para obtener información sobre la configuración de OTP en un multi\-del bosque de entorno, consulte [configurar una implementación de varios bosques](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurar DirectAccess con autenticación OTP](/configure/Configure-RA-with-OTP-Authentication.md). Implementación de OTP consta de una serie de pasos de configuración, incluida la preparación de la infraestructura para la autenticación de OTP, la configuración del servidor OTP, configuración de OTP en el servidor de acceso remoto y actualizando la configuración de cliente de DirectAccess.  
  
4.  [Solución de problemas de una implementación de OTP] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). En esta sección de solución de problemas describe una serie de los errores más comunes que pueden producirse al implementar el acceso remoto con autenticación OTP.  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Aumento de la seguridad mediante OTP aumenta la seguridad de la implementación de DirectAccess. Un usuario requiere credenciales de OTP para obtener acceso a la red interna. Un usuario proporciona credenciales OTP a través de las conexiones de área de trabajo disponibles en las conexiones de red en el equipo cliente de Windows 10 o Windows 8, o mediante el Asistente de conectividad de DirectAccess \(DCA\) en equipos cliente que ejecutan Windows 7. El proceso de autenticación de OTP funciona de la siguiente manera:  
  
1.  El cliente de DirectAccess ingresa credenciales de dominio para tener acceso a los servidores de infraestructura de DirectAccess \(a través del túnel de infraestructura\).  Si no se encuentra disponible ninguna conexión a la red interna, debido a un error específico de IKE, la conexión del área de trabajo del equipo de cliente notifica al usuario que se requieren credenciales. En los equipos cliente ejecutan Windows 7, un punto de presencia\-seguridad solicita credenciales de tarjeta inteligente aparece.  
  
2.  Después de escribir las credenciales de OTP, se envían a través de SSL para el servidor de acceso remoto, junto con una solicitud durante un breve\-certificado de inicio de sesión de tarjeta inteligente de término.  
  
3.  El servidor de acceso remoto inicia la validación de las credenciales de OTP con el radio\-en función de servidor de OTP.  
  
4.  Si se realiza correctamente, el servidor de acceso remoto firma la solicitud de certificado mediante su certificado de entidad de registro y la vuelve a enviar al equipo cliente de DirectAccess  
  
5.  El equipo cliente de DirectAccess reenvía la solicitud de certificado firmada a la CA y almacena el certificado inscrito para su uso por el SSP de Kerberos\/PA.  
  
6.  Con este certificado, el equipo cliente realiza una autenticación Kerberos de tarjeta inteligente estándar en forma transparente.  
  
## <a name="BKMK_NEW"></a>Roles y características que se incluyen en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  
  
|Rol\/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|*Rol de administración de acceso remoto*|El rol se instala y desinstala mediante la consola del Administrador del servidor. Este rol incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2 como enrutamiento y los servicios de acceso remoto que antes era un servicio de rol en la directiva de red y servicios de acceso \(NPAS\) server rol. El rol de acceso remoto consta de dos componentes:<br /><br />1.  DirectAccess y enrutamiento y servicios de acceso remoto \(RRAS\) VPN de DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />2.  Las características de enrutamiento RRAS de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />El rol de acceso remoto depende de las siguientes características del servidor:<br /><br />-Internet Information Services \(IIS\) Web Server: esta característica es necesaria para configurar el servidor de ubicación de red, utilizar la autenticación de OTP y configurar el sondeo web predeterminado.<br />-Windows Database-Used interno para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecuta el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />: Consola de administración de directivas de grupo<br />-Kit de administración de administrador de conexiones de RAS \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infraestructura y herramientas de administración gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Un equipo que cumpla los requisitos de hardware para Windows Server 2016 o Windows Server 2012.  
  
-   Para probar el escenario, se requiere al menos un equipo con Windows 10, Windows 8 o 7 de Windows configurado como un cliente de DirectAccess.  
  
-   Un servidor de OTP que admita PAP a través de RADIUS.  
  
-   Un token de software o hardware de OTP.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  
  
1.  Requisitos de software para la implementación de un solo servidor. Para obtener más información, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Además de los requisitos de software para un único servidor hay una serie de OTP\-requisitos específicos:  
  
    1.  Entidad emisora de certificados para IPsec autenticación en una implementación de OTP, que DirectAccess debe implementarse mediante IPsec máquinas certificados emitidos por una entidad de certificación. La autenticación IPsec mediante el servidor de acceso remoto como un proxy Kerberos no se admite en una implementación de OTP. Se requiere una CA interna.  
  
    2.  CA para OTP autenticación-A Microsoft Enterprise CA \(en Windows 2003 Server o una versión posterior\) es necesaria para emitir el certificado de cliente OTP. Puede usarse la misma CA que se usa para emitir certificados para la autenticación IPsec. El servidores de CA tiene que estar accesible a través del primer túnel de infraestructura.  
  
    3.  Seguridad al grupo usuarios exentos de autenticación sólida, un grupo de seguridad de Active Directory que contenga estos usuarios se requiere.  
  
    4.  Cliente\-requisitos-para Windows 10 y Windows 8 equipos cliente, el Asistente de conectividad de red \(NCA\) servicio se utiliza para detectar si se necesitan credenciales OTP. Si es así, el Administrador de medios de DirectAccess solicitará las credenciales.  NCA se incluye en el sistema operativo y no se requiere ninguna instalación o implementación. Para equipos cliente de Windows 7, el Asistente de conectividad de DirectAccess \(DCA\) 2.0 es necesario. Esto se puede descargar en el [Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Tenga en cuenta lo siguiente:  
  
        1.  Autenticación de OTP puede usarse en paralelo con tarjeta inteligente y el módulo de plataforma segura \(TPM\)\-la autenticación basada en. Habilitar la autenticación de OTP en la consola de Administración de acceso remoto también permite el uso de la autenticación de tarjeta inteligente.  
  
        2.  Durante los usuarios de la configuración de acceso remoto de una seguridad especificada grupo puede estar exento de dos\-factor de autenticación y así autenticar con el nombre de usuario\/contraseña únicamente.  
  
        3.  Los modos del nuevo PIN y el siguiente código de token de OTP no son compatibles  
  
        4.  En una implementación multisitio de acceso remoto, la configuración de OTP es global y se identifica para todos los puntos de entrada. Si se configuran varios servidores RADIUS o CA para OTP, se ordenan para cada servidor de acceso remoto de acuerdo con la disponibilidad y proximidad.  
  
        5.  Cuando se configura OTP en un acceso remoto de varios\-las CA OTP deben corresponder solo al bosque de recursos de entorno de bosque, y se debe configurar la inscripción de certificados a través de confianzas de bosque. Para obtener más información, consulte [AD CS: Inscripción de certificados entre bosques con Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Los usuarios que usan un token KEY FOB OTP deben insertar el código PIN seguido por el código de token \(sin ningún separador\) en el cuadro de diálogo de OTP de DirectAccess. Los usuarios que están usando el token PIN PAD OTP solo deben insertar el código de token en el cuadro de diálogo.  
  
        7.  Cuando se habilita WEBDAV no se debe habilitar OTP.  
  
## <a name="KnownIssues"></a>Problemas conocidos  
Los problemas que se mencionan a continuación son problemas conocidos de la configuración de un escenario de OTP:  
  
-   Acceso remoto usa un mecanismo de sondeo para comprobar la conectividad con RADIUS\-servidores OTP basados en. En algunos casos esto puede provocar un error que se va a emitir en el servidor OTP. Para evitar este problema, haz lo siguiente en el servidor OTP:  
  
    -   Crea una cuenta de usuario que coincida con el nombre de usuario y la contraseña configurados en el servidor de acceso remoto para el mecanismo de sondeo. El nombre de usuario no debe definir un usuario de Active Directory.  
  
        De forma predeterminada, el nombre de usuario del servidor de acceso remoto es DAProbeUser y la contraseña es DAProbePass. Estos valores predeterminados se pueden modificar usando los siguientes valores en el registro del servidor de acceso remoto:  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_máquina\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Si cambias el certificado raíz de IPsec en una implementación de DirectAccess configurada y en ejecución, OTP dejará de funcionar. Para resolver este problema, en cada servidor de DirectAccess, en un símbolo del sistema de Windows PowerShell, ejecute el comando: `iisreset`  
  
