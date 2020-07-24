---
title: Solucionar problemas de VPN de Always On
description: En este tema se proporcionan instrucciones para comprobar y solucionar problemas de la implementación de VPN Always On en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: bbb614886099bf2adc1239a699ef8d904e71be7b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961787"
---
# <a name="troubleshoot-always-on-vpn"></a>Solucionar problemas de VPN de Always On 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si la instalación de la VPN Always On no puede conectar los clientes a la red interna, es probable que la causa sea un certificado VPN no válido, directivas NPS incorrectas o problemas con los scripts de implementación del cliente o en enrutamiento y acceso remoto. El primer paso para solucionar problemas y probar la conexión VPN es comprender los componentes principales de la infraestructura de VPN de Always On. 

Puede solucionar problemas de conexión de varias maneras. En el caso de problemas del lado cliente y solución de problemas generales, los registros de aplicaciones de los equipos cliente son invaluables. En el caso de problemas específicos de la autenticación, el registro de NPS en el servidor NPS puede ayudarle a determinar el origen del problema.

## <a name="error-codes"></a>Códigos de error

### <a name="error-code-800"></a>Código de error: 800

- **Descripción del error.** no se pudo establecer la conexión remota porque se produjo un error en los túneles VPN probados. Es posible que el servidor VPN no esté disponible. Si esta conexión está intentando usar un túnel L2TP/IPsec, es posible que los parámetros de seguridad necesarios para la negociación de IPsec no estén configurados correctamente.

- **Causa posible.** Este error se produce cuando el tipo de túnel VPN es **automático** y se produce un error en el intento de conexión para todos los túneles VPN.

- **Posibles soluciones:**

    - Si sabe qué túnel usar para su implementación, establezca el tipo de VPN en el tipo de túnel en cuestión en el lado cliente de VPN.

    - Al establecer una conexión VPN con un tipo de túnel determinado, la conexión seguirá produciendo un error, pero se producirá un error más específico del túnel (por ejemplo, "GRE bloqueado para PPTP").

    - Este error también se produce cuando no se puede establecer contacto con el servidor VPN o se produce un error en la conexión de túnel.

- **Asegúrese de que:**

    - Los puertos IKE (puertos UDP 500 y 4500) no se bloquean.

    - Los certificados correctos para IKE están presentes tanto en el cliente como en el servidor.

### <a name="error-code-809"></a>Código de error: 809

- **Descripción del error.**  No se pudo establecer la conexión de red entre el equipo y el servidor VPN porque el servidor remoto no responde. Esto puede deberse a que uno de los dispositivos de red (por ejemplo, firewalls, NAT, enrutadores) entre el equipo y el servidor remoto no está configurado para permitir conexiones VPN. Póngase en contacto con el administrador o con su proveedor de servicios para determinar qué dispositivo puede estar causando el problema.

- **Causa posible.** Este error se debe a la admisión de puertos UDP 500 o 4500 en el servidor VPN o el firewall.

- **Posible solución.** Asegúrese de que se permiten los puertos UDP 500 y 4500 a través de todos los firewalls entre el cliente y el servidor RRAS.

### <a name="error-code-812"></a>Código de error: 812

- **Descripción del error.** No se puede conectar a Always On VPN. se impidió la conexión debido a una directiva configurada en el servidor RAS/VPN. En concreto, el método de autenticación que el servidor usó para comprobar su nombre de usuario y contraseña puede no coincidir con el método de autenticación configurado en el perfil de conexión. Póngase en contacto con el administrador del servidor RAS y notifíquelo a este error.

- **Causas posibles:**

    - La causa habitual de este error es que el NPS ha especificado una condición de autenticación que el cliente no puede cumplir. Por ejemplo, el NPS puede especificar el uso de un certificado para proteger la conexión PEAP, pero el cliente está intentando usar EAP-MSCHAPv2.

    - El registro de eventos 20276 se registra en el visor de eventos cuando el valor del Protocolo de autenticación del servidor VPN basado en RRAS no coincide con el del equipo cliente de VPN.

- **Posible solución.** Asegúrese de que la configuración de cliente coincide con las condiciones especificadas en el servidor NPS.

### <a name="error-code-13806"></a>Código de error: 13806

- **Descripción del error.** IKE no encontró un certificado de equipo válido. Póngase en contacto con el administrador de seguridad de red sobre la instalación de un certificado válido en el almacén de certificados adecuado.

- **Causa posible.** Este error suele producirse cuando no hay ningún certificado de equipo o certificado de equipo raíz en el servidor VPN.

- **Posible solución.** Asegúrese de que los certificados descritos en esta implementación están instalados tanto en el equipo cliente como en el servidor VPN.

### <a name="error-code-13801"></a>Código de error: 13801

- **Descripción del error.** Las credenciales de autenticación IKE no son aceptables.

- **Causas posibles.** Este error suele producirse en uno de los siguientes casos:

    - El certificado de equipo usado para la validación de IKEv2 en el servidor RAS no tiene **autenticación de servidor** en **uso mejorado de clave**.

    - El certificado de equipo del servidor RAS ha expirado.

    - El certificado raíz para validar el certificado de servidor RAS no está presente en el equipo cliente.

    - El nombre del servidor VPN que se usa en el equipo cliente no coincide con el **subjectName** del certificado del servidor.

- **Posible solución.** Compruebe que el certificado de servidor incluye la **autenticación de servidor** en **uso mejorado de clave**. Compruebe que el certificado de servidor sigue siendo válido. Compruebe que la CA usada aparece en **entidades de certificación raíz de confianza** en el servidor RRAS. Compruebe que el cliente VPN se conecta mediante el FQDN del servidor VPN, tal como se muestra en el certificado del servidor VPN.

### <a name="error-code-0x80070040"></a>Código de error: 0x80070040

- **Descripción del error.** El certificado de servidor no tiene **autenticación de servidor** como una de sus entradas de uso de certificado.

- **Causa posible.** Este error puede producirse si no hay instalado ningún certificado de autenticación de servidor en el servidor RAS.

- **Posible solución.** Asegúrese de que el certificado de equipo que usa el servidor RAS para **IKEv2** tenga **autenticación de servidor** como una de las entradas de uso de certificado.

### <a name="error-code-0x800b0109"></a>Código de error: 0x800B0109

Por lo general, el equipo cliente de VPN se une al dominio basado en Active Directory. Si usa credenciales de dominio para iniciar sesión en el servidor VPN, el certificado se instala automáticamente en el almacén de entidades de certificación raíz de confianza. Sin embargo, si el equipo no está unido al dominio o si usa una cadena de certificados alternativa, puede experimentar este problema.

- **Descripción del error.** Una cadena de certificados procesada pero terminada en un certificado raíz en el que el proveedor de confianza no confía.

- **Causa posible.** Este error puede producirse si el certificado de CA raíz de confianza correspondiente no está instalado en el almacén de entidades de certificación raíz de confianza en el equipo cliente.

- **Posible solución.** Asegúrese de que el certificado raíz está instalado en el equipo cliente en el almacén de entidades de certificación raíz de confianza.

## <a name="logs"></a>Registros

### <a name="application-logs"></a>Registros de aplicación

Los registros de aplicaciones de los equipos cliente registran la mayor parte de los detalles de los eventos de conexión VPN.

Busque eventos de RasClient de código fuente. Todos los mensajes de error devuelven el código de error al final del mensaje. A continuación se detallan algunos de los códigos de error más comunes, pero una lista completa está disponible en los [códigos de error de enrutamiento y acceso remoto](/previous-versions//mt728163(v=technet.10)).

## <a name="nps-logs"></a>Registros de NPS

NPS crea y almacena los registros de cuentas de NPS. De forma predeterminada, estos datos se almacenan en los archivos de registro% SYSTEMROOT% \\ system32 \\ \\ en un archivo denominado en*xxxx*. txt, donde *xxxx* es la fecha en que se creó el archivo.

De forma predeterminada, estos registros están en formato de valores separados por comas, pero no incluyen una fila de encabezado. La fila de encabezado es:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si pega esta fila de encabezado como la primera línea del archivo de registro y, a continuación, importa el archivo en Microsoft Excel, las columnas se etiquetarán correctamente.

Los registros de NPS pueden ser útiles para diagnosticar problemas relacionados con la Directiva. Para obtener más información sobre los registros de NPS, consulte [interpretar los archivos de registro de formato de base de datos NPS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771748(v=ws.10)).

## <a name="vpn_profileps1-script-issues"></a>Problemas de script de VPN_Profile.ps1

Los problemas más comunes al ejecutar manualmente el script de VPN_ Profile.ps1 incluyen:

- ¿Usa una herramienta de conexión remota?  Asegúrese de no usar RDP u otro método de conexión remota, ya que no se usa la detección de inicio de sesión de usuario.

- ¿El usuario es administrador de ese equipo local?  Asegúrese de que, al ejecutar el script de VPN_Profile.ps1, el usuario tiene privilegios de administrador.

- ¿Tiene habilitadas las características de seguridad adicionales de PowerShell? Asegúrese de que la Directiva de ejecución de PowerShell no está bloqueando el script. Considere la posibilidad de desactivar el modo de lenguaje restringido, si está habilitado, antes de ejecutar el script. Puede activar el modo de lenguaje restringido después de que el script se complete correctamente.

## <a name="always-on-vpn-client-connection-issues"></a>Problemas de conexión de cliente VPN de Always On

Una pequeña configuración incorrecta puede provocar un error en la conexión del cliente y puede ser difícil encontrar la causa.  Un cliente VPN Always On pasa por varios pasos antes de establecer una conexión. Al solucionar problemas de conexión de cliente, recorra el proceso de eliminación con lo siguiente:

1. ¿La máquina de plantilla está conectada externamente? Un examen de **whatismyip** debe mostrar una dirección IP pública que no le pertenece.

2. ¿Se puede resolver el nombre del servidor VPN o de acceso remoto en una dirección IP? En el **Panel de control**  >  **redes** y **Internet**  >  **conexiones de red**de Internet, abra las propiedades del perfil de VPN. El valor de la pestaña **General** debe poder resolverse públicamente a través de DNS.

3. ¿Se puede tener acceso al servidor VPN desde una red externa? Considere la posibilidad de abrir el protocolo de mensajes de control de Internet (ICMP) a la interfaz externa y hacer ping en el nombre del cliente remoto. Después de que un ping se realice correctamente, puede quitar la regla de permiso de ICMP.

4. ¿Tiene configuradas correctamente las NIC internas y externas en el servidor VPN? ¿Están en subredes diferentes? ¿La NIC externa se conecta a la interfaz correcta en el Firewall?

5. ¿Se abren los puertos UDP 500 y 4500 desde el cliente a la interfaz externa del servidor VPN? Compruebe el firewall del cliente, el firewall del servidor y los firewalls de hardware. IPSEC usa el puerto UDP 500, por lo que debe asegurarse de que no tiene IPEC deshabilitado o bloqueado en cualquier lugar.

6. ¿Se produce un error de validación de certificado? Compruebe que el servidor NPS tenga un certificado de autenticación de servidor que pueda atender solicitudes IKE. Asegúrese de que tiene la dirección IP del servidor VPN correcta especificada como cliente NPS. Asegúrese de que está autenticando con PEAP y de que las propiedades de EAP protegido solo deben permitir la autenticación con un certificado. Puede comprobar los errores de autenticación en los registros de eventos NPS. Para obtener más información, consulte [instalar y configurar el servidor NPS](vpn-deploy-nps.md) .

7. ¿Se está conectando pero no tiene acceso a Internet o a la red local? Compruebe si hay problemas de configuración en los grupos de direcciones IP del servidor DHCP/VPN.

8. ¿Se conecta y tiene una dirección IP interna válida pero no tiene acceso a los recursos locales?  Compruebe que los clientes saben cómo llegar a esos recursos. Puede usar el servidor VPN para enrutar las solicitudes.

## <a name="azure-ad-conditional-access-connection-issues"></a>Problemas de conexión de acceso condicional de Azure AD

### <a name="oops---you-cant-get-to-this-yet"></a>¡ Vaya! no puede obtener esto todavía

- **Descripción del error.** Cuando no se cumple la Directiva de acceso condicional, se bloquea la conexión VPN, pero se conecta después de que el usuario seleccione **X** para cerrar el mensaje.  Al seleccionar **Aceptar** , se produce otro intento de autenticación, que finaliza en otro mensaje ". Estos eventos se registran en el registro de eventos operativos de AAD del cliente.

- **Causa posible**

  - El usuario tiene un certificado de autenticación de cliente válido en su almacén de certificados personal que no ha emitido Azure AD.

  - Falta la sección de Perfil de VPN \<TLSExtensions\> o no contiene las entradas de acceso ** \<EKUName\> condicional de AAD \</EKUName\> \<EKUOID\> 1.3.6.1.4.1.311.87</Ekuoid de \> \<EKUName> acceso condicional de AAD</ekuname \> \<EKUOID\> 1.3.6.1.4.1.311.87</ekuoid \> ** . Las \<EKUName> \<EKUOID> entradas y indican al cliente VPN qué certificado se debe recuperar del almacén de certificados del usuario al pasar el certificado al servidor VPN. Sin esto, el cliente VPN usa cualquier certificado de autenticación de cliente válido que se encuentra en el almacén de certificados del usuario y la autenticación se realiza correctamente. 

  - El servidor RADIUS (NPS) no se ha configurado para aceptar solo certificados de cliente que contengan el OID de **acceso condicional de AAD** .

- **Posible solución.** Para omitir este bucle, haga lo siguiente:

  1. En Windows PowerShell, ejecute el cmdlet **Get-WMIObject** para volcar la configuración del perfil de VPN. 
  2. Compruebe que **\<TLSExtensions>** existen las **\<EKUName>** secciones, y **\<EKUOID>** y que muestra el nombre y el OID correctos.
      
      ```powershell
      PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

      __GENUS                 : 2
      __CLASS                 : MDM_VPNv2_01
      __SUPERCLASS            :
      __DYNASTY               : MDM_VPNv2_01
      __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
      __PROPERTY_COUNT        : 10
      __DERIVATION            : {}
      __SERVER                : DERS2
      __NAMESPACE             : root\cimv2\mdm\dmmap
      __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                  Nv2"
      AlwaysOn                :
      ByPassForLocal          :
      DnsSuffix               :
      EdpModeId               :
      InstanceID              : AlwaysOnVPN
      LockDown                :
      ParentID                : ./Vendor/MSFT/VPNv2
      ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                  Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                  corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                  gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                  erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                  com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                  rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                  ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                  erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                  p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                  <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                  alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                  ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                  rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                  1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                  se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                  apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                  w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                  ions
                                  xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                  ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                  EKUName>AAD Conditional
                                  Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                  Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                  AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                  ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                  rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                  >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                  g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                  nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
      RememberCredentials     : False
      TrustedNetworkDetection :
      PSComputerName          : DERS2
      ```

  3. Para determinar si hay certificados válidos en el almacén de certificados del usuario, ejecute el comando **certutil** :

     ```powershell
     C:\>certutil -store -user My

      My "Personal"
      ================ Certificate 0 ================
      Serial Number: 32000000265259d0069fa6f205000000000026
      Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
       NotBefore: 12/8/2017 8:07 PM
       NotAfter: 12/8/2018 8:07 PM
      Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
      Certificate Template Name (Certificate Type): User
      Non-root Certificate
      Template: User
      Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
        Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
        Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
        Provider = Microsoft Enhanced Cryptographic Provider v1.0
      Encryption test passed
        
      ================ Certificate 1 ================
      Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
      Issuer: CN=Microsoft VPN root CA gen 1
       NotBefore: 12/8/2017 6:24 PM
       NotAfter: 12/8/2017 7:29 PM
      Subject: CN=WinFed@deverett.info
      Non-root Certificate
      Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
        Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
        Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
        Provider = Microsoft Software Key Storage Provider
      Private key is NOT exportable
      Encryption test passed
     ```
     >[!NOTE]
     >Si un certificado del emisor **CN = Microsoft VPN root CA gen 1** está presente en el almacén personal del usuario, pero el usuario obtuvo acceso seleccionando **X** para cerrar el mensaje de perdedor, recopile los registros de eventos de CAPI2 para comprobar que el certificado usado para autenticar sea un certificado de autenticación de cliente válido que no se haya emitido desde la CA raíz de VPN de Microsoft.

  4. Si existe un certificado de autenticación de cliente válido en el almacén personal del usuario, se produce un error en la conexión (como debería) después de que el usuario seleccione la **X** y si **\<TLSExtensions>** existen las secciones, y **\<EKUName>** **\<EKUOID>** y contiene la información correcta.
   
     Aparece un mensaje de error que indica "no se encontró un certificado que se pueda usar con el protocolo de autenticación extensible".

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>No se puede eliminar el certificado de la hoja de conectividad VPN

- **Descripción del error.** No se pueden eliminar los certificados de la hoja de conectividad VPN.

- **Causa posible.** El certificado se establece en **principal**.

- **Posible solución.**

    1. En la hoja conectividad VPN, seleccione el certificado.
    2. En **principal**, seleccione **no**y, a continuación, seleccione **Guardar**.
    3. En la hoja conectividad VPN, vuelva a seleccionar el certificado.
    4. Seleccione **Eliminar**.
