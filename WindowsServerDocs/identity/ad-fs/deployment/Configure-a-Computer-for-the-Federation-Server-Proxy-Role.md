---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurar un equipo para el rol de servidor proxy de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861806"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar un equipo para el rol de servidor proxy de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de configurar un equipo con los certificados necesarios y ha instalado el servicio de rol Proxy de servicio de federación, está listo para configurar el equipo para convertirse en un servidor proxy de federación. Puede realizar el siguiente procedimiento para que el equipo actúe como servidor proxy de federación.  
  
> [!IMPORTANT]  
> Antes de usar este procedimiento para configurar el equipo del servidor proxy de federación, asegúrese de que ha seguido los pasos descritos en [lista de comprobación: Configuración de seguridad de un servidor Proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md) en el orden en que aparecen. Asegúrese de que se han implementado al menos un servidor de federación y todas las credenciales necesarias para la autorización de la configuración del servidor proxy de federación. También debe configurar la capa de Sockets seguros \(SSL\) enlaces en el sitio Web predeterminado o este asistente no se iniciarán. Todas estas tareas deben completarse para que el servidor proxy de federación funcione.  
  
Una vez que haya configurado el equipo, compruebe que el servidor proxy de federación funciona del modo esperado. Para obtener más información, consulta [Comprobar que un servidor proxy de federación está operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Cómo configurar un equipo para el rol de servidor proxy de federación  
  
1.  Hay dos maneras de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   En el **iniciar** , escriba**Asistente para la configuración de AD FS Federation Server Proxy**, y, a continuación, presione ENTRAR.  
  
    -   En cualquier momento después de que el Asistente para la instalación está completa, abra el Explorador de Windows, navegue hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, doble\-haga clic en **FspConfigWizard.exe**.  
  
2.  Independientemente del método que elijas, inicia el asistente y en la **Página principal** haz clic en **Siguiente**.  
  
3.  En la página **Especificar el nombre del Servicio de federación**, en **Nombre del Servicio de federación**, escribe el nombre que represente al Servicio de federación para el cual este equipo actuará con el rol de proxy.  
  
4.  En función de los requisitos específicos de tu red, determina si tendrás que utilizar un servidor proxy HTTP para reenviar las solicitudes al servicio de federación. En caso de que así sea, activa la casilla **Usar un servidor proxy HTTP al enviar solicitudes a este servicio de federación** , escribe la dirección del servidor proxy en **Dirección del servidor proxy HTTP** , haz clic en **Probar conexión** para comprobar la conectividad y haz clic en **Siguiente**.  
  
5.  Cuando se te pida, especifica las credenciales necesarias para establecer una relación de confianza entre este servidor proxy de federación y el servicio de federación.  
  
    De forma predeterminada, solo la cuenta de servicio utilizada por el servicio de federación o un miembro de la local BUILTIN\\grupo Administradores puede autorizar a un servidor proxy de federación.  
  
6.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si los ajustes son correctos, haz clic en **Siguiente** para empezar a configurar el equipo con esa configuración de proxy.  
  
7.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
    No hay ninguna consola de administración de Microsoft \(MMC\) ajustar\-en que se usará para administrar los proxys de servidor de federación. Para configurar los valores para cada uno de los proxys de servidor de federación de su organización, use los cmdlets de Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurar un TCP alternativo\/puerto IP para las operaciones de Proxy  
De forma predeterminada, el servicio de proxy de servidor de federación está configurado para usar el puerto TCP 443 para el tráfico HTTPS y el puerto 80 para el tráfico HTTP para la comunicación con el servidor de federación. Si deseas configurar puertos diferentes, como el puerto TCP 444 para HTTPS y el puerto 81 para HTTP, debes llevar a cabo las siguientes tareas.  
  
> [!NOTE]  
> Si planea implementar inicialmente AD FS para operar con TCP alternativo\/puertos IP, primero debe modificar los puertos en los enlaces de protocolo IIS para HTTP y HTTPS en el servidor de federación y equipos de servidores proxy de federación. Esto debe ocurrir antes de ejecutar al Asistente para configuración de AD FS para la configuración inicial. Si configura Internet Information Services \(IIS\) en primer lugar, el TCP alternativo\/detectados al Asistente para configuración de puerto IP\-en función de configuración que se produce dentro de AD FS y el procedimiento siguiente no es es necesario. Si deseas cambiar más adelante la configuración del puerto, actualiza primero los enlaces de protocolo IIS y utiliza el siguiente procedimiento para actualizar correctamente la configuración del puerto. Para obtener más información acerca de cómo editar los enlaces de IIS, consulte [artículo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) en Microsoft Knowledge Base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar TCP alternativo\/puertos IP para el servidor proxy de federación usar  
  
1.  Configura el servidor de federación para que utilice los puertos no predeterminados.  
  
    Para ello, especifique el número de puerto no predeterminado incluyéndolo con las *HttpsPort* y *HttpPort* opciones como parte de la **establecer\-ADFSProperties** cmdlet. Por ejemplo, para configurar estos puertos, use los siguientes comandos en la sesión de Windows PowerShell en el equipo del servidor de federación:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurar el servidor proxy de federación para utilizar el puerto no predeterminado.  
  
    Para ello, especifique el número de puerto no predeterminado incluyéndolo con las *HttpsPort* y *HttpPort* opciones como parte de la **establecer\-ADFSProxyProperties** cmdlet. Por ejemplo, para configurar estos puertos, use los siguientes comandos en la sesión de Windows PowerShell en el equipo del servidor de federación:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Direcciones URL de extremo no están habilitadas de forma predeterminada para el servicio de proxy de servidor de federación. Si está configurando una nueva instalación de servidor de federación, primero debe habilitar extremos de servicio de proxy de servidor de federación. Por ejemplo, se supone que para todos los puntos de conexión que hace referencia en el ejemplo de este procedimiento se ha habilitado para proxy seleccionándolos en el complemento Administración de AD FS\-en y, a continuación, seleccione **habilitar en proxy**.  
  
3.  Actualizar la instalación de IIS en el servidor proxy de federación por lo que ese lenguaje de marcado de aserción de seguridad \(SAML\) y WS\-confiar en los puntos de conexión se configuren para reflejar el número de puerto actualizado. Para ello, puede usar el Bloc de notas para modificar lo siguiente en el archivo Web.config, que se encuentra en % systemdrive\\inetpub\\adfs\\ls\\ en el equipo del servidor proxy de federación. Por ejemplo, suponiendo que tiene un servidor de federación llamado sts1.contoso.com y el número de puerto nuevo es 444, busque y abra el archivo Web.config en el Bloc de notas en el equipo del servidor proxy de federación, busque la sección siguiente, modificar el número de puerto como a continuación y, a continuación, guardar y salir del Bloc de notas.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Agregue la cuenta de usuario de servicio de federación servidor proxy a la lista de control de acceso \(ACL\) para las direcciones URL de punto de conexión relacionado. Por ejemplo, si el número de puerto es 1234 y la cuenta de usuario que se usa para ejecutar el AD FSfederation servidor proxy de servicio bajo está integrado\-en la cuenta de servicio de red, escriba el siguiente comando en un símbolo del sistema:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    En el servidor de federación y los equipos de servidores proxy de federación, se deben ejecutar los comandos anteriores.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

