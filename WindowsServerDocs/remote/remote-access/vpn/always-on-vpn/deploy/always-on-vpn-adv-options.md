---
title: Características avanzadas de VPN de Always On
description: Más allá del escenario de implementación que se proporciona en esta implementación, puede agregar otras características avanzadas de VPN para mejorar la seguridad y la disponibilidad de la conexión VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/2019
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: fd3ebf1acada5de1ed3f4b14ddeca761728ffa75
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965547"
---
# <a name="advanced-features-of-always-on-vpn"></a>Características avanzadas de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Más información acerca de la tecnología VPN Always On](../always-on-vpn-technology-overview.md)
- [**Siguiente:** Inicio de la planeación de la Always On la implementación de VPN](always-on-vpn-deploy-planning.md)

Además de los escenarios de implementación que se proporcionan, puede agregar otras características avanzadas de VPN para mejorar la seguridad y la disponibilidad de la conexión VPN. Por ejemplo, el servidor VPN puede usar estas características para asegurarse de que el cliente que se conecta es correcto antes de permitir una conexión.

## <a name="high-availability"></a>Alta disponibilidad

Las siguientes son opciones adicionales para la alta disponibilidad.

|Opción  |Descripción  |
|---------|---------|
|Resistencia del servidor y equilibrio de carga     |En entornos que requieren alta disponibilidad o que admiten un gran número de solicitudes, puede aumentar el rendimiento y la resistencia del acceso remoto mediante el equilibrio de carga entre varios servidores que ejecutan el servidor de directivas de redes (NPS) y habilitar la agrupación en clústeres del servidor de acceso remoto.<p>Documentos relacionados:<ul><li>[Equilibrio de carga del servidor proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implementación del acceso remoto en un clúster](../../../ras/cluster/deploy-remote-access-in-cluster.md)</li></ul>        |
|Resistencia de sitios geográficos     |En el caso de la geolocalización basada en IP, puede usar Traffic Manager global con DNS en Windows Server 2016. Para un equilibrio de carga geográfico más sólido, puede usar soluciones globales de equilibrio de carga del servidor, como Microsoft Azure Traffic Manager.<p>Documentos relacionados:<ul><li>[¿Qué es Traffic Manager?](/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Autenticación avanzada

A continuación se muestran opciones adicionales para la autenticación.

|Opción  |Descripción  |
|---------|---------|
|Windows Hello para empresas     |En Windows 10, Windows Hello para empresas reemplaza las contraseñas al proporcionar una sólida autenticación en dos fases en equipos y dispositivos móviles. Esta autenticación consta de un nuevo tipo de credencial de usuario que está asociada a un dispositivo y utiliza un número de identificación personal (PIN) o biométrico.<p>El cliente VPN de Windows 10 es compatible con Windows Hello para empresas. Una vez que el usuario inicia sesión con un gesto, la conexión VPN usa el certificado de Windows Hello para empresas para la autenticación basada en certificados.<p>Documentos relacionados:<ul><li>[Windows Hello para empresas](/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Caso práctico técnico: [Habilitar el acceso remoto con Windows Hello para empresas en Windows 10](/previous-versions//mt728163(v=technet.10))</li></ul>         |
|Autenticación multifactor (MFA) de Azure     |Azure MFA tiene versiones locales y en la nube que se pueden integrar con el mecanismo de autenticación de VPN de Windows.<p>Para obtener más información sobre cómo funciona este mecanismo, consulte [integración de la autenticación RADIUS con Azure servidor multi-factor Authentication](/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Características avanzadas de VPN

A continuación se muestran opciones adicionales para las características avanzadas.

|Opción  |Descripción  |
|---------|---------|
|Filtrado de tráfico     |Si tiene que aplicar la elección de las aplicaciones a las que pueden acceder los clientes VPN, puede habilitar los filtros de tráfico de VPN.<p>Para obtener más información, consulte [características de seguridad de VPN](/windows/access-protection/vpn/vpn-security-features).         |
|VPN activada por aplicaciones     |Puede configurar los perfiles de VPN para que se conecten automáticamente cuando se inicien determinadas aplicaciones o tipos de aplicaciones.<p>Para obtener más información sobre esta y otras opciones de activación, consulte [Opciones de perfil desencadenado automáticamente por VPN](/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acceso condicional de VPN   |El acceso condicional y el cumplimiento de dispositivos pueden requerir que los dispositivos administrados cumplan los estándares antes de poder conectarse a la VPN. Una de las características avanzadas del acceso condicional de VPN permite restringir las conexiones VPN solo a aquellas en las que el certificado de autenticación de cliente contiene el OID de "acceso condicional de AAD" de **1.3.6.1.4.1.311.87**.<p>Para restringir las conexiones VPN, debe hacer lo siguiente:<ol><li>En el servidor NPS, abra el complemento **servidor de directivas de redes** .</li><li>Expanda **directivas**  >  **directivas de red**.</li><li>Haga clic con el botón secundario en la Directiva de red **conexiones de red privada virtual (VPN)** y seleccione **propiedades**.</li><li>Seleccione la pestaña **Configuración**.</li><li>Seleccione **específico del proveedor**y, a continuación, seleccione **Agregar**.</li><li>Seleccione la opción **permitir certificado-OID** y, a continuación, seleccione **Agregar**.</li><li>Pegue el OID de acceso condicional de AAD de **1.3.6.1.4.1.311.87** como valor de atributo y, a continuación, seleccione **Aceptar** dos veces.</li><li>Seleccione **cerrar**y, a continuación, seleccione **aplicar**.<p>Después de seguir estos pasos, cuando los clientes de VPN intenten conectarse con cualquier certificado que no sea el certificado de la nube de corta duración, se produce un error en la conexión.</li></ol>Para obtener más información sobre el acceso condicional, vea [VPN y acceso condicional](/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Bloquear clientes VPN que usan certificados revocados
  
Después de instalar las actualizaciones, el servidor RRAS puede aplicar la revocación de certificados para las VPN que usan IKEv2 y certificados de equipo para la autenticación, como las VPN de túnel de dispositivo siempre activada. Esto significa que para dichas VPN, el servidor RRAS puede denegar conexiones VPN a los clientes que intenten usar un certificado revocado.

**Disponibilidad**

En la tabla siguiente se enumeran las versiones de que contienen las correcciones para cada versión de Windows.

|Versión del sistema operativo |Release  |
|---------|---------|
|Windows Server, versión 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server, versión 1809  |[KB4505658](https://support.microsoft.com/help/4505658/windows-10-update-kb4505658)  |
|Windows Server, versión 1803  |[KB4507466](https://support.microsoft.com/help/4507466/windows-10-update-kb4507466)  |
|Windows Server, versión 1709  |[KB4507465](https://support.microsoft.com/help/4507465/windows-10-update-kb4507465)  |
|Windows Server 2016, versión 1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |

**Cómo configurar los requisitos previos** 

1. Instale las actualizaciones de Windows a medida que estén disponibles.
1. Asegúrese de que todos los certificados de cliente VPN y de servidor RRAS que use tengan entradas CDP y de que el servidor RRAS pueda llegar a las CRL respectivas.
1. En el servidor RRAS, use el cmdlet **set-VpnAuthProtocol** de PowerShell para configurar el parámetro **RootCertificateNameToAccept** .<p>
   En el ejemplo siguiente se enumeran los comandos para hacerlo. En el ejemplo, **CN = entidad de certificación raíz de Contoso** representa el nombre distintivo de la entidad de certificación raíz. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Cómo configurar el servidor RRAS para exigir la revocación de certificados para las conexiones VPN basadas en certificados de equipo IKEv2**

1. En una ventana del símbolo del sistema, ejecute el siguiente comando: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Reinicie el servicio de **enrutamiento y acceso remoto** .
  
Para deshabilitar la revocación de certificados para estas conexiones VPN, establezca **CertAuthFlags = 2** o quite el valor **CertAuthFlags** y, a continuación, reinicie el servicio de **enrutamiento y acceso remoto** . 

**Revocar un certificado de cliente VPN para una conexión VPN basada en un certificado de equipo IKEv2**
1. Revocar el certificado de cliente de VPN de la entidad de certificación.
1. Publicar una nueva CRL de la entidad de certificación.
1. En el servidor RRAS, abra una ventana de símbolo del sistema administrativo y, a continuación, ejecute los siguientes comandos:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Cómo comprobar que la revocación de certificados para conexiones VPN basadas en certificados de equipo IKEv2 funciona**  
>[!Note]  
> Antes de usar este procedimiento, asegúrese de habilitar el registro de eventos de CAPI2 operativo.
1. Siga los pasos anteriores para revocar un certificado de cliente de VPN.
1. Intente conectarse a la VPN mediante el uso de un cliente que tenga el certificado revocado. El servidor RRAS debe rechazar la conexión y mostrar un mensaje como "las credenciales de autenticación IKE son inaceptables".
1. En el servidor RRAS, abra Visor de eventos y navegue hasta **aplicaciones y servicios registros/Microsoft/Windows/CAPI2**. 
1. Busque un evento que tenga la siguiente información:
   * Nombre de registro: **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID. de evento: **41** 
   * El evento contiene el siguiente texto: **Subject = "*Client FQDN*"** (el*FQDN del cliente* representa el nombre de dominio completo del cliente que tiene el certificado revocado). 

   El **<Result>** campo de los datos de evento debe incluir **el certificado revocado**. Por ejemplo, vea los siguientes extractos de un evento:
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="https://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>Protección adicional

### <a name="trusted-platform-module-tpm-key-attestation"></a>Atestación de clave de Módulo de plataforma segura (TPM)

Un certificado de usuario que tenga una clave atestiguada por TPM proporciona un mayor control de seguridad, al que se hace una copia de seguridad mediante la no exportabilidad, el antimartillo y el aislamiento de claves que proporciona el TPM.

Para obtener más información sobre la atestación de clave de TPM en Windows 10, consulte [atestación de clave de TPM](../../../../../identity/ad-ds/manage/component-updates/tpm-key-attestation.md).

## <a name="next-step"></a>Paso siguiente

[Empezar a planear la implementación de VPN Always on](always-on-vpn-deploy-planning.md): antes de instalar el rol de servidor de acceso remoto en el equipo que va a usar como servidor VPN, realice las siguientes tareas. Después de la planeación adecuada, puede implementar Always On VPN y, opcionalmente, configurar el acceso condicional para la conectividad VPN mediante Azure AD.  

## <a name="related-topics"></a>Temas relacionados
- [Equilibrio de carga del servidor proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): clientes servicio de autenticación remota telefónica de usuario (RADIUS), que son servidores de acceso a la red como servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, crean solicitudes de conexión y las envían a servidores RADIUS como NPS. En algunos casos, un servidor NPS podría recibir demasiadas solicitudes de conexión al mismo tiempo, lo que produce un rendimiento degradado o una sobrecarga.

- [Información general de Traffic Manager](/azure/traffic-manager/traffic-manager-overview): en este tema se proporciona información general sobre Azure Traffic Manager, que permite controlar la distribución del tráfico de usuario para los puntos de conexión de servicio. Traffic Manager usa el sistema de nombres de dominio (DNS) para dirigir las solicitudes del cliente al punto de conexión más adecuado en función de un método de enrutamiento del tráfico y el estado de los puntos de conexión. 

- [Windows Hello para empresas](/windows/access-protection/hello-for-business/hello-identity-verification): en este tema se proporcionan los requisitos previos, como implementaciones solo en la nube e implementaciones híbridas.  En este tema también se enumeran las preguntas más frecuentes sobre Windows Hello para empresas.

- [Caso práctico técnico: habilitación del acceso remoto con Windows Hello para empresas en Windows 10](/previous-versions//mt728163(v=technet.10)): en este caso práctico, aprenderá cómo Microsoft implementa el acceso remoto con Windows Hello para empresas.  Windows Hello para empresas es un enfoque de autenticación basado en certificado o clave pública/privada dirigido a organizaciones y consumidores que no se limita a las contraseñas. Esta forma de autenticación se basa en credenciales de par de claves que pueden reemplazar a las contraseñas y que resisten las infracciones, los robos y las suplantaciones de identidad. 

- [Integración de la autenticación RADIUS con azure servidor multi-factor Authentication](/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): en este tema se explica cómo agregar y configurar una autenticación de cliente RADIUS con Azure servidor multi-factor Authentication. RADIUS es un protocolo estándar para aceptar las solicitudes de autenticación y procesar estas solicitudes. Servidor Azure Multi-Factor Authentication puede actuar como servidor RADIUS. 

- [Características de seguridad de VPN](/windows/access-protection/vpn/vpn-security-features): en este tema se proporcionan instrucciones de seguridad de VPN para la integración de VPN, Windows Information Protection (WIP) con VPN y los filtros de tráfico. 

- [Opciones de perfil desencadenado automáticamente por VPN](/windows/access-protection/vpn/vpn-auto-trigger-profile): en este tema se proporcionan opciones de Perfil de desencadenamiento automático de VPN, como un desencadenador de aplicación, un desencadenador basado en nombre y un Always on.

- [VPN y acceso condicional](/windows/access-protection/vpn/vpn-conditional-access): en este tema se proporciona información general sobre la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento de dispositivos para clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

- [Atestación de clave de TPM](../../../../../identity/ad-ds/manage/component-updates/tpm-key-attestation.md): en este tema se proporciona información general sobre módulo de plataforma segura (TPM) y los pasos para implementar la atestación de clave de TPM. También puede encontrar información y pasos de solución de problemas para resolver problemas.
