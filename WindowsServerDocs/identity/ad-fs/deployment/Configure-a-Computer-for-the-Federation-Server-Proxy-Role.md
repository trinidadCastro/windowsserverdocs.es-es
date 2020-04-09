---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurar un equipo para el rol de servidor proxy de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: a8bfe21f50a68edfcdbc7c937dc914ff1e1d94c3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854908"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar un equipo para el rol de servidor proxy de federación

Después de configurar un equipo con los certificados necesarios y de instalar el servicio de rol de Proxy de Servicio de federación, está listo para configurar el equipo para que se convierta en un servidor proxy de Federación. Puede realizar el siguiente procedimiento para que el equipo actúe como servidor proxy de federación.  
  
> [!IMPORTANT]  
> Antes de usar este procedimiento para configurar el equipo de servidor proxy de Federación, asegúrese de que ha seguido todos los pasos de [lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md) en el orden en que aparecen. Asegúrese de que se han implementado al menos un servidor de federación y todas las credenciales necesarias para la autorización de la configuración del servidor proxy de federación. También debe configurar Capa de sockets seguros \(SSL\) enlaces en el sitio web predeterminado, o este asistente no se iniciará. Todas estas tareas deben completarse para que el servidor proxy de federación funcione.  
  
Una vez que haya configurado el equipo, compruebe que el servidor proxy de federación funciona del modo esperado. Para obtener más información, consulta [Comprobar que un servidor proxy de federación está operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Para configurar un equipo para el rol de servidor proxy de federación  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, siga uno de estos procedimientos:  
  
    -   En la pantalla **Inicio** , escriba**AD FS Asistente para configuración de servidor proxy de Federación**y, a continuación, presione Entrar.  
  
    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C:\\Windows\\ADFS** y, a continuación, haga doble\-haga clic en **FspConfigWizard. exe**.  
  
2.  Mediante cualquiera de los métodos, inicie el asistente y, en la página **Bienvenida**, haga clic en **Siguiente**.  
  
3.  En la página **Especificar el nombre del Servicio de federación**, en **Nombre del Servicio de federación**, escriba el nombre que representa el Servicio de federación para el que este equipo actuará en el rol de servidor proxy.  
  
4.  En función de sus requisitos de red específicos, determine si tendrá que usar un servidor proxy HTTP para desviar las solicitudes al servicio de federación. Si lo necesita, seleccione la casilla de verificación **Usar un servidor proxy HTTP al enviar solicitudes a este Servicio de federación**, en **Dirección del servidor proxy HTTP**, escriba la dirección del servidor proxy, haga clic en **Probar conexión** para comprobar la conectividad y luego haga clic en **Siguiente**.  
  
5.  Cuando se le indique, especifique las credenciales necesarias para establecer una relación de confianza entre este servidor proxy de federación y el servicio de federación.  
  
    De forma predeterminada, solo la cuenta de servicio usada por el Servicio de federación o un miembro del grupo local Builtin\\administradores puede autorizar un servidor proxy de Federación.  
  
6.  En la página **Listo para aplicar configuración**, revise los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar este equipo con estos valores de servidor proxy.  
  
7.  En la página **Resultados de configuración**, revise los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
    No hay ningún complemento Microsoft Management Console \(MMC\) Snap\-en para usar para administrar los servidores proxy de Federación. Para configurar los valores de cada uno de los servidores proxy de Federación de su organización, use los cmdlets de Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configuración de un puerto TCP\/IP alternativo para operaciones de proxy  
De forma predeterminada, el servicio de servidor proxy de Federación está configurado para usar el puerto TCP 443 para el tráfico HTTPS y el puerto 80 para el tráfico HTTP para la comunicación con el servidor de Federación. Si deseas configurar puertos diferentes, como el puerto TCP 444 para HTTPS y el puerto 81 para HTTP, debes llevar a cabo las siguientes tareas.  
  
> [!NOTE]  
> Si piensa implementar AD FS para que funcione en puertos TCP\/IP alternativos, primero debe modificar los puertos de los enlaces de protocolo IIS para HTTP y HTTPS tanto en el servidor de Federación como en los equipos de servidor proxy de Federación. Esto debe ocurrir antes de ejecutar el Asistente para configuración de AD FS para la configuración inicial. Si configura Internet Information Services \(IIS\) primero, se detecta la configuración de puerto TCP\/IP alternativa cuando se produce la configuración basada\-del asistente en AD FS y el procedimiento siguiente no es necesario. Si deseas cambiar más adelante la configuración del puerto, actualiza primero los enlaces de protocolo IIS y utiliza el siguiente procedimiento para actualizar correctamente la configuración del puerto. Para obtener más información acerca de la edición de enlaces de IIS, vea el [artículo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) en Microsoft Knowledge base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar puertos TCP\/IP alternativos para que los use el servidor proxy de Federación  
  
1.  Configura el servidor de federación para que utilice los puertos no predeterminados.  
  
    Para ello, especifique el número de puerto no predeterminado incluyéndolo con las opciones *HttpsPort* y *HttpPort* como parte del cmdlet **set\-ADFSProperties** . Por ejemplo, para configurar estos puertos, use los siguientes comandos en la sesión de Windows PowerShell en el equipo del servidor de Federación:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configure el servidor proxy de Federación para que use el puerto no predeterminado.  
  
    Para ello, especifique el número de puerto no predeterminado incluyéndolo con las opciones *HttpsPort* y *HttpPort* como parte del cmdlet **set\-ADFSProxyProperties** . Por ejemplo, para configurar estos puertos, use los siguientes comandos en la sesión de Windows PowerShell en el equipo del servidor de Federación:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Las direcciones URL del extremo no están habilitadas de forma predeterminada para el servicio de servidor proxy de Federación. Si está configurando una nueva instalación de servidor de Federación, primero debe habilitar los puntos de conexión de servicio de servidor proxy de Federación. Por ejemplo, se supone que para todos los extremos a los que hace referencia el ejemplo de este procedimiento, puede seleccionarlos en el\-de administración de AD FS en y seleccionar **Habilitar en proxy**.  
  
3.  Actualice la instalación de IIS en el servidor proxy de Federación de modo que Lenguaje de marcado de aserción de seguridad \(SAML\) y los puntos de conexión de confianza WS\-estén configurados para reflejar el número de Puerto actualizado. Para ello, puede utilizar el Bloc de notas para modificar lo siguiente en el archivo Web. config, que se encuentra en SystemDrive%\\Inetpub\\ADFS\\LS\\ en el equipo del servidor proxy de Federación. Por ejemplo, suponiendo que tiene un servidor de Federación llamado sts1.contoso.com y el nuevo número de puerto es 444, busque y abra el archivo Web. config en el Bloc de notas en el equipo del servidor proxy de Federación, busque la siguiente sección, modifique el número de puerto como se indica a continuación y, a continuación, guarde y salga del Bloc de notas.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Agregue la cuenta de usuario del servicio de servidor proxy de Federación a la lista de control de acceso \(ACL\) para las direcciones URL de extremo relacionadas. Por ejemplo, si el número de puerto es 1234 y la cuenta de usuario que se usa para ejecutar el servicio de proxy de servidor de AD FSfederation en es el\-integrado en la cuenta de servicio de red, escriba el siguiente comando en un símbolo del sistema:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Los comandos anteriores deben ejecutarse tanto en el servidor de Federación como en los equipos de servidor proxy de Federación.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

