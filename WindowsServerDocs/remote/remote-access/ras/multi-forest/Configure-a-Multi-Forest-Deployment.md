---
title: Configure a Multi-Forest Deployment
description: En este tema forma parte de la Guía de implementación de acceso remoto en un entorno de varios bosques en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c8feff2-cae1-4376-9dfa-21ad3e4d5d99
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5321cc5339bdc38eec0726c108ec42fe0112c772
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812196"
---
# <a name="configure-a-multi-forest-deployment"></a>Configure a Multi-Forest Deployment

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se explica cómo configurar una implementación de Acceso remoto con varios bosques en distintos escenarios posibles. En todos esos escenarios se da por hecho que DirectAccess está implementado en un solo bosque (denominado Bosque1) y que DirectAccess se va a configurar para que funcione con un bosque nuevo denominado Bosque2.  
  
## <a name="AccessForest2"></a>Acceso a los recursos de Bosque2  
En este escenario, DirectAccess ya está implementado en Bosque1 y está configurado para que los clientes de Bosque1 puedan acceder a la red corporativa. De forma predeterminada, los clientes que se conectan a través de DirectAccess solo tienen acceso a los recursos de Bosque1 y, como tal, no pueden acceder a los servidores de Bosque2.  
  
#### <a name="to-enable-directaccess-clients-to-access-resources-from-forest2"></a>Para permitir que los clientes de DirectAccess accedan a los recursos de Bosque2  
  
1.  Si el sufijo DNS de Bosque2 no forma parte del sufijo DNS de Bosque1, agregue reglas NRPT con los sufijos de los dominios de Bosque2 y, si lo desea, agregue los sufijos de los dominios de Bosque2 a la lista de búsqueda de sufijos DNS.  
  
2.  En caso de que IPv6 esté implementado en la red interna, agregue los prefijos IPv6 internos pertinentes de Bosque2.  
  
## <a name="EnableForest2DA"></a>Permitir que los clientes de Bosque2 se conecten mediante DirectAccess  
En este escenario la implementación de Acceso remoto se va a configurar de forma que los clientes de Bosque2 puedan acceder a la red corporativa. Aquí se da por hecho que se han creado los grupos de seguridad necesarios para los equipos cliente de Bosque2.   
  
#### <a name="to-allow-clients-from-forest2-to-access-the-corporate-network"></a>Para permitir el acceso de los clientes de Bosque2 a la red corporativa  
  
1.  Agregue el grupo de seguridad de los clientes de Bosque2.  
  
2.  Si el sufijo DNS de Bosque2 no forma parte del sufijo DNS de Bosque1, agregue reglas NRPT con los sufijos de dominio de los clientes de Bosque2 para permitir el acceso a los controladores de dominio para la autenticación y, opcionalmente, agregue los sufijos de los dominios de Bosque2 a la suf DNS corregir la lista de búsqueda. 
  
3.  Agregue los prefijos IPv6 internos de Bosque2 para permitir que DirectAccess cree el túnel IPsec hacia los controladores de dominio para la autenticación.  
  
4.  Actualice la lista de servidores de administración.  
  
## <a name="AddEPForest2"></a>Agregar puntos de entrada de Bosque2  
En este escenario, DirectAccess está implementado en una configuración multisitio en Bosque1 y lo que se pretende es agregar un servidor de acceso remoto llamado DA2 desde Bosque2 a modo de punto de entrada a la implementación multisitio de DirectAccess existente.  
  
#### <a name="to-add-a-remote-access-server-from-forest2-as-an-entry-point"></a>Para agregar un servidor de acceso remoto desde Bosque2 como punto de entrada  
  
1.  Asegúrese de que el administrador de Acceso remoto posee permisos suficientes para escribir GPO en el dominio de DA2 y, asimismo, que dicho administrador es un administrador local en DA2.  
  
2.  Agregue DA2 como punto de entrada.   
  
3.  Agregue reglas NRPT con los sufijos de los dominios de Bosque2 para permitir el acceso a los controladores de dominio para la autenticación y, si lo desea, agregue los sufijos de los dominios de Bosque2 a la lista de búsqueda de sufijos DNS.  
  
4.  Agregue los prefijos IPv6 internos pertinentes de Bosque2 (si procede) para permitir que Acceso remoto establezca el túnel IPsec hacia los recursos corporativos, además de para confirmar que los sondeos de NCSI funcionan correctamente.  
  
5.  Actualice la lista de servidores de administración.  
  
## <a name="OTPMultiForest"></a>Configurar OTP en una implementación de varios bosques  
Tenga en cuenta los siguientes términos al configurar OTP en una implementación con varios bosques:  
  
-   Árbol principal de PKI de bosques de la entidad emisora de certificados CA raíz.  
  
-   Entidad de certificación empresarial, todo ello otras CA.  
  
-   Bosque de recursos, el bosque que contiene la entidad de certificación raíz y se considera la "administración bosque/dominio".  
  
-   Bosque Account-todos los otros bosques de la topología.  
  
En este procedimiento se necesita el script de PowerShell PKISync.ps1. Consulte [AD CS: Script PKISync.ps1 para la inscripción de certificados entre bosques](https://technet.microsoft.com/library/ff961506.aspx).  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
### <a name="BKMK_CertPub"></a>Configurar CA como editores de certificados  
  
1.  Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados para que se puedan realizar referencias LDAP en todos los CA empresariales de todos los bosques:  
  
    ```  
    certutil -setreg Policy\EditFlags +EDITF_ENABLELDAPREFERRALS  
    ```  
  
2.  Agregue todas las cuentas de equipo de CA empresariales a los grupos de seguridad Publicadores de certificados de Active Directory de cada uno de los bosques de cuenta.  
  
3.  Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados para reiniciar todos los servicios certsvc en todos los equipos de CA de todos los bosques:  
  
    ```  
    net stop certsvc && net start certsvc  
    ```  
  
4.  Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados para extraer el certificado de CA raíz:  
  
    ```  
    certutil -config <Computer-Name>\<Root-CA-Name> -ca.cert <root-ca-cert-filename.cer>  
    ```  
  
    (Si ejecuta el comando en la CA raíz puede omitir la información de conexión - config < Computer-Name >\\< nombre de la entidad de certificación raíz >)  
  
    1.  Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados para importar el certificado de CA raíz del paso anterior en la CA del bosque de cuenta:  
  
        ```  
        certutil -dspublish -f <root-ca-cert-filename.cer> RootCA  
        ```  
  
    2.  Permisos de lectura/escritura de plantillas de certificado de bosque de recursos de la concesión para el \<cuenta de bosque\>\\< cuenta de administrador\>.  
  
    3.  Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados para extraer todos los certificados de CA empresariales del bosque de recursos:  
  
        ```  
        certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
        ```  
  
        (Si ejecuta el comando en la CA raíz puede omitir la información de conexión - config < Computer-Name >\\< nombre de la entidad de certificación raíz >)  
  
    4.  Ejecute los siguientes comandos desde un símbolo del sistema con privilegios elevados para importar los certificados de CA empresariales del paso anterior en la CA del bosque de cuenta:  
  
        ```  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> NTAuthCA  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> SubCA  
        ```  
  
    5.  Quite las plantillas de certificado de OTP del bosque de cuenta de la lista de plantillas de certificado emitidas.  
  
### <a name="BKMK_DelImp"></a>Eliminar e importar plantillas de certificado OTP  
  
1.  Elimine las plantillas de certificado de OTP del bosque de cuenta (esto es, de Bosque2).  
  
2.  Use los siguientes comandos de PowerShell para copiar las plantillas de certificado y los objetos Oid del bosque de recursos a cada uno de los bosques de cuenta:  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <DA OTP registration authority template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <Secure DA OTP logon certificate template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Oid -f  
    ```  
  
### <a name="BKMK_Publish"></a>Publicar plantillas de certificado OTP  
  
-   Publique las plantillas de certificado recién importadas en todas las CA de los bosques de cuenta.  
  
### <a name="BKMK_Extract"></a>Extraer y sincronizar la entidad de certificación  
  
1.  Ejecute los siguientes comandos desde un símbolo del sistema con privilegios elevados para extraer todos los certificados de CA empresariales de los bosques de cuenta:  
  
    ```  
    certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
    ```  
  
2.  Use el siguiente comando de PowerShell para sincronizar las CA en todos los bosques, desde los boques de cuenta al bosque de recursos:  
  
    ```  
    .\PKISync.ps1 -sourceforest <account forest DNS> -targetforest <resource forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
3.  Use el siguiente comando de PowerShell para sincronizar las CA en todos los bosques, desde el bosque de recursos a los boques de cuenta:  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
## <a name="configuration-procedures"></a>Procedimientos de configuración  
En las siguientes secciones encontrará los procedimientos de configuración necesarios para las implementaciones de escenario anteriores. Cuando complete un procedimiento, vuelva al escenario para continuar.  
  
### <a name="NRPT_DNSSearchSuffix"></a>Agregar reglas NRPT y sufijos DNS  
Los clientes que se conectan con DirectAccess a la red corporativa usan la tabla de directivas de resolución de nombres (NRPT) para averiguar qué servidor DNS se debe usar para resolver la dirección de los distintos recursos. Esto permite al cliente resolver las direcciones de los recursos corporativos, al tiempo que le ayuda a distinguir adecuadamente entre lo que es o no corporativo, aspecto necesario para que DirectAccess siga funcionando. Las herramientas de configuración de DirectAccess detectan automáticamente el sufijo DNS raíz de Bosque1 y lo agrega a la tabla NRPT. Sin embargo, esto no ocurre con los sufijos FQDN de Bosque2, de modo que el administrador de Acceso remoto deberá agregarlos a la tabla manualmente.  
  
La lista de búsqueda de sufijos DNS permite que los clientes usen nombres de etiqueta cortos en lugar de FQDN. Las herramientas de configuración de Acceso remoto agregan automáticamente todos los dominios de Bosque1 a la lista de búsqueda de sufijos DNS. Si quiere que los clientes puedan usar nombres de etiqueta cortos para los recursos en Bosque2, deberá agregarlos manualmente a la lista.  
  
##### <a name="to-add-a-dns-suffix-to-the-nrpt-table-and-domain-suffixes-to-the-dns-suffix-search-list"></a>Para agregar un sufijo DNS a la tabla NRPT y sufijos de dominio a la lista de búsqueda de sufijos DNS  
  
1.  En el área **Paso 3: Servidores de infraestructura** del panel central de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En la página **Servidor de ubicación de red**, haga clic en **Siguiente**.  
  
3.  En la tabla de la página **DNS** , especifique los sufijos de nombre adicionales que formen parte de la red corporativa en Bosque2. En **Dirección de servidor DNS**, especifique la dirección del servidor DNS, ya sea manualmente o haciendo clic en **Detectar**. Si no especifica la dirección, se aplican las nuevas entradas como exenciones de la tabla NRPT. Haga clic en **Siguiente**.  
  
4.  Opcional: En la página **Lista de búsqueda de sufijos DNS**, agregue sufijos DNS especificándolos en el cuadro **Nuevo sufijo** y haciendo clic en **Agregar**. Haga clic en **Siguiente**.  
  
5.  En la página **Administración** , haga clic en **Finalizar**.  
  
6.  Haga clic en **Finalizar** en el panel central de la Consola de administración de acceso remoto.  
  
7.  En el cuadro de diálogo **Revisión de acceso remoto** , haga clic en **Aplicar**.  
  
8.  En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
### <a name="IPv6Prefix"></a>Agregar prefijo IPv6 interno  
  
> [!NOTE]  
> Agregar un prefijo IPv6 interno solo procede cuando IPv6 está implementado en la red interna.  
  
Acceso remoto administra una lista de prefijos IPv6 para los recursos corporativos. Por lo tanto, los clientes que se conecten mediante DirectAccess solo podrán acceder a aquellos recursos que tengan esos prefijos IPv6. Dado que la consola de administración de acceso remoto y los comandos de Windows PowerShell automáticamente agregan prefijos IPv6 de Bosque1 y no se pueden agregar los prefijos de otros bosques, debe agregar los prefijos que falten de Bosque2 manualmente.  
  
##### <a name="to-add-an-ipv6-prefix"></a>Para agregar un prefijo IPv6  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel central de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En el Asistente para instalación de acceso remoto, haga clic en **Configuración de prefijo**.  
  
3.  En **Prefijos IPv6 de la red interna** en dicha**** página, agregue prefijos IPv6 separados por puntos y coma (por ejemplo, 2001:db8:1::/64;2001:db8:2::/64). Haga clic en **Siguiente**.  
  
4.  En la página **Autenticación** , haga clic en **Finalizar**.  
  
5.  Haga clic en **Finalizar** en el panel central de la Consola de administración de acceso remoto.  
  
6.  En el cuadro de diálogo **Revisión de acceso remoto** , haga clic en **Aplicar**.  
  
7.  En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
### <a name="SGs"></a>Agregar grupos de seguridad de cliente  
Para habilitar los equipos cliente de Windows 8 de Bosque2 para tener acceso a los recursos a través de DirectAccess, debe agregar el grupo de seguridad de Bosque2 a la implementación de acceso remoto.  
  
##### <a name="to-add-windows-8-client-security-groups"></a>Para agregar grupos de seguridad de cliente de Windows 8  
  
1.  En el área **Paso 1: Clientes remoto** del panel central de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En el Asistente para la instalación del cliente de DirectAccess, haga clic en **Seleccionar grupos** y, en la página **Seleccionar grupos**, haga clic en **Agregar**.  
  
3.  En el cuadro de diálogo **Seleccionar grupos**, seleccione los grupos de seguridad que contengan a los equipos cliente de DirectAccess. Haga clic en **Siguiente**.  
  
4.  En la página **Asistente para la conectividad de red**, haga clic en **Finalizar**.  
  
5.  Haga clic en **Finalizar** en el panel central de la Consola de administración de acceso remoto.  
  
6.  En el cuadro de diálogo **Revisión de acceso remoto** , haga clic en **Aplicar**.  
  
7.  En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
Para habilitar el 7 de Windows que los equipos cliente de Bosque2 para acceder a recursos a través de DirectAccess cuando multisitio está habilitada, debe agregar el grupo de seguridad de Bosque2 a la implementación de acceso remoto para cada punto de entrada. Para obtener información acerca de cómo agregar grupos de seguridad de Windows 7, vea la descripción de la **compatibilidad con clientes** página 3.6. Habilitar la implementación multisitio.  
  
### <a name="RefreshMgmtServers"></a>Actualizar la lista de servidores de administración  
Acceso remoto detecta de manera automática los servidores de infraestructura que hay en todos los bosques que contienen GPO de configuración de DirectAccess. Si DirectAccess se implementó en un servidor de Bosque1, el GPO de servidor se escribirá en su dominio de Bosque1. Si se permitió el acceso a DirectAccess a los clientes de Bosque2, el GPO de cliente se escribirá en un dominio de Bosque2.  
  
El proceso de detección automática de servidores de infraestructura es necesario para permitir el acceso mediante DirectAccess a los controladores de dominio y a System Center Configuration Manager. Debe iniciar el proceso de detección manualmente.  
  
##### <a name="to-refresh-the-management-servers-list"></a>Para actualizar la lista de servidores de administración  
  
1.  En la Consola de administración de acceso remoto, haga clic en **Configuración** y, en el panel **Tareas**, haga clic en **Actualizar servidores de administración**.  
  
2.  En el cuadro de diálogo **Actualizando servidores de administración** , haga clic en **Cerrar**.  
  


