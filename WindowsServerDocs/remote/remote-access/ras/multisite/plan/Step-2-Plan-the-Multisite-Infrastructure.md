---
title: Paso 2 planear la infraestructura multisitio
description: Obtenga información acerca de cómo completar la planeación de la infraestructura multisitio; incluye, Active Directory, grupos de seguridad y objetos directiva de grupo.
manager: brianlic
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 460cf6c921e50ae0026f742fa92ed252f9eb773b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941711"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Paso 2 planear la infraestructura multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente paso en la implementación de acceso remoto en una topología multisitio es completar la planeación de la infraestructura multisitio; incluye, Active Directory, grupos de seguridad y objetos directiva de grupo.
## <a name="21-plan-active-directory"></a><a name="bkmk_2_1_AD"></a>2,1 plan Active Directory
Una implementación multisitio de acceso remoto se puede configurar en varias topologías:

-   **Sitio de Active Directory único, varios puntos de entrada**: en esta topología, tiene un único sitio de Active Directory para toda la organización con vínculos de intranet rápidos en todo el sitio, pero tiene varios servidores de acceso remoto implementados en toda la organización, cada uno de ellos actúa como punto de entrada. Un ejemplo geográfico de esta topología es tener un único sitio Active Directory para el Estados Unidos con puntos de entrada en la costa este y la costa oeste.

    ![Infraestructura multisitio](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)

-   **Varios sitios de Active Directory, varios puntos de entrada**: en esta topología, tiene dos o más sitios de Active Directory con un servidor de acceso remoto implementado como punto de entrada para cada sitio. Cada servidor de acceso remoto está asociado con el Active Directory controlador de dominio para el sitio. Un ejemplo geográfico de esta topología es tener un sitio Active Directory para el Estados Unidos y otro para Europa con un único punto de entrada para cada sitio. Tenga en cuenta que si tiene varios sitios Active Directory no es necesario tener un punto de entrada asociado a cada sitio. Además, algunos sitios de Active Directory pueden tener más de un punto de entrada asociado.

    ![Topología multisitio](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)

En un punto de entrada multisitio, puede configurar un solo servidor de acceso remoto, varios servidores de acceso remoto o un clúster de servidores de acceso remoto.

### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory prácticas recomendadas y recomendaciones
Tenga en cuenta las siguientes recomendaciones y restricciones para la implementación de Active Directory en un escenario de multisitio:

1.  Cada sitio de Active Directory puede contener uno o más servidores de acceso remoto, o un clúster de servidores, que funcione como puntos de entrada multisitio para los equipos cliente. Sin embargo, no es necesario que un sitio Active Directory tenga un punto de entrada.

2.  Un punto de entrada multisitio solo se puede asociar a un único sitio Active Directory. Cuando los equipos cliente que ejecutan Windows 8 se conectan a un punto de entrada específico, se consideran como pertenecientes al Active Directory sitio asociado a ese punto de entrada.

3.  Se recomienda que cada sitio de Active Directory tenga un controlador de dominio. El controlador de dominio puede ser de solo lectura.

4.  Si cada Active Directory sitio contiene un controlador de dominio, el GPO de un servidor en el punto de entrada se administra mediante uno de los controladores de dominio en el Active Directory sitio asociado al extremo. Si no hay ningún controlador de dominio habilitado para escritura en ese sitio, el GPO de un servidor se administra en un controlador de dominio habilitado para escritura que se encuentra más cerca del primer servidor de acceso remoto que está configurado en el punto de entrada. Lo más cercano viene determinado por un cálculo del costo de los vínculos. Tenga en cuenta que en este escenario, después de realizar los cambios de configuración, es posible que se produzca un retraso al replicar entre el controlador de dominio que administra el GPO y el controlador de dominio de solo lectura en el sitio de Active Directory del servidor.

5.  Los GPO de cliente y los GPO de servidor de aplicaciones opcionales se administran en el controlador de dominio que se ejecuta como emulador de controlador de dominio principal (PDC). Esto significa que los GPO de cliente no se administran necesariamente en el Active Directory sitio que contiene el punto de entrada al que se conectan los clientes.

6.  Si no se puede tener acceso al controlador de dominio de un sitio Active Directory, el servidor de acceso remoto se conectará a un controlador de dominio alternativo en el sitio (si está disponible). Si no es así, se conecta al controlador de dominio de otro sitio para recuperar un GPO actualizado y autenticar a los clientes. En ambos casos, la consola de administración de acceso remoto y los cmdlets de PowerShell no se pueden usar para recuperar o modificar las opciones de configuración hasta que el controlador de dominio esté disponible. Tenga en cuenta lo siguiente:

    1.  Si el servidor que se ejecuta como el emulador de PDC no está disponible, debe designar un controlador de dominio disponible que tenga GPO actualizados como el emulador de PDC.

    2.  Si el controlador de dominio que administra un GPO de servidor no está disponible, use el cmdlet Set-DAEntryPointDC PowerShell para asociar un nuevo controlador de dominio al punto de entrada. El nuevo controlador de dominio debe tener GPO actualizados antes de ejecutar el cmdlet.

## <a name="22-plan-security-groups"></a><a name="bkmk_2_2_SG"></a>2,2 planear grupos de seguridad
Durante la implementación de un solo servidor con la configuración avanzada, todos los equipos cliente que tienen acceso a la red interna a través de DirectAccess se recopilaron en un grupo de seguridad. En una implementación multisitio, este grupo de seguridad solo se usa para equipos cliente de Windows 8. En una implementación multisitio, los equipos cliente de Windows 7 se recopilarán en grupos de seguridad independientes para cada punto de entrada de la implementación multisitio. Por ejemplo, si anteriormente ha agrupado todos los equipos cliente del grupo DA_Clients, ahora debe quitar todos los equipos con Windows 7 de ese grupo y colocarlos en un grupo de seguridad diferente. Por ejemplo, en la topología de varios sitios de Active Directory, varios puntos de entrada, cree un grupo de seguridad para el punto de entrada de Estados Unidos (DA_Clients_US) y otro para el punto de entrada de Europa (DA_Clients_Europe). Coloque los equipos cliente de Windows 7 ubicados en el Estados Unidos en el grupo de DA_Clients_US y en los que se encuentren en Europa en el grupo de DA_Clients_Europe. Si no tiene ningún equipo cliente de Windows 7, no es necesario planear los grupos de seguridad para equipos con Windows 7.

Los grupos de seguridad necesarios son los siguientes:

-   Un grupo de seguridad para todos los equipos cliente de Windows 8. Se recomienda crear un grupo de seguridad único para estos clientes para cada dominio.

-   Un grupo de seguridad único que contiene equipos cliente de Windows 7 para cada punto de entrada. Se recomienda crear un grupo único para cada dominio. Por ejemplo: domain1 \ DA_Clients_Europe; Dominio2 \ DA_Clients_Europe; Domain1 \ DA_Clients_US; Dominio2 \ DA_Clients_US.

## <a name="23-plan-group-policy-objects"></a><a name="bkmk_2_3_GPO"></a>2,3 planear objetos directiva de grupo
La configuración de DirectAccess configurada durante la implementación de acceso remoto se recopila en los GPO. La implementación de un solo servidor ya usa los GPO para los clientes de DirectAccess, el servidor de acceso remoto y, opcionalmente, para los servidores de aplicaciones. Una implementación multisitio requiere los siguientes GPO:

-   Un GPO de servidor para cada punto de entrada.

-   Un GPO para cada dominio que contenga equipos cliente que ejecuten Windows 8.

-   Un GPO para cada punto de entrada y cada dominio que contiene equipos cliente de Windows 7.

Los GPO se pueden configurar de la siguiente manera:

-   **Automáticamente**: puede especificar que el acceso remoto cree automáticamente los GPO. Se especifica un nombre predeterminado para cada GPO y se puede modificar.

-   **Manualmente**: puede usar GPO creados manualmente por el administrador de Active Directory.

> [!NOTE]
> Una vez que DirectAccess está configurado para usar GPO específicos, no se puede configurar para que use distintos GPO.

### <a name="231-automatically-created-gpos"></a>2.3.1 GPO creados automáticamente
Ten en cuenta lo siguiente al usar GPO creados automáticamente:

-   Los GPO creados automáticamente se aplican según los parámetros de ubicación y destino del vínculo, de la siguiente manera:

    -   Para el GPO de servidor, los parámetros de ubicación y de vínculo apuntan al dominio que contiene el servidor de acceso remoto.

    -   En el caso de los GPO de cliente, el destino del vínculo se establece en la raíz del dominio en el que se creó el GPO. Se crea un GPO para cada dominio que contiene equipos cliente y el GPO se vinculará a la raíz de cada dominio. .

-   En el caso de los GPO creados automáticamente, para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto requiere los siguientes permisos:

    -   Permisos de creación de GPO para los dominios necesarios.

    -   Permisos de vinculación para todas las raíces de dominios de clientes seleccionadas.

    -   Se requiere permiso para crear el filtro WMI para los GPO si DirectAccess se configuró para funcionar solo en equipos móviles.

    -   Permisos de vínculo para las raíces de los dominios asociados a los puntos de entrada (los dominios de GPO de servidor)

    -   Se recomienda que el administrador de acceso remoto tenga permisos de lectura en los GPO en todos los dominios necesarios. Esto permite que el acceso remoto Compruebe que no existen GPO con nombres duplicados al crear GPO para la implementación multisitio.

    -   Active Directory permisos de replicación en los dominios asociados a los puntos de entrada. Esto se debe a que cuando se agregan inicialmente puntos de entrada, el GPO de servidor para el punto de entrada se crea en el controlador de dominio más cercano a ese punto de entrada. Sin embargo, dado que la creación de vínculos solo se admite en el emulador de PDC, el GPO debe replicarse desde el controlador de dominio en el que se creó al controlador de dominio que se ejecuta como emulador de PDC antes de crear el vínculo.

Tenga en cuenta que si los permisos correctos para la replicación y los GPO de vinculación no existen, se emite una advertencia. La operación de acceso remoto continuará pero no se producirá la replicación ni la vinculación. Si se emite una advertencia de vínculo, los vínculos no se crearán automáticamente, incluso después de que se hayan implementado los permisos. En su lugar, el administrador deberá crear los vínculos manualmente.

### <a name="232-manually-created-gpos"></a>2.3.2 GPO creados manualmente
Ten en cuenta lo siguiente al usar GPO creados manualmente:

-   Los siguientes GPO se deben crear manualmente para la implementación multisitio:

    -   **GPO de servidor**: un GPO de servidor para cada punto de entrada (en el dominio en el que se encuentra el punto de entrada). Este GPO se aplicará en cada servidor de acceso remoto del punto de entrada.

    -   **GPO de cliente (Windows 7)**: un GPO para cada punto de entrada y cada dominio que contiene equipos cliente de Windows 7 que se conectarán a los puntos de entrada de la implementación multisitio. Por ejemplo, Domain1 \ DA_W7_Clients_GPO_Europe; Dominio2 \ DA_W7_Clients_GPO_Europe; Domain1 \ DA_W7_Clients_GPO_US; Dominio2 \ DA_W7_Clients_GPO_US. Si ningún equipo cliente de Windows 7 se conectará a los puntos de entrada, no se necesitan GPO.

-   No es necesario crear GPO adicionales para equipos cliente de Windows 8. Ya se creó un GPO para cada dominio que contiene equipos cliente al implementar el único servidor de acceso remoto. En una implementación multisitio, estos GPO de cliente funcionarán como los GPO para los clientes de Windows 8.

-   Los GPO deben existir antes de hacer clic en el botón confirmar en los asistentes para la implementación multisitio.

-   Al usar GPO creados manualmente, se busca un vínculo al GPO en todo el dominio. Si el GPO no está vinculado en el dominio, se creará un vínculo automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.

-   Al usar GPO creados manualmente, para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto necesita permisos de GPO completos (seguridad de edición, eliminación y modificación) en los GPO creados manualmente.

Tenga en cuenta que si los permisos correctos para la replicación y los GPO de vinculación no existen, se emite una advertencia. La operación de acceso remoto continuará pero no se producirá la replicación ni la vinculación. Si se emite una advertencia de vínculo, los vínculos no se crearán automáticamente, incluso después de que se hayan implementado los permisos. En su lugar, el administrador deberá crear los vínculos manualmente.

### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 administración de GPO en un entorno de controlador de varios dominios
Cada GPO se administrará mediante un controlador de dominio específico, como se indica a continuación:

-   El GPO de servidor es administrado por uno de los controladores de dominio del sitio Active Directory asociado con el servidor, o si los controladores de dominio de ese sitio son de solo lectura, por un controlador de dominio habilitado para escritura más cercano al primer servidor del punto de entrada.

-   Los GPO de cliente se administrarán mediante el controlador de dominio que se ejecuta como emulador de PDC.

Si desea modificar manualmente la configuración de GPO, tenga en cuenta lo siguiente:

-   En el caso de los GPO de servidor, para identificar qué controlador de dominio está asociado a un punto de entrada específico, ejecute el cmdlet de PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>` .

-   De forma predeterminada, cuando se realizan cambios con los cmdlets de PowerShell de red o la consola de administración de directiva de grupo, se usa el controlador de dominio que actúa como emulador de PDC.

-   Si modifica la configuración de un controlador de dominio que no es el controlador de dominio asociado con el punto de entrada (para GPO de servidor) o el emulador de PDC (para los GPO de cliente), tenga en cuenta lo siguiente:

    1.  Antes de modificar la configuración, asegúrese de que el controlador de dominio se replica con un GPO actualizado y [realice una copia de seguridad](https://go.microsoft.com/fwlink/?LinkID=257928)de la configuración del GPO antes de realizar los cambios. Si no se actualiza el GPO, pueden producirse conflictos de combinación durante la replicación, lo que resulta en una configuración de acceso remoto dañada.

    2.  Después de modificar la configuración, debe esperar a que los cambios se repliquen en el controlador de dominio asociado a los GPO. No realice cambios adicionales mediante la consola de administración de acceso remoto o los cmdlets de acceso remoto de PowerShell hasta que se complete la replicación. Si se edita un GPO en dos controladores de dominio distintos antes de que se complete la replicación, se podrían producir conflictos de combinación, lo que provocaría una configuración dañada.

-   También puede cambiar la configuración predeterminada mediante el cuadro de diálogo **cambiar el controlador de dominio** en la consola de administración de directiva de grupo, o mediante el cmdlet de PowerShell **Open-NetGPO** , de modo que los cambios realizados con los cmdlets de red o la consola de usen el controlador de dominio que especifique.

    1.  Para hacer esto en la consola de administración de directiva de grupo, haga clic con el botón secundario en el contenedor dominio o sitios y haga clic en **cambiar controlador de dominio**.

    2.  Para hacer esto en PowerShell, especifique el parámetro DomainController para el cmdlet Open-NetGPO. Por ejemplo, para habilitar los perfiles públicos y privados en firewall de Windows en un GPO denominado Domain1 \ DA_Server_GPO _Europe mediante un controlador de dominio denominado europe-dc.corp.contoso.com, haga lo siguiente:

        ```
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True
        Save-NetGPO -GpoSession $gpoSession
        ```

#### <a name="modifying-domain-controller-association"></a>Modificación de la Asociación del controlador de dominio
Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. En algunos escenarios, puede ser necesario asignar un controlador de dominio diferente para un GPO:

-   **Mantenimiento y tiempo de inactividad del controlador de dominio**: cuando el controlador de dominio que administra un GPO no está disponible, puede que sea necesario administrar el GPO en un controlador de dominio diferente.

-   **Optimización de la distribución de la configuración**: después de que cambie la infraestructura de red, puede ser necesario administrar el GPO de servidor de un punto de entrada en un controlador de dominio en el mismo Active Directory sitio que el punto de entrada.

## <a name="24-plan-dns"></a><a name="bkmk_2_4_DNS"></a>2,4 planear DNS
Tenga en cuenta lo siguiente al planear DNS para una implementación multisitio:

1.  Los equipos cliente usan la dirección ConnectTo para conectarse al servidor de acceso remoto. Cada punto de entrada de la implementación requiere una dirección ConnectTo diferente. Cada dirección ConnectTo de punto de entrada debe estar disponible en el DNS público y la dirección que elija debe coincidir con el nombre de sujeto del certificado IP-HTTPS que implemente para la conexión IP-HTTPS.

2.  Además, el acceso remoto exige el enrutamiento simétrico; por lo tanto, solo los clientes Teredo e IP-HTTPS pueden conectarse cuando se habilita una implementación multisitio. Para permitir que los clientes IPv6 nativos se conecten, la dirección ConnectTo (la dirección URL IP-HTTPS) debe resolverse en las direcciones IPv6 e IPv4 externas (con conexión a Internet) en el servidor de acceso remoto.

3.  El acceso remoto crea un sondeo web predeterminado que los equipos cliente de DirectAccess usan para comprobar la conectividad de la red interna. Durante la configuración del único servidor, se deben haber registrado los siguientes nombres en DNS:

    1.  DirectAccess-WebProbeHost: debe resolverse en la dirección IPv4 interna del servidor de acceso remoto o en la dirección IPv6 en un entorno de solo IPv6.

    2.  DirectAccess-CorpConnectivityHost: debe resolverse en una dirección de localhost (bucle invertido) (bien:: 1 o 127.0.0.1, dependiendo de si IPv6 está implementado en la red corporativa).

    En una implementación multisitio, se debe crear una entrada DNS adicional para DirectAccess-WebProbeHost para cada punto de entrada. Al agregar un punto de entrada, el acceso remoto intenta crear automáticamente esta entrada adicional de DirectAccess-WebProbeHost. Sin embargo, si se produce un error, se mostrará una advertencia y deberá crear la entrada manualmente.

    > [!NOTE]
    > Cuando la eliminación de registros obsoletos de DNS está habilitada en la infraestructura de DNS, se recomienda deshabilitar la eliminación de registros obsoletos en las entradas DNS creadas automáticamente por el acceso remoto.






