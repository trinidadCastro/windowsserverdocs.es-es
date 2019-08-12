---
title: Procedimientos habituales de solución de problemas de KMS y DNS
description: ''
ms.topic: article
ms.date: 07/22/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 12e3c1fa82a567c43507df2f2ffd72595c3903ba
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68664897"
---
# <a name="common-troubleshooting-procedures-for-kms-and-dns-issues"></a>Procedimientos habituales de solución de problemas de KMS y DNS

Es posible que tenga que usar algunos de estos métodos si una o varias de las condiciones siguientes son "true":

- Use los medios con licencias por volumen y una&nbsp;clave de producto genérica de licencia por volumen para instalar uno de los siguientes sistemas operativos:
   - Windows Server 2019
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- El Asistente para activación no puede conectarse a un equipo host de KMS.

Al intentar activar un sistema de cliente, el Asistente para activación utiliza DNS para buscar un equipo correspondiente que ejecute el software de KMS. Si el asistente consulta DNS y no encuentra la entrada DNS para el equipo host de KMS, notificará un error.   

<a id="list"></a>Revise la lista siguiente para buscar un enfoque que se ajuste a sus circunstancias:

- Si no puede instalar un host de KMS o no puede usar la activación de KMS, pruebe el procedimiento [Cambio de la clave de producto a una de tipo MAK](#change-the-product-key-to-an-mak).
- Si tiene que instalar y configurar un host de KMS, use el procedimiento [Configuración de un host de KMS para que los clientes puedan activarlo](#configure-a-kms-host-for-the-clients-to-activate-against).
- Si el cliente no puede encontrar el host de KMS existente, use los procedimientos siguientes para solucionar los problemas de las configuraciones de enrutamiento. Estos procedimientos se organizan de simples a complejos.
  - [Verificación de la conectividad de IP básica con el servidor DNS](#verify-basic-ip-connectivity-to-the-dns-server)
  - [Verificación de la configuración de host de KMS](#verify-the-configuration-of-the-kms-host)  
  - [Determinación del tipo de problema de enrutamiento](#determine-the-type-of-routing-issue)
  - [Verificación de la configuración de DNS](#verify-the-dns-configuration)
  - [Creación manual de un registro SRV de KMS](#manually-create-a-kms-srv-record)
  - [Asignación manual de un host de KMS a un cliente de KMS](#manually-assign-a-kms-host-to-a-kms-client)
  - [Configuración del host de KMS para publicar en varios dominios DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>Cambio de la clave de producto a una de tipo MAK

Si no puede instalar un host de KMS o, por algún otro motivo, no puede usar la activación de KMS, cambie la clave de producto a una de tipo MAK. Si descargó imágenes de Windows de Microsoft Developer Network (MSDN) o de TechNet, las unidades de almacén (SKU) que se enumeran debajo de los medios suelen tener medios de licencias por volumen y la clave de producto que se proporciona es una clave MAK.

Para cambiar la clave de producto a una de tipo MAK, siga estos pasos:

1. Abra una ventana del símbolo del sistema con permisos elevados. Para ello, presione la tecla del logotipo de Windows + X, haga clic en **Símbolo del sistema** y, a continuación, seleccione **Ejecutar como administrador**. Si se le pide una contraseña de administrador o una confirmación, escriba la contraseña o dé su confirmación.
2. Escriba el siguiente comando en el símbolo del sistema:
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > El marcador de posición **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx** representa la clave de producto de MAK.  

[Vuelva a la lista de procedimientos.](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>Configuración de un host de KMS para que los clientes puedan activarlo

La activación de KMS requiere la configuración de un host de KMS para que los clientes puedan activarlo. Si no hay ningún host de KMS configurado en su entorno, instale y active uno mediante una clave de host de KMS adecuada. Después de configurar un equipo en la red para hospedar el software de KMS, publique la configuración del Sistema de nombres de dominio (DNS).

Para obtener información sobre el proceso de configuración de host de KMS, consulte [Activar con el Servicio de administración de claves](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt) e [Instalar y configurar VAMT](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt).

[Vuelva a la lista de procedimientos.](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>Verificación de la conectividad de IP básica con el servidor DNS

Verifique la conectividad de IP básica con el servidor DNS mediante el comando ping. Para ello, siga estos pasos en el cliente de KMS que experimenta el error y el equipo host de KMS:

1. Abra una ventana del símbolo del sistema con permisos elevados.
1. Escriba el siguiente comando en el símbolo del sistema:
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > Si la salida de este comando no incluye la frase "Reply from", significa que hay un problema de red o un problema de DNS que debe resolver para poder usar los otros procedimientos de este artículo. Para obtener más información acerca de cómo solucionar problemas de TCP/IP si no puede hacer ping en el servidor DNS, consulte [Solución de problemas de TCP/IP avanzada](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip).

[Vuelva a la lista de procedimientos.](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>Verificación de la configuración de host de KMS

Compruebe el registro del servidor host de KMS para determinar si se está registrando con DNS. De forma predeterminada, un servidor host de KMS registra dinámicamente un registro SRV de DNS una vez cada 24 horas. 
> [!IMPORTANT]
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756), por si se produjeran problemas.  

Para seleccionar esta configuración, realice estos pasos:
1. Inicia el Editor del Registro. Para ello, haga clic con el botón derecho en **Inicio**, seleccione **Ejecutar**, escriba **regedit** y, a continuación, presione ENTRAR.
1. Busque la subclave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** y compruebe el valor de la entrada **DisableDnsPublishing**. Esta entrada tiene los siguientes valores posibles:
   - **0** o no definido (valor predeterminado): el servidor host de KMS registra un registro SRV una vez cada 24 horas.
   - **1**: el servidor host de KMS no registra automáticamente los registros SRV. Si la implementación no admite actualizaciones dinámicas, vea [Creación manual de un registro SRV de KMS](#manually-create-a-kms-srv-record).  
1. Si falta la entrada **DisableDnsPublishing**, créela (el tipo es DWORD). Si el registro dinámico es aceptable, deje el valor sin definir o establézcalo en **0**.

[Vuelva a la lista de procedimientos.](#list)

## <a name="determine-the-type-of-routing-issue"></a>Determinación del tipo de problema de enrutamiento

Puede usar los comandos siguientes para determinar si se trata de un problema de resolución de nombres o un problema de registro SRV.  

1. En un cliente de KMS, abra una ventana del símbolo del sistema con permisos elevados.  
1. En el símbolo del sistema, escriba los siguientes comandos:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > En este comando, <KMS_FQDN> representa el nombre de dominio completo (FQDN) del equipo host de KMS y el \<puerto\> representa el puerto TCP que usa KMS.  

   Si estos comandos resuelven el problema, significa que se trata de un problema del registro SRV. Puede solucionar el problema mediante el uso de uno de los comandos que se documentan en el procedimiento [Asignación manual de un host de KMS a un cliente de KMS](#manually-assign-a-kms-host-to-a-kms-client).  

1. Si el problema continúa, ejecute los comandos siguientes:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > En este comando, la \<dirección IP\> representa la dirección IP del equipo host de KMS y el \<puerto\> representa el puerto TCP que usa KMS.  

   Si estos comandos resuelven el problema, lo más probable es que haya un problema de resolución de nombres. Para obtener información adicional sobre la solución de problemas, consulte el procedimiento [Verificación de la configuración de DNS](#verify-the-dns-configuration).

1. Si ninguno de estos comandos soluciona el problema, compruebe la configuración del firewall del equipo. Cualquier comunicación de activación que se produzca entre los clientes de KMS y el host de KMS utiliza el puerto TCP 1688. Los firewalls en el cliente de KMS y el host de KMS deben permitir la comunicación a través del puerto 1688.

[Vuelva a la lista de procedimientos.](#list)

## <a name="verify-the-dns-configuration"></a>Verificación de la configuración de DNS

>[!NOTE]
> A menos que se indique lo contrario, siga estos pasos en un cliente de KMS que haya experimentado el error aplicable.

1. Abra una ventana del símbolo del sistema con permisos elevados.
1. Escriba el siguiente comando en el símbolo del sistema:
   ```cmd
   IPCONFIG /all
   ```
1. En los resultados del comando, tenga en cuenta la siguiente información:
   - La dirección IP asignada del equipo cliente de KMS
   - La dirección IP del servidor DNS principal que usa el equipo cliente de KMS
   - La dirección IP de la puerta de enlace predeterminada que usa el equipo cliente de KMS
   - La lista de búsqueda de sufijos DNS que usa el equipo cliente de KMS
1. Verifique que los registros SRV del host de KMS están registrados en DNS. Para ello, realice los pasos siguientes:  
   1. Abra una ventana del símbolo del sistema con permisos elevados.
   1. Escriba el siguiente comando en el símbolo del sistema:
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. Abra el archivo KMS.txt que genera el comando. Este archivo debe contener una o varias entradas que se parezcan a la entrada siguiente:
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > En esta entrada, contoso.com representa el dominio del host de KMS.
      1. Verifique la dirección IP, el nombre de host, el puerto y el dominio del host de KMS.
      1. Si existen estas entradas **_vlmcs** y contienen los nombres de host de KMS esperados, vaya a [Asignación manual de un host de KMS a un cliente de KMS](#manually-assign-a-kms-host-to-a-kms-client).  
      > [!NOTE]
      > Si el comando [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) encuentra el host de KMS, no significa que el cliente DNS pueda encontrar el host de KMS. Si el comando **nslookup** encuentra el host de KMS, pero todavía no lo puede activar mediante el host de KMS, compruebe la otra configuración de DNS, como el sufijo DNS principal y la lista de búsqueda del sufijo DNS.
1. Verifique que la lista de búsqueda del sufijo DNS principal contiene el sufijo de dominio DNS que está asociado con el host de KMS. Si la lista de búsqueda no incluye esta información, vaya al procedimiento [Configuración del host de KMS para publicar en varios dominios DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains).

[Vuelva a la lista de procedimientos.](#list)

## <a name="manually-create-a-kms-srv-record"></a>Creación manual de un registro SRV de KMS

Para crear manualmente un registro SRV para un host de KMS que use un servidor DNS de Microsoft, siga estos pasos:

1. Abra el Administrador de DNS en el servidor DNS. Para abrir el administrador de DNS,seleccione **Inicio**, **Herramientas administrativas** y, a continuación, seleccione **DNS**.
1. Seleccione el servidor DNS en el que tiene que crear el registro de recursos SRV.
1. En el árbol de consola, expanda la opción **Zonas de búsqueda directa**, haga clic con el botón derecho en el dominio y, a continuación, seleccione **Other New Records** (Otros registros nuevos).
1. Desplácese hacia abajo en la lista, seleccione **Ubicación de servicio (SRV)** y, a continuación, seleccione **Crear registro**.
1. Escriba la siguiente información:
   - Servicio: **_VLMCS**
   - Protocolo: **_TCP**
   - Número de puerto: **1688**
   - Host que ofrece este servicio: **&lt;*FQDN del host de KMS*&gt;**
1. Cuando haya terminado, seleccione **Aceptar** y, a continuación, haga clic en **Listo**.

Para crear manualmente un registro SRV para un host de KMS que use un servidor DNS compatible con BIND 9.x, siga las instrucciones para ese servidor DNS y proporcione la siguiente información para el registro SRV:

- Nombre:&nbsp; **_vlmcs._TCP**
- Tipo:&nbsp;**SRV**
- Prioridad: **0**
- Peso: **0**
- Puerto: **1688**
- Nombre de host: **&lt;*FQDN o nombre del host de KMS*&gt;**

> [!NOTE]
> KMS no utiliza los valores de **Prioridad** o **Peso**. Sin embargo, el registro debe incluirlos.

Para configurar un servidor DNS compatible con BIND 9.x para que admita la publicación automática de KMS, configure el servidor DNS para habilitar las actualizaciones de registros de recursos desde los hosts de KMS. Por ejemplo, agregue la siguiente línea a la definición de zona de Named.conf o de Named.conf.local:

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>Asignación manual de un host de KMS a un cliente de KMS

De forma predeterminada, los clientes de KMS usan el proceso de detección automática. Según este proceso, un cliente de KMS consulta a DNS si hay una lista de servidores que han publicado registros SRV _vlmcs dentro de la zona de pertenencia del cliente. DNS devuelve la lista de hosts de KMS en orden aleatorio. El cliente elige un host de KMS e intenta establecer una sesión en él. Si este intento funciona, el cliente almacena en caché el nombre del host de KMS e intenta usarlo para el siguiente intento de renovación. Si se produce un error en la configuración de la sesión, el cliente elige otro host de KMS de forma aleatoria. Le recomendamos encarecidamente que use el proceso de detección automática.  

Sin embargo, puede asignar manualmente un host de KMS a un cliente de KMS determinado. Para ello, siga estos pasos.

1. En un cliente de KMS, abra una ventana del símbolo del sistema con permisos elevados.
1. En función de la implementación, siga uno de estos pasos:
   - Para asignar un host de KMS mediante el FQDN del host, ejecute el siguiente comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - Para asignar un host de KMS mediante la dirección IP de la versión 4 del host, ejecute el siguiente comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - Para asignar un host de KMS mediante la dirección IP de la versión 6 del host, ejecute el siguiente comando:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - Para asignar un host de KMS mediante el nombre NETBIOS del host, ejecute el siguiente comando:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - Para revertir a la detección automática en un cliente de KMS, ejecute el siguiente comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > Estos comandos usan los siguientes marcadores de posición:
     >- **<KMS_FQDN>** representa el nombre de dominio completo (FQDN) del equipo host de KMS.
     >- **\<IPv4Address\>** representa la dirección IP de la versión 4 del equipo host de KMS.
     >- **\<IPv6Address\>** representa la dirección IP de la versión 6 del equipo host de KMS.
     >- **\<\>NETBIOSName** representa el nombre NetBIOS del equipo host de KMS.
     >- **\<port\>** representa el puerto TCP que usa KMS.  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>Configuración del host de KMS para publicar en varios dominios DNS

> [!IMPORTANT]
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.

Tal y como se describe en [Asignación manual de un host de KMS a un cliente de KMS](#manually-assign-a-kms-host-to-a-kms-client), los clientes de KMS suelen usar el proceso de detección automática para identificar los hosts de KMS. Este proceso requiere que los registros de _vlmcs SRV estén disponibles en la zona DNS del equipo cliente de KMS. La zona DNS se corresponde con el sufijo DNS principal del equipo o con uno de los siguientes dominios:
- En el caso de los equipos unidos a un dominio, el dominio del equipo asignado por el sistema de DNS (por ejemplo, el DNS de Active Directory Domain Services [AD DS]).
- Para los equipos de grupo de trabajo, el dominio del equipo asignado por el Protocolo de configuración dinámica de host (DHCP). Este nombre de dominio se define mediante la opción que tiene el valor de código 15, tal y como se define en las Solicitudes de comentarios (RFC) 2132.

De forma predeterminada, un host de KMS registra sus registros SRV en la zona DNS que corresponde al dominio del equipo host de KMS. Por ejemplo, supongamos que un host de KMS se une al dominio contoso.com. En este escenario, el host de KMS registra su registro SRV de _vmlcs en la zona DNS de contoso.com. Por lo tanto, el registro identifica el servicio como VLMCS._TCP.CONTOSO.COM.

Si el host de KMS y los clientes de KMS usan diferentes zonas DNS, debe configurar el host de KMS para publicar automáticamente sus registros SRV en varios dominios DNS. Para ello, realice los pasos siguientes:

1. En el host de KMS, inicie el Editor del Registro. 
1. Busque la subclave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** y selecciónela.
1. En el panel **Detalles**, haga clic con el botón derecho en un área en blanco, seleccione **Nuevo** y, a continuación, seleccione **Valor de cadena múltiple**.
1. Escriba **DnsDomainPublishList** como nombre de la nueva entrada.
1. Haga clic con el botón derecho en la nueva entrada **DnsDomainPublishList** y, luego, seleccione **Modificar**.
1. En el cuadro de diálogo **Modificar cadenas múltiples**, escriba los sufijos de dominio DNS que KMS publica en una línea independiente y, a continuación, seleccione **Aceptar**.
   > [!NOTE]
   > En Windows Server 2008 R2, el formato de **DnsDomainPublishList** es distinto. Si quiere obtener más información, vea la guía de referencia técnica de activación de volumen.
1. Use la herramienta administrativa de los servicios para reiniciar el Servicio de licencias de software. Esta operación crea los registros SRV.
1. Verifique que, mediante un método típico, el cliente de KMS puede ponerse en contacto con el host de KMS que ha configurado. Verifique que el cliente de KMS identifica correctamente el host de KMS por nombre y por dirección IP. Si se produce un error en cualquiera de estas verificaciones, investigue este problema de la resolución de cliente DNS.
1. Para borrar cualquier nombre de host de KMS almacenado en caché previamente en el cliente de KMS, abra una ventana de símbolo del sistema con privilegios elevados en el cliente de KMS y, a continuación, ejecute el siguiente comando:
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```
