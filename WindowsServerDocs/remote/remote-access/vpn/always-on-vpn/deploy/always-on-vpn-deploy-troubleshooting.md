---
title: Solucionar problemas de la VPN de Always On
description: En este tema proporciona instrucciones para comprobar y solucionar problemas de implementación de VPN siempre activada en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067512"
---
# Solucionar problemas de la VPN de Always On 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si se produce un error en la configuración de VPN siempre activada conectar los clientes a la red interna, la causa es probable que un certificado VPN no válido, las directivas NPS incorrectas o problemas con los scripts de implementación de cliente o en Enrutamiento y acceso remoto. El primer paso para solucionar problemas y probar la conexión VPN es comprender los componentes principales de la infraestructura de VPN siempre activada. 

Puedes solucionar problemas de conexión de varias maneras. Para los problemas del cliente y solución de problemas generales, los registros de aplicaciones en los equipos cliente son muy útil. Para los problemas de autenticación específicas, el registro NPS en el servidor NPS puede ayudarte a determinar el origen del problema.


## Códigos de error


### Código de error: 800

-   **Descripción del error.** No se realizó la conexión remota porque no se pudo los intentos túneles de VPN. El servidor VPN podría ser inaccesible. Si esta conexión está intentando usar un túnel de IPsec/L2TP, los parámetros de seguridad necesarias para IPsec negociación no esté configurada correctamente.


-   **Causa posible.** Este error se produce cuando el tipo de túnel VPN es **automática** y se produce un error en el intento de conexión para todos los túneles VPN.

-   **Posibles soluciones:**

    -   Si sabes qué túnel que se usará para la implementación, Establece el tipo de VPN en ese tipo de túnel particular en el lado de cliente VPN.

    -   Al realizar una conexión VPN con un tipo de túnel en concreto, todavía se producirá un error de la conexión, pero producirá un error de túnel más específico (por ejemplo, "GRE bloqueado para PPTP").

    -   Este error también se produce cuando no se puede alcanzar el servidor VPN o se produce un error en la conexión de túnel.

-   **Asegúrate:**

    -   Puertos IKE (UDP ports500 y 4500) no se bloquean.

    -   Los certificados correctos para IKE están presentes en el cliente y el servidor.

### Código de error: 809

-   **Descripción del error.**  No se pudo establecer la conexión de red entre el equipo y el servidor VPN porque el servidor remoto no responde. Esto puede deberse a uno de los dispositivos de red \ (por ejemplo, firewalls, NAT, routers\) entre el equipo y el servidor remoto no está configurado para permitir conexiones VPN. Ponte en contacto con el administrador o el proveedor de servicios para determinar qué dispositivo puede provocar el problema.

-   **Causa posible.** Este error se debe a bloqueados UDP 500 o 4500 puertos en el servidor VPN o el firewall.

-   **Posibles soluciones.** Asegúrate de que se permiten 4500 y UDP ports500 a través de todos los firewalls entre el cliente y el servidor RRAS. 
    
    
### Código de error: 812

-   **Descripción del error.** No se puede conectar a la VPN siempre activada. La conexión que no se pudo debido a una directiva configurada en el servidor VPN de RAS. Específicamente, el método de autenticación que usa el servidor para comprobar su nombre de usuario y contraseña puede no coincidir con el método de autenticación configurado en el perfil de conexión. Ponte en contacto con el administrador del servidor RAS y notificar compartiendo de este error.

-   **Causas posibles:**

    -  La causa habitual de este error es que el NPS ha especificado una condición de autenticación que el cliente no puede cumplir. Por ejemplo, el NPS puede especificar el uso de un certificado para proteger la conexión de PEAP, pero el cliente está intentando usar EAP-MSCHAPv2.

    -  Registro de eventos 20276 se registra en el Visor de eventos cuando la configuración de protocolo de autenticación de servidor VPN basado en RRAS no coincide con la del equipo cliente VPN.

-   **Posibles soluciones.** Asegúrate de que la configuración de cliente que coincida con las condiciones que se especifican en el servidor NPS.


### Código de error: 13806

-   **Descripción del error.** No se pudo encontrar un certificado de máquina válido IKE. Ponte en contacto con el Administrador de seguridad de red sobre la instalación de un certificado válido en el almacén de certificados adecuado.

-   **Causa posible.** Este error suele ocurrir cuando no hay ningún certificado de máquina o certificado raíz de la máquina está presente en el servidor VPN.

-   **Posibles soluciones.** Asegúrate de que los certificados descritos en esta implementación están instalados en el equipo cliente y el servidor VPN.

### Código de error: 13801

-   **Descripción del error.** Las credenciales de autenticación de IKE son inaceptables.

-   **Causas posibles.** Este error suele ocurrir en uno de los siguientes casos:

    -   El certificado de equipo que se usa para la validación de IKEv2 en el servidor RAS no tiene el método de **Autenticación de servidor** en **Uso mejorado de claves**.

    -   El certificado de equipo en el servidor RAS ha expirado.

    -   El certificado raíz para validar el certificado de servidor RAS no está presente en el equipo cliente.

    -   El nombre del servidor VPN usado en el equipo cliente no coincide con el **subjectName** del certificado del servidor.

-   **Posibles soluciones.** Comprueba que el certificado de servidor incluye el método de **Autenticación de servidor** en **Uso mejorado de claves**. Comprueba que el certificado de servidor sigue siendo válido. Comprueba que la entidad de certificación usado aparece en **Entidades de certificación raíz de confianza** en el servidor RRAS. Comprueba que el cliente VPN se conecta mediante el FQDN del servidor VPN como se muestra en el certificado del servidor VPN.


### Código de error: 0x80070040

-   **Descripción del error.** El certificado de servidor no tiene la **Autenticación de servidor** como uno de sus entradas de uso de certificados.

-   **Causa posible.** Este error puede producirse si no está instalado ningún certificado de autenticación de servidor en el servidor RAS.

-   **Posibles soluciones.** Asegúrate de que el certificado de máquina que usa el servidor RAS para **IKEv2** tiene **Autenticación de servidor** como una de las entradas de uso de certificados.

### Código de error: 0x800B0109

Por lo general, el equipo cliente VPN está unido al dominio basada en Active Directory. Si usas las credenciales de dominio para iniciar sesión en el servidor VPN, el certificado se instala automáticamente en las entidades de certificación raíz de confianza de la tienda. Sin embargo, si el equipo no está unido al dominio o si usas una cadena de certificados alternativos, puede experimentar este problema.

-   **Descripción del error.** Una cadena de certificados procesado pero termina en un certificado raíz que no confía en el proveedor de confianza.

-   **Causa posible.** Este error puede producirse si no está instalado el certificado raíz de confianza adecuada entidad emisora de certificados en las entidades de certificación raíz de confianza en el equipo cliente de la tienda.

-   **Posibles soluciones.** Asegúrate de que el certificado raíz está instalado en el equipo cliente en el almacén de entidades de certificación raíz de confianza.

## Registros

### Registros de aplicaciones

Los registros de aplicaciones en los equipos cliente registran la mayoría de los detalles de nivel superior de eventos de la conexión VPN.

Busca eventos desde el origen de cliente RAS. Todos los mensajes de error devuelven el código de error al final del mensaje. Algunos de los códigos de error más común se detallan a continuación, pero está disponible en [los códigos de Error de acceso remoto de enrutamiento y](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)una lista completa.

## Registros NPS
NPS se crea y almacena los registros de contabilidad de NPS. De manera predeterminada, estas se almacenan en %SYSTEMROOT%\\System32\\Logfiles\\ en un archivo denominado en*XXXX.* txt, donde *XXXX* es la fecha de creación del archivo.

De manera predeterminada, estos registros se encuentran en formato de valores separados por comas, pero no incluyen una fila de encabezado. Es la fila de encabezado:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si esta fila de encabezado se pega como la primera línea del archivo de registro, a continuación, importar el archivo en Microsoft Excel, las columnas se denominará correctamente.

Los registros NPS pueden ser útiles para diagnosticar problemas relacionados con la directiva. Para obtener más información acerca de los registros NPS, consulte [Interpretar los archivos de registro de formato de base de datos de NPS](https://technet.microsoft.com/library/cc771748.aspx).

## Problemas de script VPN_Profile.ps1
Los problemas más comunes al ejecutar manualmente el script VPN_ Profile.ps1 incluyen:

- ¿Se usa una herramienta de conexión remota?  Asegúrate de no usar RDP u otro método de conexión remoto como lo comedores con la detección de inicio de sesión de usuario.

- ¿Es el usuario Administrador de la máquina local?  Asegúrate de que mientras se ejecuta el script VPN_Profile.ps1 el usuario tenga privilegios de administrador.

- ¿Tienes características de seguridad adicionales de PowerShell habilitadas? Asegúrate de que la directiva de ejecución de PowerShell no está bloqueando el script. Considera la posibilidad de desactivación del modo de idioma restringido, si se habilita, antes de ejecutar el script. Se puede activar el modo de idioma restringido después de que el script se completa satisfactoriamente.

## Problemas de conexión de cliente de VPN siempre activada
Un error de configuración pequeño puede provocar un error en la conexión del cliente y puede ser un reto para averiguar la causa.  Un cliente VPN siempre activada se pasa a través de varios pasos antes de establecer una conexión. Al solucionar problemas de conexión de cliente, ve a través del proceso de eliminación con lo siguiente:


1. ¿Está conectada la máquina de plantilla externamente? Un examen **whatismyip** debería mostrar una dirección IP pública que no pertenece.

2. ¿Puede resolver el nombre del servidor VPN de acceso remoto a una dirección IP? En el **Panel de Control** \> **red** e **Internet** \> **Las conexiones de red**, abre las propiedades para el perfil de VPN. Debe ser el valor en la pestaña **General** públicamente debe poder resolver a través de DNS.

3. ¿Acceder el servidor VPN desde una red externa? Considera la posibilidad de apertura de protocolo de mensaje de Control de Internet (ICMP) a la interfaz externa y ping en el nombre desde el cliente remoto. Después de un comando ping es correcto, puedes quitar el ICMP permitir la regla.

4. ¿Tienes la NIC interna y externa en el servidor VPN configurado correctamente? ¿Son en distintas subredes? ¿La NIC externa que se conecta a la interfaz correcta en el servidor de seguridad?

5. ¿Son UDP 500 y 4500 puertos abiertos desde el cliente a la interfaz externa del servidor VPN? Comprobar el firewall de cliente, de seguridad del servidor y firewalls de hardware. IPSEC utiliza hacer es así 500, del puerto UDP seguro que no tienes IPEC deshabilitado o bloqueado en cualquier lugar.

7. ¿Es la validación de certificados produce un error? Comprueba que el servidor NPS tiene un certificado de autenticación de servidor que puede realizar el mantenimiento solicitudes IKE. Asegúrate de que tienes el IP del servidor VPN correcto especificado como un cliente NPS. Asegúrate de que se autentican con PEAP, y las propiedades de EAP protegido solo deben permitir la autenticación con un certificado. Puedes comprobar los registros de eventos NPS para errores de autenticación. Para obtener más información, consulta [instalar y configurar el servidor NPS](vpn-deploy-nps.md)

8. ¿Estás conectando pero no tienen acceso a la red de Internet/local? Comprueba los grupos IP de servidor DHCP o VPN para problemas de configuración.

9.  ¿Se conecta y tiene una IP interna válida pero no no tienen acceso a los recursos locales?  Comprueba que los clientes sepan cómo obtener acceso a los recursos. Puedes usar el servidor VPN para enrutar las solicitudes.


## Problemas de conexión de Azure AD de acceso condicional

### Vaya - no podrá acceder a esta aún

-   **Descripción del error.** Cuando la directiva de acceso condicional y no se cumple, bloquear la conexión VPN, pero se conecta una vez el usuario hace clic en **X** para cerrar el mensaje.  Al hacer clic en **Aceptar** , hace que otro intento de autenticación que termina en otro mensaje de _queja_ . Estos eventos se registran en el registro de eventos de operaciones de AAD del cliente. 

-   **Causa posible.** 

    - El usuario tiene un certificado de autenticación de cliente válidas en su certificado Personal de la tienda que no se ha emitido por AD Azure.

    - El perfil de VPN \ < TLSExtensions\ > sección es que faltan o no no contienen la **\ < EKUName\ > AAD condicional texto\ < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > acceso condicional de AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** entradas. El \ < EKUName > y \ entradas < EKUOID > indican el cliente VPN el certificado que se debe recuperar desde el almacén de certificados del usuario al pasar el certificado con el servidor VPN. Sin esto, el cliente VPN usa es el certificado de autenticación de cliente válido en el almacén de certificados del usuario y la autenticación se realiza correctamente. 

    - El servidor RADIUS (NPS) no se configuró para aceptar únicamente los certificados de cliente que contienen el **Acceso condicional de AAD** OID.

-   **Posibles soluciones.** Para evitar este bucle, hacer lo siguiente:

    1. En Windows PowerShell, ejecuta el cmdlet **Get-WmiObject** para volcar la configuración de perfil VPN. 
    2. Comprueba que la **\ < TLSExtensions >**, **\ < EKUName >**, y **\ < EKUOID >** secciones existe y se muestra el nombre correcto y OID. 
        ```
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

    3. Para determinar si hay certificados válidos en el almacén de certificados del usuario, ejecute el comando **Certutil** :

       ```
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
       >Si un certificado de emisor **CN = gen de entidad de certificación raíz de VPN de Microsoft 1** está presente en el almacén Personal del usuario, pero el usuario obtenido acceso haciendo clic en **X** para cerrar el mensaje de queja, recopilar registros de eventos CAPI2 para comprobar el certificado que se usa para autenticar era un certificado de autenticación de cliente válido que no se ha emitido desde la entidad de certificación raíz de VPN de Microsoft.

   4. Si existe un certificado de autenticación de cliente válido en el almacén Personal del usuario, la conexión se produce un error (como debería) después de que el usuario hace clic en la **X** y si la **\ < TLSExtensions >**, **\ < EKUName >**, y **\ < EKUOID >** las secciones existen y contienen la información correcta.<p>La _no se pudo encontrar un certificado que puede usarse con el protocolo de autenticación Extensible._ aparece el mensaje de error.

### No se ha podido eliminar el certificado de la hoja de conectividad VPN

-   **Descripción del error.** No se puede eliminar los certificados en la hoja de conectividad VPN.

-   **Causa posible.** El certificado se establece en **principal**.

-   **Posibles soluciones.** 

    1. En la hoja de conectividad VPN, selecciona el certificado.
    2. En **principal**, selecciona **No** y haga clic en **Guardar**.
    3. En la hoja de conectividad VPN, seleccione el certificado de nuevo.
    4. Haz clic en **Eliminar**.


---
