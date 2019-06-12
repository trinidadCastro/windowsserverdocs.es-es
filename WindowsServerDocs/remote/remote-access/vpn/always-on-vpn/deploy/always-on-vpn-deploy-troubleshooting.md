---
title: Solucionar problemas de VPN de Always On
description: Este tema proporciona instrucciones para comprobar y solucionar problemas de implementación de VPN de Always On en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d9e0efede39f5a8189dbb3d62033210c393c424d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749647"
---
# <a name="troubleshoot-always-on-vpn"></a>Solucionar problemas de VPN de Always On 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si la configuración de VPN de Always On no se puede conectar a los clientes a la red interna, la causa probable es que es un certificado VPN no válido, directivas NPS incorrectas o problemas con los scripts de implementación de cliente o de enrutamiento y acceso remoto. El primer paso para solucionar problemas y comprobar la conexión VPN es comprender los componentes principales de la infraestructura de VPN de Always On. 

Puede solucionar los problemas de conexión de varias maneras. Para problemas de lado cliente y solución de problemas generales, los registros de aplicaciones en los equipos cliente son un valor incalculable. Para problemas específicos de la autenticación, el registro NPS en el servidor NPS puede ayudarle a determinar el origen del problema.

## <a name="error-codes"></a>Códigos de error

### <a name="error-code-800"></a>Código de error: 800

- **Descripción del error.** La conexión remota no se realizó debido a un error de los túneles VPN probados. El servidor VPN podría estar inaccesible. Si esta conexión está intentando usar un túnel L2TP/IPsec, los parámetros de seguridad necesarios para podría no estar configurada correctamente la negociación de IPsec.

- **Causa posible.** Este error se produce cuando el tipo de túnel VPN es **automática** y se produce un error en el intento de conexión para todos los túneles VPN.

- **Posibles soluciones:**

    - Si sabe qué túnel que se usará para la implementación, establezca el tipo de VPN para ese tipo de túnel en concreto en el lado del cliente VPN.

    - Al realizar una conexión VPN con un tipo de túnel en concreto, la conexión aún se producirá un error, pero producirá un error más específico de túnel (por ejemplo, "GRE bloqueado para PPTP").

    - Este error también se produce cuando no se puede alcanzar el servidor VPN o se produce un error en la conexión de túnel.

- **Asegúrate:**

    - No se bloqueen IKE (puertos UDP 500 y 4500).

    - Los certificados correctos para IKE están presentes en el cliente y el servidor.

### <a name="error-code-809"></a>Código de error: 809

- **Descripción del error.**  No se pudo establecer la conexión de red entre el equipo y el servidor VPN porque el servidor remoto no responde. Esto podría deberse a que uno de los dispositivos de red (p. ej., enrutadores de firewalls, NAT,) entre el equipo y el servidor remoto no está configurado para permitir conexiones VPN. Póngase en contacto con el administrador o el proveedor de servicios para determinar qué dispositivo pueda estar causando el problema.

- **Causa posible.** Este error aparece cuando bloqueadas UDP 500 o 4500 puertos en el servidor VPN o el firewall.

- **Posible solución.** Asegúrese de que se permiten los puertos UDP 500 y 4500 a través de todos los firewalls entre el cliente y el servidor RRAS.

### <a name="error-code-812"></a>Código de error: 812

- **Descripción del error.** No se puede conectar a la VPN de Always On. Se ha impedido que la conexión debido a una directiva configurada en el servidor RAS/VPN. En concreto, el método de autenticación del servidor utilizado para comprobar su nombre de usuario y contraseña no puede coincidir con el método de autenticación configurado en el perfil de conexión. Póngase en contacto con el administrador del servidor RAS y notificar al jugador de este error.

- **Causas posibles:**

    - La causa habitual de este error es que el NPS ha especificado una condición de autenticación que el cliente no puede cumplir. Por ejemplo, NPS puede especificar el uso de un certificado para proteger la conexión de PEAP, pero el cliente está intentando usar EAP-MSCHAPv2.

    - Registro de eventos 20276 se registra en el Visor de eventos cuando la configuración de protocolo de autenticación de servidor VPN basada en RRAS no coincide con la del equipo cliente VPN.

- **Posible solución.** Asegúrese de que la configuración de cliente coincide con las condiciones que se especifican en el servidor NPS.

### <a name="error-code-13806"></a>Código de error: 13806

- **Descripción del error.** IKE no se pudo encontrar un certificado de equipo válido. Póngase en contacto con el Administrador de seguridad de red acerca de cómo instalar un certificado válido en el almacén de certificados adecuado.

- **Causa posible.** Este error suele producirse cuando ningún certificado de equipo o certificado de equipo raíz está presente en el servidor VPN.

- **Posible solución.** Asegúrese de que los certificados que se describen en esta implementación están instalados en el equipo cliente y el servidor VPN.

### <a name="error-code-13801"></a>Código de error: 13801

- **Descripción del error.** Las credenciales de autenticación de IKE son inaceptables.

- **Causas posibles.** Este error suele producirse en uno de los casos siguientes:

    - No tiene el certificado de equipo usado para la validación de IKEv2 en el servidor RAS **autenticación de servidor** en **uso mejorado de clave**.

    - Ha expirado el certificado de equipo en el servidor RAS.

    - El certificado raíz para validar el certificado de servidor RAS no está presente en el equipo cliente.

    - El nombre del servidor VPN usado en el equipo cliente no coincide con el **subjectName** del certificado del servidor.

- **Posible solución.** Compruebe que el certificado de servidor incluye **autenticación de servidor** en **uso mejorado de clave**. Compruebe que el certificado de servidor sigue siendo válido. Compruebe que aparece en la CA se usan **entidades emisoras raíz de confianza** en el servidor RRAS. Compruebe que el cliente VPN se conecta con el FQDN del servidor VPN, tal como se presenta en el certificado del servidor VPN.

### <a name="error-code-0x80070040"></a>Código de error: 0x80070040

- **Descripción del error.** El certificado de servidor no tiene **autenticación de servidor** como uno de sus entradas de uso del certificado.

- **Causa posible.** Este error puede producirse si no se instala ningún certificado de autenticación de servidor en el servidor RAS.

- **Posible solución.** Asegúrese de que el certificado de equipo usa el servidor RAS para **IKEv2** tiene **autenticación de servidor** como una de las entradas de uso del certificado.

### <a name="error-code-0x800b0109"></a>Código de error: 0x800B0109

Por lo general, el equipo cliente VPN está unido al dominio basada en Active Directory. Si usa las credenciales de dominio para iniciar sesión en el servidor VPN, el certificado se instala automáticamente en las entidades de certificación raíz de confianza almacenar. Sin embargo, si el equipo no está unido al dominio o si usa una cadena de certificados alternativo, puede experimentar este problema.

- **Descripción del error.** Una cadena de certificados ha procesado pero termina en un certificado raíz que no confía en el proveedor de confianza.

- **Causa posible.** Este error puede producirse si el certificado raíz de confianza adecuado CA no está instalado en las entidades de certificación raíz de confianza se almacena en el equipo cliente.

- **Posible solución.** Asegúrese de que el certificado raíz está instalado en el equipo cliente en el almacén entidades emisoras raíz de confianza.

## <a name="logs"></a>Registros

### <a name="application-logs"></a>Registros de aplicación

Los registros de aplicaciones en los equipos cliente registran la mayoría de los detalles de eventos de conexión de VPN de nivel superior.

Busque eventos de origen de cliente RAS. Todos los mensajes de error devuelven el código de error al final del mensaje. A continuación se detallan algunas de los códigos de error más comunes, pero está disponible en una lista completa [códigos de Error de acceso remoto y enrutamiento](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Registros NPS

NPS crea y almacena los registros de contabilidad de NPS. De forma predeterminada, estos se almacenan en % SYSTEMROOT %\\System32\\Logfiles\\ en un archivo denominado en*XXXX*.txt, donde *XXXX* es la fecha en que se creó el archivo.

De forma predeterminada, estos registros están en formato de valores separados por comas, pero no incluyen una fila de encabezado. La fila de encabezado es:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si pega esta fila de encabezado de la primera línea del archivo de registro, a continuación, importar el archivo en Microsoft Excel, las columnas se denominará correctamente.

Los registros NPS pueden ser útil para diagnosticar problemas relacionados con la directiva. Para obtener más información sobre los registros de NPS, consulte [interpretar los archivos de registro de formato de base de datos de NPS](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpnprofileps1-script-issues"></a>Problemas de la secuencia de comandos de VPN_Profile.ps1

Los problemas más comunes al ejecutar manualmente la secuencia de comandos del archivo Profile.ps1 VPN_ incluyen:

- ¿Usa una herramienta de conexión remota?  Asegúrese de que no se debe utilizar RDP u otro método de conexión remota, como confunden con la detección de inicio de sesión de usuario.

- ¿Es el usuario Administrador de la máquina local?  Asegúrese de que al ejecutar el script VPN_Profile.ps1 el usuario tiene privilegios de administrador.

- ¿Tienen características adicionales de seguridad de PowerShell habilitadas? Asegúrese de que la directiva de ejecución de PowerShell no está bloqueando la secuencia de comandos. Considere la posibilidad de desactivar el modo de lenguaje restringido, si se habilita, antes de ejecutar el script. Puede activar modo de lenguaje restringido después de que el script se complete correctamente.

## <a name="always-on-vpn-client-connection-issues"></a>Problemas de conexión de cliente de VPN de Always On

Una configuración incorrecta pequeño puede provocar un error en la conexión de cliente y puede resultar complicado para encontrar la causa.  Un cliente de VPN de Always On se pasa a través de varios pasos antes de establecer una conexión. Al solucionar problemas de conexión de cliente, vaya a través del proceso de eliminación con lo siguiente:

1. ¿Está conectada la máquina de plantilla externamente? Un **whatismyip** examen debe mostrar una dirección IP pública que no pertenecen a usted.

2. ¿Puede resolver el nombre del servidor VPN de acceso remoto a una dirección IP? En **Panel de Control** > **red** y **Internet** > **las conexiones de red**, abra las propiedades para el perfil de VPN. El valor de la **General** ficha debe poder resolverse públicamente a través de DNS.

3. ¿Se puede acceder al servidor VPN desde una red externa? Considere la posibilidad de abrir el protocolo de mensajes de Control de Internet (ICMP) a la interfaz externa y hacer ping en el nombre del cliente remoto. Después de un comando ping es correcto, puede quitar el ICMP regla de permiso.

4. ¿Tienen las NIC internas y externas en el servidor VPN configurado correctamente? ¿Están en subredes diferentes? ¿Se conecta la NIC externa a la interfaz correcta en el firewall?

5. ¿Son UDP 500 y 4500 puertos abierto desde el cliente a la interfaz externa del servidor VPN? Compruebe el firewall del cliente, firewall de servidor y los firewalls de hardware. IPSEC usa UDP puerto 500, por lo que debe asegurarse que no tienen IPEC deshabilitado o bloqueado en cualquier lugar.

6. ¿Es no superan la validación de certificados? Compruebe que el servidor NPS tiene un certificado de autenticación de servidor que puede atender solicitudes de IKE. Asegúrese de que tiene la dirección IP correcta de servidor VPN especificada como un cliente NPS. Asegúrese de que se está autenticando con PEAP, y las propiedades de EAP protegido solo deben permitir la autenticación con un certificado. Puede comprobar los registros de eventos NPS para errores de autenticación. Para obtener más información, consulte [instalar y configurar el servidor NPS](vpn-deploy-nps.md)

7. ¿Estás conectando pero no tienen acceso a la red local o Internet? Compruebe los grupos IP del servidor DHCP o VPN para problemas de configuración.

8. ¿Se conecta y tiene una dirección IP interna válida, pero no que no tienen acceso a los recursos locales?  Compruebe que los clientes sepan cómo acceder a esos recursos. Puede usar el servidor VPN para enrutar las solicitudes.

## <a name="azure-ad-conditional-access-connection-issues"></a>Problemas de conexión de acceso condicional de AD Azure

### <a name="oops---you-cant-get-to-this-yet"></a>Vaya - no se puede llegar a esto aún

- **Descripción del error.** Cuando la directiva de acceso condicional no se cumple, bloqueando la conexión VPN, pero se conecta después de que el usuario selecciona **X** para cerrar el mensaje.  Seleccionar **Aceptar** provoca otro intento de autenticación, lo que termina en otro mensaje "¡vaya!". Estos eventos se registran en el registro de eventos operativos de AAD del cliente.

- **Causa posible**

  - El usuario tiene un certificado de autenticación de cliente válido en su certificado Personal almacén que no fue emitido por Azure AD.

  - El perfil de VPN \<TLSExtensions\> sección es falta o no no contienen el **\<EKUName\>acceso condicional de AAD\</EKUName\> \< EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > acceso condicional de AAD < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** entradas. El \<EKUName > y \<EKUOID > entradas indican el cliente VPN qué certificado debe recuperar del almacén de certificados del usuario al pasar el certificado al servidor VPN. Sin esto, el cliente VPN usa cualquier certificado de autenticación de cliente válido se encuentra en el almacén de certificados del usuario y la autenticación se realiza correctamente. 

  - No se configuró el servidor RADIUS (NPS) para aceptar únicamente los certificados de cliente que contienen el **acceso condicional de AAD** OID.

- **Posible solución.** Para salir de este bucle, realice lo siguiente:

  1. En Windows PowerShell, ejecute el **Get-WmiObject** cmdlet para volcar la configuración del perfil VPN. 
  2. Compruebe que la  **\<TLSExtensions >** ,  **\<EKUName >** , y  **\<EKUOID >** secciones existe y se muestra el valor correcto nombre y OID.
      
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

  3. Para determinar si hay certificados válidos en el almacén de certificados del usuario, ejecute el **Certutil** comando:

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
     >Si un certificado de emisor **CN = gen. de entidad de certificación raíz de VPN de Microsoft 1** está presente en el almacén Personal del usuario, pero el usuario obtuviera acceso seleccionando **X** para cerrar el mensaje ¡vaya!, recopilar registros de eventos de CAPI2 para comprobar el certificado utilizado para autenticar era un certificado de autenticación de cliente válido que no se ha emitido desde la CA raíz de VPN de Microsoft.

  4. Si existe un certificado de autenticación de cliente válido en el almacén Personal del usuario, la conexión se produce un error (como debería) cuando el usuario selecciona el **X** y si el  **\<TLSExtensions >** ,  **\<EKUName >** , y  **\<EKUOID >** secciones existe y contiene la información correcta.
   
     Aparece un mensaje de error que dice "un certificado no se encontró que puede utilizarse con el protocolo de autenticación Extensible".

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>No se puede eliminar el certificado en la hoja de la conectividad VPN

- **Descripción del error.** No se puede eliminar los certificados en la hoja de la conectividad VPN.

- **Causa posible.** El certificado se establece en **principal**.

- **Posible solución.**

    1. En la hoja de la conectividad VPN, seleccione el certificado.
    2. En **principal**, seleccione **No**, a continuación, seleccione **guardar**.
    3. En la hoja de la conectividad VPN, seleccione el certificado nuevo.
    4. Seleccione **eliminar**.
