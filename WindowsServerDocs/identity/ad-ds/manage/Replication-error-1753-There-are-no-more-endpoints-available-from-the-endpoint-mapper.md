---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 'Error de replicación 1753: no hay más puntos de conexión disponibles desde el asignador de puntos de conexión'
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9752425c0732c2290642d62239151f20acb99ad0
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88940485"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Error de replicación 1753: no hay más puntos de conexión disponibles desde el asignador de puntos de conexión

>Se aplica a: Windows Server

En este artículo se describen los síntomas, los pasos de la causa y la resolución de las operaciones de Active Directory que generan un error con Win32 1753: "no hay más extremos disponibles desde el asignador de extremos".

DCDIAG informa de que se ha producido un error 1753 en la prueba de conectividad, la prueba de Active Directory de las replicaciones o la prueba de KnowsOfRoleHolders: "no hay más extremos disponibles desde el asignador de extremos".

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN.EXE informa de que se ha producido un error en el intento de replicación con el estado 1753.
Los comandos REPADMIN que suelen mencionar el estado 1753 incluyen, pero no se limitan a:

* REPADMIN/REPLSUM
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN/SYNCALL

A continuación se muestra un resultado de ejemplo de "REPADMIN/SHOWREPS" que describe la replicación de entrada de CONTOSO-DC2 a CONTOSO-DC1 con el error "se denegó el acceso a la replicación":

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

El comando **comprobar topología de replicación** de Active Directory sitios y servicios devuelve "no hay más extremos disponibles desde el asignador de extremos".

Al hacer clic con el botón derecho en el objeto de conexión desde un controlador de dominio de origen y elegir **comprobar la topología de replicación** , se produce el error "no hay más extremos disponibles desde el asignador de extremos". A continuación se muestra el mensaje de error en pantalla:

Texto del título del diálogo: comprobar el texto del mensaje de diálogo de la topología de replicación: se produjo el siguiente error al intentar ponerse en contacto con el controlador de dominio: no hay más extremos disponibles desde el asignador de extremos.

El comando **Replicar ahora** en Active Directory sitios y servicios devuelve "no hay más extremos disponibles desde el asignador de extremos".
Al hacer clic con el botón derecho en el objeto de conexión desde un controlador de dominio de origen y elegir **Replicar ahora** se produce el error "no hay más extremos disponibles desde el asignador de extremos".
A continuación se muestra el mensaje de error en pantalla:

Texto del título del cuadro de diálogo: replicar ahora texto del mensaje de diálogo: se produjo el siguiente error al intentar sincronizar el contexto \<%directory partition name%> de nomenclatura del controlador de dominio \<Source DC> al controlador de dominio \<Destination DC> :

no hay más puntos de conexión disponibles desde el asignador de puntos de conexión.
La operación no continuará

Los eventos NTDS KCC, NTDS general o Microsoft-Windows-ActiveDirectory_DomainService con el estado-2146893022 se registran en el registro de servicios de directorio en Visor de eventos.

Active Directory eventos que suelen mencionar el estado-2146893022 incluyen, pero no se limitan a:

| Id. de evento | Origen de eventos | Cadena de evento|
| --- | --- | --- |
| 1655 | NTDS general | Active Directory intentó comunicarse con el catálogo global siguiente y los intentos fueron incorrectos. |
| 1925 | KCC NTDS | Error al intentar establecer un vínculo de replicación para la siguiente partición de directorio grabable. |
| 1265 | KCC NTDS | Error en un intento del comprobador de coherencia de la información (KCC) para agregar un acuerdo de replicación para la siguiente partición de directorio y el controlador de dominio de origen. |

## <a name="cause"></a>Causa

En los pasos siguientes se muestra el flujo de trabajo RPC a partir del registro de la aplicación de servidor con el asignador de extremos de RPC (EPM) en el paso 1 para pasar datos del cliente RPC a la aplicación cliente en el paso 7.

### <a name="adds-rpc-workflow"></a>AGREGA un flujo de trabajo RPC

1. La aplicación de servidor registra sus puntos de conexión con el asignador de extremos de RPC (EPM)
1. El cliente realiza una llamada RPC (en nombre de un usuario, sistema operativo o operación iniciada por la aplicación)
1. El RPC del lado cliente se pone en contacto con los equipos de destino EPM y pide el punto de conexión para completar la llamada de cliente
1. EPM del equipo servidor responde con un punto de conexión
1. El cliente RPC se pone en contacto con la aplicación de servidor
1. La aplicación de servidor ejecuta la llamada, devuelve el resultado al RPC del cliente
1. El RPC del lado cliente devuelve el resultado a la aplicación cliente

El error 1753 se genera mediante un error entre los pasos #3 y #4. En concreto, el error 1753 significa que el cliente RPC (DC de destino) pudo ponerse en contacto con el servidor RPC (DC de origen) a través del Puerto 135, pero el EPM del servidor RPC (DC de origen) no encontró la aplicación RPC de interés y devolvió el error del lado servidor 1753. La presencia del error 1753 indica que el cliente RPC (DC de destino) recibió la respuesta de error del lado servidor del servidor RPC (DC de origen de replicación de AD) a través de la red.

Las causas raíz específicas del error 1753 incluyen:

* Nunca se ha iniciado la aplicación de servidor (es decir, el paso #1 en el diagrama "más información" que se encuentra arriba).
* Se inició la aplicación de servidor, pero se produjo algún error durante la inicialización que impidió que se registrara con el asignador de extremos de RPC (es decir, el paso #1 en el diagrama "más información" anterior se intentaba, pero no se pudo realizar).
* La aplicación de servidor se inició, pero posteriormente se desvocó. (es decir, el paso #1 del diagrama "más información" anterior se completó correctamente, pero se deshizo más tarde porque el servidor ha muerto).
* La aplicación de servidor anuló manualmente el registro de sus puntos de conexión (similar a 3, pero intencional. No es probable, pero se incluye por integridad).
* El cliente RPC (DC de destino) se ha puesto en contacto con un servidor RPC diferente del previsto debido a un error de asignación de nombre a dirección IP en DNS, WINS o el archivo host/LMHOSTS.

El error 1753 no se debe a:

* Falta de conectividad de red entre el cliente RPC (DC de destino) y el servidor RPC (DC de origen) a través del Puerto 135
* Falta de conectividad de red entre el servidor RPC (DC de origen) mediante el Puerto 135 y el cliente RPC (DC de destino) a través del puerto efímero.
* Un error de coincidencia de contraseña o la incapacidad del controlador de dominio de origen para descifrar un paquete cifrado de Kerberos

## <a name="resolutions"></a>Soluciones

Comprobar que se ha iniciado el servicio que registra su servicio con el asignador de extremos

* En los controladores de dominio de Windows 2000 y Windows Server 2003: Asegúrese de que el controlador de dominio de origen se ha iniciado en modo normal.
* En Windows Server 2008 o Windows Server 2008 R2: desde la consola del DC de origen, inicie el administrador de servicios (Services. msc) y compruebe que el servicio de Active Directory Domain Services se está ejecutando.

Compruebe que el cliente RPC (DC de destino) está conectado al servidor RPC deseado (DC de origen).

Todos los controladores de dominio de un bosque de Active Directory común registran un registro CNAME del controlador de dominio en el _msdcs. \<forest root domain> Zona DNS independientemente del dominio en el que residen en el bosque. El registro CNAME de DC se deriva del atributo objectGUID del objeto de configuración NTDS para cada controlador de dominio.

Al realizar operaciones basadas en la replicación, un controlador de dominio de destino consulta DNS para el registro CNAME de DC de origen. El registro CNAME contiene el nombre de equipo completo del DC de origen que se usa para derivar la dirección IP de los DC de origen a través de la búsqueda de caché del cliente DNS, la búsqueda de archivos host/LMHost, el host A/AAAA en DNS o WINS.

Los objetos de configuración NTDS obsoletos y las asignaciones de nombre a dirección erróneas en DNS, WINS, host y LMHOST pueden provocar que el cliente RPC (DC de destino) se conecte al servidor RPC incorrecto (DC de origen). Además, la asignación de nombre a dirección no válida puede hacer que el cliente RPC (DC de destino) se conecte a un equipo que no tenga aún la aplicación de servidor RPC de interés (en este caso, el rol Active Directory). (Ejemplo: un registro de host obsoleto para DC2 contiene la dirección IP de DC3 o un equipo miembro).

Compruebe que el objectGUID del DC de origen que existe en la copia de los controladores de dominio de destino de Active Directory coincide con el objectGUID del DC de origen almacenado en la copia de los controladores de dominio de origen de Active Directory. Si hay alguna discrepancia, use repadmin/showobjmeta en el objeto de configuración NTDS para ver cuál corresponde a la última promoción del controlador de dominio de origen (sugerencia: Compare las marcas de fecha del objeto de configuración NTDS crear fecha desde/showobjmeta con la última fecha de promoción en el archivo de origen de archivos de registro Dcpromo. log. Es posible que tenga que usar la última fecha de modificación y creación de DCPROMO. Archivo de registro en sí). Si los GUID del objeto no son idénticos, es probable que el controlador de dominio de destino tenga un objeto de configuración NTDS obsoleto para el DC de origen cuyo registro CNAME hace referencia a un registro de host con un nombre incorrecto para la asignación de IP.

En el controlador de dominio de destino, ejecute IPCONFIG/ALL para determinar qué servidores DNS usa el controlador de dominio de destino para la resolución de nombres:

```
c:>ipconfig /all
```

En el controlador de dominio de destino, ejecute NSLOOKUP en el registro CNAME de DC completo de los DC de origen:

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

Compruebe que la dirección IP devuelta por NSLOOKUP "posee" el nombre de host/identidad de seguridad del controlador de dominio de origen:

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

Inicie sesión en la consola del controlador de dominio de origen, ejecute "IPCONFIG" desde el símbolo del sistema y compruebe que el controlador de dominio de origen es el propietario de la dirección IP devuelta por el comando NSLOOKUP anterior

Buscar hosts obsoletos o duplicados en las asignaciones de IP en DNS

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

Si hay direcciones IP no válidas en los registros de host, investigue si está habilitada la eliminación de registros obsoletos de DNS y está correctamente configurada.

Si las pruebas anteriores o un seguimiento de red no muestran una consulta de nombre que devuelva una dirección IP no válida, considere las entradas obsoletas en los archivos de HOST, los archivos LMHOSTs y los servidores WINS. Tenga en cuenta que los servidores DNS también pueden configurarse para realizar la resolución de nombres de reserva WINS.

* Compruebe que la aplicación de servidor (Active Directory et al) se ha registrado con el asignador de extremos en el servidor RPC (DC de origen).
* Active Directory usa una mezcla de puertos conocidos y registrados dinámicamente. En esta tabla se enumeran los puertos y protocolos conocidos usados por Active Directory controladores de dominio.

| Aplicación de servidor RPC | Port | TCP | UDP |
| --- | --- | --- | --- |
| Servidor DNS | 53 | X | X |
| Kerberos | 88 | X | X |
| Servidor LDAP | 389 | X | X |
| Microsoft-DS | 445 | X | X |
| SSL de LDAP | 636 | X | X |
| Servidor del catálogo global (GC) | 3268 | X |   |
| Servidor del catálogo global (GC) | 3269 | X |   |

Los puertos conocidos no se registran con el asignador de extremos.

Active Directory y otras aplicaciones también registran servicios que reciben puertos asignados dinámicamente en el intervalo de puertos efímeros de RPC. Estas aplicaciones de servidor RPC están asignadas dinámicamente a puertos TCP entre 1024 y 5000 en equipos con Windows 2000 y Windows Server 2003 y puertos entre el intervalo de 49152 y 65535 en equipos con Windows Server 2008 y Windows Server 2008 R2. El puerto RPC utilizado por la replicación puede estar codificado de forma rígida en el registro siguiendo los pasos que se describen en el [artículo de KB 224196](https://support.microsoft.com/kb/224196). Active Directory continúa registrando en EPM cuando se configura para usar un puerto codificado de forma rígida.

Compruebe que la aplicación de servidor RPC de interés se ha registrado con el asignador de extremos de RPC en el servidor RPC (el controlador de dominio de origen en el caso de la replicación de AD).

Hay varias maneras de realizar esta tarea, pero una consiste en instalar y ejecutar PORTQRY desde un símbolo del sistema con privilegios de administrador en la consola del controlador de dominio de origen con la sintaxis:

```
portquery -n <source DC> -e 135 > file.txt
```

En la salida de Portqry, tenga en cuenta los números de puerto que se registran dinámicamente con la "interfaz DRS de directorio de MS NT" (UUID = 351...) para el protocolo ncacn_ip_tcp. En el fragmento de código siguiente se muestra el resultado de portquery de ejemplo de un DC de Windows Server 2008 R2:

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

Otras posibles formas de resolver este error:

* Compruebe que el controlador de dominio de origen se ha iniciado en modo normal y que el rol de sistema operativo y controlador de dominio en el controlador de dominio de origen se ha iniciado completamente.
* Compruebe que el servicio Dominio de Active Directory se está ejecutando. Si el servicio está detenido o no está configurado con valores de inicio predeterminados, restablezca los valores de inicio predeterminados, reinicie el controlador de dominio modificado y vuelva a intentar la operación.
* Compruebe que el valor de inicio y el estado del servicio para el servicio RPC y el localizador de RPC son correctos para la versión del sistema operativo del cliente RPC (DC de destino) y el servidor RPC (DC de origen). Si el servicio está detenido o no está configurado con valores de inicio predeterminados, restablezca los valores de inicio predeterminados, reinicie el controlador de dominio modificado y vuelva a intentar la operación.
   * Además, asegúrese de que el contexto de servicio coincide con los valores de configuración predeterminados que se muestran en la tabla siguiente.

      | Servicio | Estado predeterminado (tipo de inicio) en Windows Server 2003 y versiones posteriores | Estado predeterminado (tipo de inicio) en Windows Server 2000 |
      | --- | --- | --- |
      | Llamada a procedimiento remoto | Iniciado (automático) | Iniciado (automático) |
      | Localizador de llamadas a procedimientos remotos | Null o detenido (manual) | Iniciado (automático) |

* Compruebe que el tamaño del intervalo de puertos dinámicos no se ha restringido. A continuación se muestra la sintaxis de NETSH para Windows Server 2008 y Windows Server 2008 R2 para enumerar el intervalo de puertos RPC:

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* Compruebe que las definiciones de Puerto codificadas de forma rígida definidas en KB 224196 se encuentran dentro del intervalo de puertos dinámico para la versión del sistema operativo de DC de origen. Revise el [artículo 224196 de Knowledge Base](https://support.microsoft.com/kb/224196) y asegúrese de que el puerto codificado está dentro del intervalo de puertos efímeros para la versión del sistema operativo del controlador de dominio de origen.

* Compruebe que la clave ClientProtocols existe en HKLM\Software\Microsoft\Rpc y que contiene los cinco valores predeterminados siguientes:

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>Más información

Ejemplo de un nombre incorrecto para la asignación de direcciones IP que produce el error de RPC 1753 frente a-2146893022: el nombre de la entidad de seguridad de destino es incorrecto

El dominio contoso.com consta de DC1 y DC2 con las direcciones IP x. x. 1.1 y x. x. 1.2. Los registros "A"/"AAAA" de host para DC2 se registran correctamente en todos los servidores DNS configurados para DC1. Además, el archivo de hosts en DC1 contiene una asignación de entrada DC2s nombre de host completo a la dirección IP x. x. 1.2. Más adelante, la dirección IP de DC2's cambia de X. X. 1.2 a X. X. 1.3 y un nuevo equipo miembro se une al dominio con la dirección IP x. x. 1.2. Los intentos de replicación de AD desencadenados por el comando **replicar Now** en Active Directory complemento sitios y servicios producen el error 1753, tal y como se muestra en el siguiente seguimiento:

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

En el marco **10**, el controlador de dominio de destino consulta el asignador de punto de conexión de DC de origen a través del Puerto 135 para el Active Directory UUID de clase del servicio de replicación de E351...

En el marco **11**, el controlador de dominio de origen, en este caso un equipo miembro que todavía no hospeda el rol de controlador de dominio y, por tanto, no ha registrado el E351... El UUID del servicio de replicación con su EPM local responde con el error simbólico EP_S_NOT_REGISTERED que se asigna al error decimal 1753, 0x6d9 de error hexadecimal y error descriptivo "no hay más extremos disponibles desde el asignador de extremos".

Más adelante, el equipo miembro con la dirección IP x. x. 1.2 se promociona como una réplica "MayberryDC" en el dominio contoso.com. Una vez más, el comando **Replicar ahora** se usa para desencadenar la replicación pero se produce un error en la pantalla "el nombre de la entidad de seguridad de destino es incorrecto". El equipo a cuyo adaptador de red se asigna la dirección IP x. x. 1.2 es un controlador de dominio que se está iniciando en el modo normal y ha registrado el E351... UUID del servicio de replicación con su EPM local pero no posee el nombre o la identidad de seguridad de DC2 y no puede descifrar la solicitud de Kerberos de DC1, por lo que la solicitud ahora produce el error "el nombre principal de destino es incorrecto". El error se asigna al error decimal-2146893022/0x80090322 de error hexadecimal.

Estas asignaciones de host a IP no válidas pueden deberse a entradas obsoletas en los archivos host/LMHost, los registros de host A/AAAA en DNS o WINS.

Resumen: en este ejemplo se produjo un error porque una asignación de host a IP no válida (en el archivo de HOST en este caso) hizo que el controlador de dominio de destino se resolvera como un "origen" que no tenía el servicio Active Directory Domain Services en ejecución (o incluso instalado para ese fin), por lo que el SPN de replicación no se registró y el DC de origen de1753 volvió el En el segundo caso, una asignación de host a dirección IP no válida (de nuevo en el archivo HOST) causó que el controlador de dominio de destino se conectara a un controlador de dominio que había registrado el E351... SPN de replicación, pero el origen tenía un nombre de host y una identidad de seguridad distintos del DC de origen previsto para que se produjeron errores: 2146893022: el nombre de la entidad de seguridad de destino es incorrecto.

## <a name="related-topics"></a>Temas relacionados

* [Solución de problemas de operaciones de Active Directory que producen el error 1753: no hay más extremos disponibles desde el asignador de extremos.](https://support.microsoft.com/kb/2089874)
* [Artículo de KB 839880 solución de errores del asignador de extremos de RPC con las herramientas de soporte de Windows Server 2003 desde el CD del producto](https://support.microsoft.com/kb/839880)
* [Artículo de KB 832017 información general del servicio y requisitos de puerto de red para el sistema Windows Server](https://support.microsoft.com/kb/832017/)
* [Artículo 224196 de Knowledge base sobre cómo restringir el tráfico de replicación de Active Directory y el tráfico RPC de cliente a un puerto específico](https://support.microsoft.com/kb/224196/)
* [Artículo 154596 de Knowledge base sobre cómo configurar la asignación dinámica de puertos RPC para trabajar con firewalls](https://support.microsoft.com/kb/154596)
* [Cómo funciona RPC](/windows/win32/rpc/how-rpc-works)
* [Cómo se prepara el servidor para una conexión](/windows/win32/rpc/how-the-server-prepares-for-a-connection)
* [Cómo el cliente establece una conexión](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [Registrar la interfaz](/windows/win32/rpc/registering-the-interface)
* [Poner el servidor a disposición en la red](/windows/win32/rpc/making-the-server-available-on-the-network)
* [Registro de puntos de conexión](/windows/win32/rpc/registering-endpoints)
* [Escuchando llamadas de cliente](/windows/win32/rpc/listening-for-client-calls)
* [Cómo el cliente establece una conexión](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [Restringir el tráfico de replicación de Active Directory y el tráfico de RPC de cliente a un puerto específico](https://support.microsoft.com/kb/224196)
* [SPN para un controlador de dominio de destino en AD DS](/openspecs/windows_protocols/ms-drsr/41efc56e-0007-4e88-bafe-d7af61efd91f)
