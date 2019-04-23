---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Comprobación de la funcionalidad de DNS para admitir la replicación de directorios
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871876"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Comprobación de la funcionalidad de DNS para admitir la replicación de directorios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Para comprobar la configuración del sistema de nombres de dominio (DNS) que podría interferir con la replicación de Active Directory, puede empezar a ejecutar la prueba básica que garantiza que el DNS funciona correctamente para su dominio. Después de ejecutar la prueba básica, puede probar otros aspectos de la funcionalidad DNS, incluido el registro del registro de recursos y la actualización dinámica.

Aunque puede ejecutar esta prueba de la funcionalidad básica de DNS en cualquier controlador de dominio, normalmente puede ejecutar esta prueba en los controladores de dominio que cree que puede estar experimentando problemas de replicación, por ejemplo, los controladores de dominio que informan de los identificadores de evento 1844, 1925, 2087, o 2088 en el registro de DNS del servicio de directorio de evento Visor.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Ejecuta la prueba DNS básica del controlador de dominio</title>

La prueba básica de DNS comprueba los siguientes aspectos de funcionalidad de DNS:


- **Conectividad:** La prueba determina si los controladores están registrados en DNS, el dominio puede ser contactado por el <system>ping</system> comando y tener Lightweight Directory Access Protocol / conectividad LDAP/RPC () llamada a procedimiento remoto. Si se produce un error en la prueba de conectividad en un controlador de dominio, no hay otras pruebas se ejecutan en ese controlador de dominio. La prueba de conectividad se realiza automáticamente antes de ejecutar cualquier otra prueba DNS.
- **Servicios esenciales:** La prueba confirma que los siguientes servicios están disponibles en el controlador de dominio probados y ejecución: Servicio cliente DNS, el servicio de Net Logon, servicio del centro de distribución de claves (KDC) y el servicio de servidor DNS (si DNS está instalado en el controlador de dominio).
- **Configuración del cliente DNS:**  La prueba confirma que los servidores DNS de todos los adaptadores de red del equipo cliente DNS están accesibles.
- **Registros de recursos:** La prueba confirma que el registro de recursos de host (A) de cada controlador de dominio está registrado en al menos uno de los servidores DNS que está configurado en el equipo cliente.
- **Zona y el inicio de autoridad (SOA):** Si el controlador de dominio se está ejecutando el servicio servidor DNS, la prueba confirma que la zona de dominio de Active Directory y el inicio del registro de recurso de autoridad (SOA) para la zona de dominio de Active Directory están presentes.
- **Zona raíz:** Comprueba si está presente la zona raíz (.).

Pertenencia al grupo Administradores de organización o equivalente, es lo mínimo necesario para completar estos procedimientos.

Puede usar el procedimiento siguiente para comprobar la funcionalidad básica de DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Para comprobar la funcionalidad básica de DNS:


1. En el controlador de dominio que desea probar o en un equipo miembro del dominio que tenga instaladas las herramientas los servicios de dominio de Active Directory (AD DS), abra un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en **Inicio**. 
2. En Iniciar búsqueda, escriba el símbolo del sistema. 
3. En la parte superior del menú Inicio, haga clic en el símbolo del sistema y, a continuación, haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
4. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Sustituya el nombre distintivo real, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt;DCName&gt;. Como alternativa, puede probar todos los controladores de dominio del bosque escribiendo/e: en lugar de/s:. El modificador /f Especifica un nombre de archivo, que en el comando anterior es dcdiagreport.txt. Si desea colocar el archivo en una ubicación diferente en el directorio de trabajo actual, puede especificar una ruta de acceso de archivo, como /f:c:reportsdcdiagreport.txt.

5. Abra el archivo dcdiagreport.txt en el Bloc de notas o un editor de texto similares. Para abrir el archivo en el Bloc de notas, en el símbolo del sistema, escriba dcdiagreport.txt el Bloc de notas y, a continuación, presione ENTRAR. Si ha colocado el archivo en un directorio de trabajo diferente, incluya la ruta de acceso al archivo. Por ejemplo, si ha colocado el archivo en c:reports, escriba c:reportsdcdiagreport.txt el Bloc de notas y, a continuación, presione ENTRAR.
6. Desplácese a la tabla de resumen en la parte inferior del archivo. 
</br></br>Anote los nombres de todos los controladores de dominio de ese estado de "Advertencia" o "Error" en la tabla de resumen del informe.  Intente determinar si hay un controlador de dominio del problema mediante la búsqueda de la sección de desglose por buscar la cadena "controlador de dominio: DCName,"donde DCName es el nombre real del controlador de dominio.

Si ve los cambios de configuración obvio que son necesarios, asegúrese de ellos, según corresponda. Por ejemplo, si observa que uno de los controladores de dominio tiene una dirección IP incorrecta Obviamente, puede corregirlo. A continuación, vuelva a ejecutar la prueba.

Para validar los cambios de configuración, vuelva a ejecutar el comando de Dcdiag/Test: DNS /v con el modificador/e: o/s:, según corresponda. Si no tienes IP versión 6 (IPv6) habilitada en el controlador de dominio, debe esperar la parte de la validación de host (AAAA) de la prueba, pero si no usas IPv6 en la red, estos registros no son necesarios.
            
## <a name="verifying-resource-record-registration"></a>Comprobando el registro del registro de recursos
    
El controlador de dominio de destino usa el registro de recursos de alias (CNAME) de DNS para localizar a su asociado de replicación del controlador de dominio de origen. Aunque los controladores de dominio que ejecutan Windows Server (empezando por Windows Server 2003 con Service Pack 1 (SP1)) pueden encontrar asociados de replicación de origen mediante el uso de nombres de dominio completo (FQDN) o, si se produce un error, NetBIOS rutasEl presencia del alias (CNAME) registro de recursos se espera y debe comprobarse para DNS correcto funcionamiento. 
      
Puede usar el procedimiento siguiente para comprobar el registro del registro de recursos, incluido el registro del registro de recursos de alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Para comprobar el registro de recursos</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio. En Iniciar búsqueda, escriba el símbolo del sistema. 
2. En la parte superior del menú Inicio, haga clic en el símbolo del sistema y, a continuación, haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.  </br></br>Puede usar la herramienta Dcdiag para comprobar el registro de todos los registros de recursos que son esenciales para la ubicación del controlador de dominio mediante la ejecución de la `dcdiag /test:dns /DnsRecordRegistration` comando.

Este comando comprueba el registro de los siguientes registros de recursos en DNS:


- **alias (CNAME):** el identificador único global (GUID): en función de registro de recursos que busca un asociado de replicación
- **host (A):** el registro de recursos de host que contiene la dirección IP del controlador de dominio
- **LDAP SRV:** los registros de recursos de servicio (SRV) que se ubican en los servidores LDAP
- **GC SRV**: los registros de recursos de servicio (SRV) busque global de servidores de catálogo
- **PDC SRV**: los registros de recursos de servicio (SRV) que busque los maestros de operaciones de emulador PDC (controlador) de dominio principal

Puede usar el procedimiento siguiente para comprobar por sí sola de registro registro de recursos de alias (CNAME).

### <a name="to-verify-alias-cname-resource-record-registration"></a>Para comprobar el registro de recursos de alias (CNAME)

1. Abra el complemento DNS. Para abrir DNS, haga clic en Inicio. En Iniciar búsqueda, escriba dnsmgmt.msc y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que muestra la acción que desee y, a continuación, haga clic en continuar.
2. Use el complemento de DNS para encontrar cualquier controlador de dominio que se está ejecutando el servicio servidor DNS, donde el servidor hospeda la zona DNS con el mismo nombre que el dominio de Active Directory del controlador de dominio.
3. En el árbol de consola, haga clic en la zona que se denomina _msdcs. NombreDeDominioDns.
4. En el panel de detalles, compruebe que existen los siguientes registros de recursos: un registro de recursos de alias (CNAME) que se denomina Dsa_Guid._msdcs. <placeholder>NombreDeDominioDNS</placeholder> y correspondiente de host (A) registro de recursos para el nombre del servidor DNS.

Si no está registrado el registro de recursos de alias (CNAME), compruebe que actualización dinámica funciona correctamente. La prueba en la sección siguiente se usa para comprobar la actualización dinámica.
    
## <a name="verifying-dynamic-update"></a>Comprobación de actualización dinámica
    
Si la prueba DNS básica se muestra que los registros de recursos no existen en DNS, use la prueba de actualización dinámica para determinar por qué el servicio de Net Logon no registró los registros de recursos automáticamente. Para comprobar que la zona de dominio de Active Directory está configurada para aceptar actualizaciones dinámicas seguras y para realizar el registro de un registro de prueba (_dcdiag_test_record), use el procedimiento siguiente. El registro de prueba se elimina automáticamente después de la prueba.

### <a name="to-verify-dynamic-updatetitle"></a>Para comprobar la actualización dinámica</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio. En Iniciar búsqueda, escriba el símbolo del sistema. En la parte superior del menú Inicio, haga clic en el símbolo del sistema y, a continuación, haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
2. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Sustituya el nombre distintivo, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt;DCName&gt;. Como alternativa, puede probar todos los controladores de dominio del bosque escribiendo/e: en lugar de/s:. Si no tiene IPv6 habilitada en el controlador de dominio, debe esperar el host (AAAA) recursos registro parte de la prueba, que es una condición normal cuando IPv6 no está habilitado.

Si no se configuran las actualizaciones dinámicas seguras, puede usar el siguiente procedimiento para configurarlos.

### <a name="to-enable-secure-dynamic-updates"></a>Para habilitar las actualizaciones dinámicas seguras


1. Abra el complemento DNS. Para abrir DNS, haga clic en Inicio. 
2. En Iniciar búsqueda, escriba dnsmgmt.msc y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que muestra la acción que desee y, a continuación, haga clic en continuar.
3. En el árbol de consola, haga clic en la zona aplicable y, a continuación, haga clic en Propiedades.
4. En la ficha General, compruebe que el tipo de zona está integrada en Active Directory.
5. En las actualizaciones dinámicas, sólo haga clic en seguridad.

## <a name="registering-dns-resource-records"></a>Registrar registros de recursos DNS
    
Si no aparecen los registros de recursos DNS en DNS para el controlador de dominio de origen, que haya comprobado que las actualizaciones dinámicas y desea registrar registros de recursos DNS inmediatamente, puede forzar el registro manualmente mediante el procedimiento siguiente. El servicio de Net Logon de un controlador de dominio registra los registros de recursos DNS que son necesarios para el controlador de dominio se encuentren en la red. El servicio cliente DNS registra el registro de recursos de host (A) que señala el registro de alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Para registrar manualmente los registros de recursos DNS</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio. 
2. En Iniciar búsqueda, escriba el símbolo del sistema. 
3. En la parte superior del inicio, haga clic en el símbolo del sistema y, a continuación, haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
4. Para iniciar el registro de recursos de localizador de controlador de dominio registros manualmente en el controlador de dominio de origen, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `net stop netlogon && net start netlogon`
5. Para iniciar el registro del host (A) registro de recursos manualmente, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `ipconfig /flushdns && ipconfig /registerdns`
6. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName>` </br></br>Sustituya el nombre distintivo, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt;DCName&gt;. Revise el resultado de la prueba para asegurarse de que se pasan las pruebas DNS. Si no tiene IPv6 habilitada en el controlador de dominio, debe esperar el host (AAAA) recursos registro parte de la prueba, que es una condición normal cuando IPv6 no está habilitado.
