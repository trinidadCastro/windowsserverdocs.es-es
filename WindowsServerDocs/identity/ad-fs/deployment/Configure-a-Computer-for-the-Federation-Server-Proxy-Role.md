---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: "Configurar un equipo para el rol de Proxy del servidor de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar un equipo para el rol de Proxy del servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de configurar un equipo con los certificados necesarios y has instalado el servicio de rol de Proxy de federación de servicio, estás listo para configurar el equipo para convertirse en un proxy de federación de servidor. Puedes usar el siguiente procedimiento para que el equipo actúa la federación rol de servidor proxy.  
  
> [!IMPORTANT]  
> Antes de usar este procedimiento para configurar el equipo de federación servidor proxy, asegúrate de haber seguido los pasos de [lista de comprobación: configuración de un Proxy de servidor de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md) en el orden en que aparecen. Asegúrate de que se implemente ese servidor de federación de al menos una y que todas las necesarias credenciales para autorizar una configuración de proxy del servidor de federación se implementan. También debes configurar la capa de Sockets seguros \(SSL\) enlaces en el sitio Web predeterminado, o no se inicia este asistente. Todas estas tareas se deben completar antes de que este proxy del servidor de federación puede funcionar.  
  
Cuando termines de configurar el equipo, comprueba que el proxy de federación de servidor funciona según lo esperado. Para obtener más información, consulta [comprueba que una federación servidor Proxy es operativa](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Para configurar un equipo para el rol de proxy del servidor de federación  
  
1.  Hay dos formas de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar al asistente, realiza una de las siguientes acciones:  
  
    -   En la **inicio**, escriba**Asistente de configuración de Proxy de servidor de AD FS federación**, y, a continuación, presione ENTRAR.  
  
    -   En cualquier momento después de que el Asistente para la instalación se completa, abre el Explorador de Windows, navega hasta la **C:\\Windows\\ADFS** carpeta y, a continuación, clic double\ **FspConfigWizard.exe**.  
  
2.  Usar cualquiera de estos métodos, iniciar el asistente y, en la **bienvenida** página, haz clic en **siguiente**.  
  
3.  En la **especificar el nombre de servicio de federación** página, debajo **nombre de servicios de federación**, escribe el nombre que representa el servicio de federación para el que este equipo actuará en la función de proxy.  
  
4.  En función de los requisitos específicos de red, determina si debes usar un servidor proxy HTTP para reenviar las solicitudes al servicio de federación. Si es así, selecciona el **usar un servidor proxy HTTP al enviar solicitudes a este servicio federación** casilla en **dirección del servidor proxy HTTP** escribe la dirección del servidor proxy, haz clic en **Probar conexión** para comprobar la conectividad y, a continuación, haz clic en **siguiente**.  
  
5.  Cuando se te pida, especifica las credenciales que son necesarias establecer una relación de confianza entre este proxy del servidor de federación y el servicio de federación.  
  
    De manera predeterminada, solo la cuenta de servicio utilizada por el servicio de federación o un miembro del grupo local BUILTIN\\Administrators puede autorizar a un proxy de federación de servidor.  
  
6.  En la **listo para aplicar configuración** página, revisa los detalles. Si aparece la configuración sea correcta, haz clic en **siguiente** para empezar a configurar el equipo con esta configuración de proxy.  
  
7.  En la **resultados de la configuración** página, revisar los resultados. Cuando haya terminado de todos los pasos de configuración, haz clic en **cerrar** para salir del asistente.  
  
    No hay ningún \(MMC\) snap\ Microsoft Management Console-en que se usará para administrar proxys de servidor de federación. Para configurar los parámetros para cada uno de los proxys de servidor de federación de la organización, usar los cmdlets de Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurar un puerto TCP/IP TCP\ alternativo para operaciones de Proxy  
De manera predeterminada, el servicio de proxy del servidor de federación está configurado para usar el puerto TCP 443 para tráfico HTTPS y el puerto 80 para el tráfico HTTP para la comunicación con el servidor de federación. Para configurar diferentes puertos, como el puerto TCP 444 para HTTPS y el puerto 81 para HTTP, deben llevar a cabo las siguientes tareas.  
  
> [!NOTE]  
> Si deseas implementar inicialmente AD FS para funcionar con los puertos TCP/IP TCP\ alternativos, primero debes modificar puertos en los enlaces de protocolo IIS para HTTP y HTTPS en los equipos de proxy de servidor de federación y el servidor de federación. Esto debe suceder antes de ejecutar a los asistentes de configuración de AD FS para la configuración inicial. Si configuras Internet Information Services \(IIS\) en primer lugar, la configuración del puerto TCP/IP TCP\ alternativa se detectan cuando la configuración basada en sistema\ se produce dentro de AD FS y el procedimiento siguiente no es necesario. Si quieres cambiar la configuración del puerto más adelante, actualizar los enlaces de protocolo IIS primero y, a continuación, usa el siguiente procedimiento para actualizar la configuración de puerto correctamente. Para obtener más información acerca de cómo modificar los enlaces de IIS, consulte [artículo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) en Microsoft Knowledge Base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar los puertos TCP/IP TCP\ alternativos para el proxy de servidor de federación usar  
  
1.  Configurar el servidor de federación para utilizar los puertos no predeterminados.  
  
    Para ello, especifica el número de puerto no predeterminado agregando con la *HttpsPort* y *HttpPort* opciones como parte de la **Set ADFSProperties** cmdlet. Por ejemplo, para configurar estos puertos, usa los siguientes comandos en la sesión de Windows PowerShell en el equipo de servidor de federación:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurar el proxy de servidor de federación para usar el puerto.  
  
    Para ello, especifica el número de puerto no predeterminado agregando con la *HttpsPort* y *HttpPort* opciones como parte de la **Set ADFSProxyProperties** cmdlet. Por ejemplo, para configurar estos puertos, usa los siguientes comandos en la sesión de Windows PowerShell en el equipo de servidor de federación:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Direcciones URL de extremo no están habilitadas de manera predeterminada para los servicios de federación de servidor proxy. Si está configurando una nueva instalación de servidor de federación, primero debes habilitar extremos de servicios de federación servidor proxy. Por ejemplo, se supone que todos los extremos de la que hace referencia en el ejemplo de este procedimiento se han habilitado ellos para el proxy en la administración de AD FS snap\ en para seleccionarlos y, a continuación, seleccionando **activar mediante proxy**.  
  
3.  Actualizar la instalación de IIS en el proxy de federación de servidor para que \(SAML\) lenguaje de marcado de aserción de seguridad y confianza WS\ extremos están configurados para reflejar el número de puerto actualizado. Para ello, puedes usar el Bloc de notas para modificar las siguientes acciones en el archivo Web.config, que se encuentra en systemdrive%\\inetpub\\adfs\\ls\\ en el equipo de proxy del servidor de federación. Por ejemplo, suponiendo que dispone de un servidor de federación denominado sts1.contoso.com y el nuevo número de puerto es 444, buscar para abrir el archivo de Web.config en el Bloc de notas en el equipo de proxy del servidor de federación, busque la siguiente sección, modificar el número de puerto como se resalta a continuación y, a continuación, guardar y salir el Bloc de notas.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Agregar la cuenta de usuario de servicios de federación servidor proxy para el control de acceso de lista \(ACL\) para las direcciones URL de extremo relacionados. Por ejemplo, si el número de puerto es 1234 y la cuenta de usuario que se usa para ejecutar el servicio de anuncios FSfederation servidor proxy en la cuenta de servicio de red en built\, escribe el siguiente comando en un símbolo del sistema:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Los comandos anteriores se deben ejecutar en el servidor de federación y los equipos de proxy del servidor de federación.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

