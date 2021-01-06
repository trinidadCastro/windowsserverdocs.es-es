---
description: Más información acerca de cómo comprobar la funcionalidad de DNS para admitir la replicación de directorios
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Comprobación de la funcionalidad de DNS para admitir la replicación de directorios
ms.author: daveba
ms.date: 05/31/2017
author: Femila
ms.topic: troubleshooting
ms.openlocfilehash: 30a655932d4b19b50cce536545007080b13fd86d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949241"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Comprobación de la funcionalidad de DNS para admitir la replicación de directorios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Para comprobar la configuración del sistema de nombres de dominio (DNS) que puede interferir con la replicación de Active Directory, puede empezar por ejecutar la prueba básica que garantiza que DNS funciona correctamente para su dominio. Después de ejecutar la prueba básica, puede probar otros aspectos de la funcionalidad de DNS, incluido el registro de registros de recursos y la actualización dinámica.

Aunque puede ejecutar esta prueba de la funcionalidad DNS básica en cualquier controlador de dominio, normalmente ejecuta esta prueba en los controladores de dominio que cree que pueden estar experimentando problemas de replicación, por ejemplo, controladores de dominio que informen de los identificadores de evento 1844, 1925, 2087 o 2088 en el registro DNS del servicio de directorio de Visor de eventos.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Ejecutar la prueba de DNS básica del controlador de dominio</title>

La prueba de DNS básica comprueba los siguientes aspectos de la funcionalidad de DNS:


- **Conectividad:** La prueba determina si los controladores de dominio están registrados en DNS, se puede poner en contacto con el comando <system>ping</system> y tener conectividad de Protocolo ligero de acceso a directorios/llamada a procedimiento remoto (LDAP/RPC). Si se produce un error en la prueba de conectividad en un controlador de dominio, no se ejecuta ninguna otra prueba en ese controlador de dominio. La prueba de conectividad se realiza automáticamente antes de que se ejecute cualquier otra prueba de DNS.
- **Servicios esenciales:** La prueba confirma que los siguientes servicios se están ejecutando y están disponibles en el controlador de dominio probado: servicio de cliente DNS, servicio de inicio de sesión de red, servicio de Centro de distribución de claves (KDC) y servicio servidor DNS (si DNS está instalado en el controlador de dominio).
- **Configuración del cliente DNS:**  La prueba confirma que se puede tener acceso a los servidores DNS de todos los adaptadores de red del equipo cliente DNS.
- **Registros de registros de recursos:** La prueba confirma que el registro de recursos de host (A) de cada controlador de dominio está registrado en al menos uno de los servidores DNS que está configurado en el equipo cliente.
- **Zona e inicio de autoridad (SOA):** Si el controlador de dominio está ejecutando el servicio servidor DNS, la prueba confirma que los Active Directory zona de dominio y el registro de recursos de inicio de autoridad (SOA) para la zona de dominio Active Directory están presentes.
- **Zona raíz:** Comprueba si la zona raíz (.) está presente.

El requisito mínimo para completar estos procedimientos es la pertenencia al grupo administradores de organización o un grupo equivalente.

Puede usar el procedimiento siguiente para comprobar la funcionalidad básica de DNS.

### <a name="to-verify-basic-dns-functionality"></a>Para comprobar la funcionalidad básica de DNS:


1. En el controlador de dominio que desea probar o en un equipo miembro del dominio que tenga instaladas las herramientas de Active Directory Domain Services (AD DS), abra un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en **Inicio**.
2. En Iniciar búsqueda, escriba símbolo del sistema.
3. En la parte superior del menú Inicio, haga clic con el botón derecho en Símbolo del sistema y luego haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
4. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Sustituya el nombre distintivo real, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt; nombrededc &gt; . Como alternativa, puede probar todos los controladores de dominio en el bosque escribiendo/e: en lugar de/s:.
El modificador/f especifica un nombre de archivo, que en el comando anterior se dcdiagreport.txt. Si desea colocar el archivo en una ubicación distinta del directorio de trabajo actual, puede especificar una ruta de acceso de archivo, como/f:c:reportsdcdiagreport.txt.

5. Abra el archivo dcdiagreport.txt en el Bloc de notas o en un editor de texto similar. Para abrir el archivo en el Bloc de notas, en el símbolo del sistema, escriba notepad dcdiagreport.txt y, a continuación, presione Entrar. Si colocó el archivo en un directorio de trabajo diferente, incluya la ruta de acceso al archivo. Por ejemplo, si colocó el archivo en c:Reports, escriba notepad c:reportsdcdiagreport.txt y, a continuación, presione Entrar.
6. Desplácese a la tabla de resumen cerca de la parte inferior del archivo.
</br></br>Anote los nombres de todos los controladores de dominio que notifican el estado "ADVERTENCIA" o "error" en la tabla de resumen.  Intente determinar si hay un controlador de dominio problemático mediante la búsqueda de la sección de salida detallada mediante la búsqueda de la cadena "DC: Nombrededc", donde Nombrededc es el nombre real del controlador de dominio.

Si ve cambios de configuración obvios que son necesarios, hagalos, según corresponda. Por ejemplo, si observa que uno de los controladores de dominio tiene una dirección IP obviamente incorrecta, puede corregirlo. Después, vuelva a ejecutar la prueba.

Para validar los cambios de configuración, vuelva a ejecutar el comando dcdiag/test: DNS/v con el modificador/e: o/s:, según corresponda. Si no tiene la versión 6 de IP (IPv6) habilitada en el controlador de dominio, debe esperar que se produzca un error en la parte de validación del host (AAAA) de la prueba, pero si no usa IPv6 en la red, estos registros no son necesarios.

## <a name="verifying-resource-record-registration"></a>Comprobando el registro de registros de recursos

El controlador de dominio de destino utiliza el registro de recursos de alias DNS (CNAME) para localizar su asociado de replicación del controlador de dominio de origen. Aunque los controladores de dominio que ejecutan Windows Server (a partir de Windows Server 2003 con Service Pack 1 (SP1)) pueden localizar los asociados de replicación de origen mediante nombres de dominio completos (FQDN) o, si se produce un error, se espera la presencia de NetBIOS namesthe del registro de recursos de alias (CNAME) y debe comprobarse para que funcione correctamente el DNS.

Puede usar el procedimiento siguiente para comprobar el registro de registros de recursos, incluido el registro de registros de recursos de alias (CNAME).

### <a name="to-verify-resource-record-registrationtitle"></a>Para comprobar el registro de registros de recursos</title>


1. Abra un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio. En Iniciar búsqueda, escriba símbolo del sistema.
2. En la parte superior del menú Inicio, haga clic con el botón derecho en Símbolo del sistema y luego haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.  </br></br>Puede usar la herramienta dcdiag para comprobar el registro de todos los registros de recursos que son esenciales para la ubicación del controlador de dominio mediante la ejecución del `dcdiag /test:dns /DnsRecordRegistration` comando.

Este comando comprueba el registro de los siguientes registros de recursos en DNS:


- **alias (CNAME):** registro de recursos basado en el identificador único global (GUID) que ubica un asociado de replicación
- **host (A):**  el registro de recursos de host que contiene la dirección IP del controlador de dominio.
- **LDAP SRV:** los registros de recursos de servicio (SRV) que buscan servidores LDAP
- **GC SRV**: los registros de recursos de servicio (SRV) que buscan servidores de catálogo global
- **PDC SRV**: los registros de recursos de servicio (SRV) que buscan los maestros de operaciones del emulador de controlador de dominio principal (PDC)

Puede usar el procedimiento siguiente para comprobar el registro de registros de recursos de alias (CNAME) por sí solo.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Para comprobar el registro de registros de recursos de alias (CNAME)

1. Abra el complemento DNS. Para abrir DNS, haga clic en iniciar. En Iniciar búsqueda, escriba DNSMgmt. msc y, a continuación, presione Entrar. Si aparece el cuadro de diálogo control de cuentas de usuario, confirme que muestra la acción deseada y, a continuación, haga clic en continuar.
2. Use el complemento DNS para buscar cualquier controlador de dominio que ejecute el servicio servidor DNS, donde el servidor hospede la zona DNS con el mismo nombre que el dominio Active Directory del controlador de dominio.
3. En el árbol de consola, haga clic en la zona denominada _msdcs. Dns_Domain_Name.
4. En el panel de detalles, compruebe que están presentes los siguientes registros de recursos: un registro de recursos de alias (CNAME) que se denomina Dsa_Guid. _ msdcs. <placeholder>Dns_Domain_Name</placeholder> y un registro de recursos de host (a) correspondiente para el nombre del servidor DNS.

Si el registro de recursos de alias (CNAME) no está registrado, compruebe que la actualización dinámica funciona correctamente. Use la prueba de la siguiente sección para comprobar la actualización dinámica.

## <a name="verifying-dynamic-update"></a>Comprobando la actualización dinámica

Si la prueba de DNS básica muestra que los registros de recursos no existen en DNS, use la prueba de actualización dinámica para determinar por qué el servicio de net Logon no registró los registros de recursos automáticamente. Use el procedimiento siguiente para comprobar que la zona de dominio Active Directory está configurada para aceptar actualizaciones dinámicas seguras y para realizar el registro de un registro de prueba (_dcdiag_test_record). El registro de prueba se elimina automáticamente después de la prueba.

### <a name="to-verify-dynamic-updatetitle"></a>Para comprobar la actualización dinámica</title>


1. Abra un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio. En Iniciar búsqueda, escriba símbolo del sistema. En la parte superior del menú Inicio, haga clic con el botón derecho en Símbolo del sistema y luego haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
2. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Sustituya el nombre distintivo, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt; nombrededc &gt; . Como alternativa, puede probar todos los controladores de dominio en el bosque escribiendo/e: en lugar de/s:. Si no tiene IPv6 habilitado en el controlador de dominio, debe esperar que se produzca un error en la parte del registro de recursos del host (AAAA) de la prueba, que es una condición normal cuando IPv6 no está habilitado.

Si no se configuran las actualizaciones dinámicas seguras, puede usar el procedimiento siguiente para configurarlas.

### <a name="to-enable-secure-dynamic-updates"></a>Para habilitar las actualizaciones dinámicas seguras


1. Abra el complemento DNS. Para abrir DNS, haga clic en iniciar.
2. En Iniciar búsqueda, escriba DNSMgmt. msc y, a continuación, presione Entrar. Si aparece el cuadro de diálogo control de cuentas de usuario, confirme que muestra la acción deseada y, a continuación, haga clic en continuar.
3. En el árbol de consola, haga clic con el botón secundario en la zona correspondiente y, a continuación, haga clic en Propiedades.
4. En la ficha General, compruebe que el tipo de zona está Integrado en Active Directory.
5. En actualizaciones dinámicas, haga clic en solo protección.

## <a name="registering-dns-resource-records"></a>Registro de registros de recursos DNS

Si los registros de recursos DNS no aparecen en DNS para el controlador de dominio de origen, ha comprobado las actualizaciones dinámicas y desea registrar los registros de recursos DNS inmediatamente, puede forzar el registro manualmente mediante el procedimiento siguiente. El servicio Inicio de sesión de red de un controlador de dominio registra los registros de recursos DNS necesarios para que el controlador de dominio se encuentre en la red. El servicio cliente DNS registra el registro de recursos de host (A) al que apunta el registro de alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Para registrar manualmente los registros de recursos DNS</title>


1. Abra un símbolo del sistema como administrador. Para abrir un símbolo del sistema como administrador, haga clic en Inicio.
2. En Iniciar búsqueda, escriba símbolo del sistema.
3. En la parte superior del inicio, haga clic con el botón secundario en símbolo del sistema y, a continuación, haga clic en ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en Continuar.
4. Para iniciar el registro de los registros de recursos del localizador del controlador de dominio manualmente en el controlador de dominio de origen, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `net stop netlogon && net start netlogon`
5. Para iniciar manualmente el registro del registro de recursos de host (A), escriba el siguiente comando en el símbolo del sistema y presione ENTRAR: `ipconfig /flushdns && ipconfig /registerdns`
6. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `dcdiag /test:dns /v /s:<DCName>` </br></br>Sustituya el nombre distintivo, el nombre NetBIOS o el nombre DNS del controlador de dominio para &lt; nombrededc &gt; . Revise la salida de la prueba para asegurarse de que se han superado las pruebas de DNS. Si no tiene IPv6 habilitado en el controlador de dominio, debe esperar que se produzca un error en la parte del registro de recursos del host (AAAA) de la prueba, que es una condición normal cuando IPv6 no está habilitado.
