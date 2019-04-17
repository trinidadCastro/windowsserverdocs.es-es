---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: "Comprueba la funcionalidad de DNS para admitir la replicación de directorio"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: identity-adds
ms.author: billmath
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: 49f2795e1042438a50850fcb7fd8224eff2cca37
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Comprueba la funcionalidad de DNS para admitir la replicación de directorio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Para comprobar la configuración de sistema de nombres de dominio (DNS) que podría interferir con la replicación de Active Directory, puedes comenzar a ejecutar la prueba básica que garantiza que DNS funciona correctamente para el dominio. Después de ejecutar la prueba básica, puedes probar otros aspectos de la funcionalidad DNS, incluidos los registros de recursos y la actualización dinámica.

Aunque puedes ejecutar esta prueba de la funcionalidad básica de DNS en cualquier controlador de dominio, por lo general ejecutar esta prueba en controladores de dominio que creas que puede estar experimentando problemas de replicación, por ejemplo, los controladores de dominio dicho informe 1844 de identificadores de evento, 1925, 2087 o 2088 en el registro de servicio de directorio Visor DNS eventos.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Ejecuta la prueba DNS básica de controladores de dominio</title>

La prueba DNS básica comprueba los siguientes aspectos de la funcionalidad DNS:


- **Conectividad:** la prueba determina si se registran los controladores de DNS, el dominio puede ponerse en contacto el <system>ping</system> ordenar y tienen el protocolo ligero de acceso a directorios / conectividad (LDAP/RPC) de llamada a procedimiento remoto. Si se produce un error en la prueba de conectividad en un controlador de dominio, no hay otras pruebas se ejecutarán en ese controlador de dominio. Prueba de conectividad se realiza automáticamente antes de ejecuta cualquier otra prueba DNS.
- **Servicios esenciales:** la prueba confirma que los siguientes servicios están disponibles en el controlador de dominio probado y en ejecución: servicio de cliente de DNS, servicio de Net Logon, servicio del centro de distribución de claves (KDC) y el servicio de servidor DNS (si DNS está instalado en el controlador de dominio).
- **Configuración del cliente DNS:** la prueba confirma que los servidores DNS en todos los adaptadores de red del equipo cliente DNS son accesibles.
- **Registros de recursos:** la prueba confirma que el registro de recursos de host (A) de cada controlador de dominio se registra en al menos uno de los servidores DNS que está configurado en el equipo cliente.
- **Zona e inicio de autoridad (inicio de autoridad):** si el controlador de dominio está ejecutando el servicio de servidor DNS, la prueba confirma que la zona de dominio de Active Directory e inicio (inicio de autoridad) de la entidad de registro de recursos de la zona de dominio de Active Directory están presentes.
- **Zona raíz:** comprueba si existe la zona de raíz (.).

Pertenencia al grupo Administradores o equivalente, es lo mínimo necesario para completar estos procedimientos.

Puedes usar el siguiente procedimiento para comprobar la funcionalidad básica de DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Para comprobar la funcionalidad básica de DNS:


1. En el controlador de dominio que quieras probar o en un equipo miembro del dominio que tenga instaladas las herramientas los servicios de dominio de Active Directory (AD DS), abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haz clic en **inicio**. 
2. En la búsqueda de inicio, escribe el símbolo del sistema. 
3. En la parte superior del menú Inicio, haz clic en el símbolo del sistema y, a continuación, haz clic en Ejecutar como administrador. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que la acción que se muestra es lo que quieras y, a continuación, haga clic en continuar.
4. En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR: `dcdiag /test:dns /v /s: &lt;DCName&gt; /DnsBasic f:/dcdiagreport.txt`
</br></br>Sustituir el nombre completo real, un nombre NetBIOS o un nombre DNS del controlador de dominio para &lt;nombreDeDC&gt;. Como alternativa, puedes probar todos los controladores de dominio del bosque escribiendo e: en lugar de/s:. El modificador /f Especifica un nombre de archivo, que en el comando anterior es dcdiagreport.txt. Si quieres colocar el archivo en una ubicación diferente en el directorio de trabajo actual, puedes especificar una ruta de acceso de archivo, como /f:c:reportsdcdiagreport.txt.

5. Abre el archivo dcdiagreport.txt en el Bloc de notas o un editor de texto similares. Para abrir el archivo en el Bloc de notas, en el símbolo del sistema, escribe dcdiagreport.txt el Bloc de notas y, a continuación, presione ENTRAR. Si se coloca el archivo en un directorio de trabajo diferentes, incluir la ruta de acceso al archivo. Por ejemplo, si se coloca el archivo en c:reports, escriba c:reportsdcdiagreport.txt el Bloc de notas y, a continuación, presione ENTRAR.
6. Desplázate a la tabla de resumen en la parte inferior del archivo. 
</br></br>Ten en cuenta los nombres de todos los controladores de dominio que informe de estado "Advertir" o "No" en la tabla resumen.  Para intentar determinar si hay un controlador de dominio de problema buscando la sección desglose mediante la búsqueda de la cadena "DC nombreDeDC de:", donde nombreDeDC es el nombre real del controlador de dominio.

Si ves que los cambios de configuración obvio que son necesarias, haz que, según corresponda. Por ejemplo, Si observas que uno de los controladores de dominio tiene una dirección IP obviamente incorrecta, puedes corregirlo. A continuación, volver a ejecutar la prueba.

Para validar los cambios de configuración, volver a ejecutar el comando de /v Dcdiag/Test: DNS con el modificador/e: o/s:, según corresponda. Si no tienes IP versión 6 (IPv6) habilitado en el controlador de dominio, deberás suponer que la porción de validación de host (AAAA) de la prueba de errores, pero si no estás usando IPv6 en la red, estos registros no son necesarios.
            
## <a name="verifying-resource-record-registration"></a>Comprobación del registro de registro de recursos
    
El controlador de dominio de destino, utiliza el registro de recursos DNS alias (CNAME) para encontrar a su asociado de replicación del controlador de dominio de origen. Aunque los controladores de dominio ejecutan Windows Server (a partir de Windows Server 2003 con Service Pack 1 (SP1)) pueden buscar a duplicadores de origen con nombres de dominio completo (FQDN) o, si esto no funciona, NetBIOS rutasEl presencia del registro de recursos alias (CNAME) se espera y debe comprobarse para DNS correcto funcionamiento. 
      
Puedes usar el siguiente procedimiento para comprobar el registro de registro de recurso, incluido el registro de registro de recursos de alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Para comprobar los registros de recursos</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haz clic en Inicio. En la búsqueda de inicio, escribe el símbolo del sistema. 
2. En la parte superior del menú Inicio, haz clic en el símbolo del sistema y, a continuación, haz clic en Ejecutar como administrador. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que la acción que se muestra es lo que quieras y, a continuación, haga clic en continuar.  </br></br>Puedes usar la herramienta Dcdiag para comprobar el registro de todos los registros de recursos que son esenciales para la ubicación del controlador de dominio mediante la ejecución de la `dcdiag /test:dns /DnsRecordRegistration` comando.

Este comando comprueba el registro de los siguientes registros de recursos de DNS:


- **alias (CNAME):** el identificador único global (GUID)-en función de registro de recursos que busca un socio de replicación
- **host (A):** el registro de recursos de host que contiene la dirección IP del controlador de dominio
- **SRV LDAP:** los registros de recursos de servicio (SRV) que se ubican en los servidores LDAP
- **GC SRV**: los registros de recursos de servicio (SRV) que busque global los servidores de catálogo
- **PDC SRV**: los registros de recursos de servicio (SRV) que busque los maestros de operaciones de emulador de dominio principal (PDC) del controlador

Puedes usar el siguiente procedimiento para comprobar el alias (CNAME) recurso registro por sí solo.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Para comprobar los registros de recursos de alias (CNAME)

1. Abre el complemento de DNS. Para abrir DNS, haga clic en Inicio. En la búsqueda de inicio, escribe dnsmgmt.msc y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que esta se muestra la acción que desee y, a continuación, haz clic en continuar.
2. Usa el complemento de DNS para localizar cualquier controlador de dominio que está ejecutando el servicio de servidor DNS, donde el servidor hospeda la zona DNS con el mismo nombre que el dominio de Active Directory del controlador de dominio.
3. En el árbol de consola, haz clic en la zona que se denomina _msdcs. NombreDeDominioDns.
4. En el panel de detalles, compruebe que están presentes en los registros de recursos siguientes: un registro de recursos de alias (CNAME) que se denomina Dsa_Guid._msdcs. <placeholder>NombreDeDominioDNS</placeholder> y correspondiente (A) recurso registro de host para el nombre del servidor DNS.

Si no se registra el registro de recursos de alias (CNAME), comprueba que la actualización dinámica está funcionando correctamente. Usa la prueba en la siguiente sección para comprobar la actualización dinámica.
    
## <a name="verifying-dynamic-update"></a>Comprobar la actualización dinámica
    
Si la prueba DNS básica muestra que no existen registros de recursos de DNS, use la prueba de actualización dinámica para determinar por qué el servicio de Net Logon no registró los registros de recursos automáticamente. Para comprobar que la zona de dominio de Active Directory está configurada para aceptar las actualizaciones dinámicas seguras y realizar el registro de un registro de prueba (_dcdiag_test_record), usa el siguiente procedimiento. El registro de prueba se eliminará automáticamente después de la prueba.

### <a name="to-verify-dynamic-updatetitle"></a>Para comprobar la actualización dinámica</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haz clic en Inicio. En la búsqueda de inicio, escribe el símbolo del sistema. En la parte superior del menú Inicio, haz clic en el símbolo del sistema y, a continuación, haz clic en Ejecutar como administrador. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que la acción que se muestra es lo que quieras y, a continuación, haga clic en continuar.
2. En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR: 

    dcdiag/test: DNS /v s:&lt;nombreDeDC&gt; /DnsDynamicUpdate
</br></br>Sustituir el nombre completo, un nombre NetBIOS o un nombre DNS del controlador de dominio para &lt;nombreDeDC&gt;. Como alternativa, puedes probar todos los controladores de dominio del bosque escribiendo e: en lugar de/s:. Si no tienes IPv6 habilitado en el controlador de dominio, deberás suponer que el host (AAAA) recurso registro parte de la prueba de errores, que es una condición normal cuando IPv6 no está habilitado.

Si no se configuran las actualizaciones dinámicas seguras, puedes usar el siguiente procedimiento para configurarlos.

### <a name="to-enable-secure-dynamic-updates"></a>Para habilitar las actualizaciones dinámicas seguras


1. Abre el complemento de DNS. Para abrir DNS, haga clic en Inicio. 
2. En la búsqueda de inicio, escribe dnsmgmt.msc y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que esta se muestra la acción que desee y, a continuación, haz clic en continuar.
3. En el árbol de consola, haz clic en la zona correspondiente y, a continuación, haga clic en Propiedades.
4. En la pestaña General, comprueba que el tipo de zona está integrada en Active Directory.
5. En las actualizaciones dinámicas, haz clic en Secure solo.

## <a name="registering-dns-resource-records"></a>Registrar los registros de recursos DNS
    
Si los registros de recursos DNS no aparecen en DNS para el controlador de dominio de origen, comprobar las actualizaciones dinámicas y quieres registrar inmediatamente los registros de recursos DNS, puede hacer que manualmente mediante el siguiente procedimiento. El servicio de Net Logon en un controlador de dominio registra los registros de recursos DNS que son necesarios para el controlador de dominio que se encuentren en la red. El servicio cliente DNS registra el registro de recursos de host (A) que señala el registro de alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Para registrar manualmente los registros de recursos DNS</title>


1. Abre un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haz clic en Inicio. 
2. En la búsqueda de inicio, escribe el símbolo del sistema. 
3. En la parte superior de inicio, haz clic en el símbolo del sistema y, a continuación, haz clic en Ejecutar como administrador. Si aparece el cuadro de diálogo de Control de cuentas de usuario, confirma que la acción que se muestra es lo que quieras y, a continuación, haga clic en continuar.
4. Para iniciar el registro de recurso de ubicador de controlador de dominio registros manualmente en el controlador de dominio de origen, en el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR: `net stop net logon &amp; net start net logon`
5. Para iniciar el registro del host de registro de recursos (A) manualmente, en el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR: `ipconfig /flushdns &amp; ipconfig /registerdns`
6. En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona Intro: dcdiag/test: DNS /v s:&lt;nombreDeDC&gt; </br>,</br>Sustituir el nombre completo, un nombre NetBIOS o un nombre DNS del controlador de dominio para &lt;nombreDeDC&gt;. Revisa el resultado de la prueba para asegurarte de que ha pasado las pruebas DNS. Si no tienes IPv6 habilitado en el controlador de dominio, deberás suponer que el host (AAAA) recurso registro parte de la prueba de errores, que es una condición normal cuando IPv6 no está habilitado.
