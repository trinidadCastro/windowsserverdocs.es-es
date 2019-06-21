---
title: Paso 2 Plan la infraestructura de multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2655f4de83576ef62b113419a69badaacc868f9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280998"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Paso 2 Plan la infraestructura de multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Es el siguiente paso de implementación de acceso remoto en una topología de multisitio completar la planificación de infraestructura multisitio; como Active Directory, los grupos de seguridad y objetos de directiva de grupo.  
## <a name="bkmk_2_1_AD"></a>2.1 planear Active Directory  
Una implementación multisitio de acceso remoto puede configurarse en un número de topologías:  
  
-   **Sitio de Active Directory único, varios puntos de entrada**-en esta topología, tiene un único sitio de Active Directory para toda la organización con vínculos rápidos de intranet en todo el sitio, pero tendrá que implementar varios servidores de acceso remoto a lo largo de su organización, cada que actúa como punto de entrada. Un ejemplo de esta topología geográfico es tener un único sitio de Active Directory para los Estados Unidos con puntos de entrada en la costa este y la costa oeste.  
  
    ![Infraestructura de multisitio](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **Varios sitios de Active Directory, varios puntos de entrada**-en esta topología, tiene dos o más sitios de Active Directory con un servidor de acceso remoto implementado como un punto de entrada para cada sitio. Cada servidor de acceso remoto está asociado con el controlador de dominio de Active Directory para el sitio. Un ejemplo de esta topología geográfico es tener un sitio de Active Directory para los Estados Unidos y otro para Europa con un único punto de entrada para cada sitio. Tenga en cuenta que si tiene varios sitios de Active Directory no es necesario tener un punto de entrada asociado con cada sitio. Además, algunos sitios de Active Directory pueden tener más de un punto de entrada asociado con él.  
  
    ![Topología de multisitio](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
En un punto de entrada multisitio, puede configurar un solo servidor de acceso remoto, varios servidores de acceso remoto, o un servidor de acceso remoto de clústeres.   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Recomendaciones y prácticas recomendadas de Active Directory  
Tenga en cuenta las siguientes recomendaciones y restricciones para la implementación de Active Directory en un escenario multisitio:  
  
1.  Cada sitio de Active Directory puede contener uno o varios servidores de acceso remoto o un clúster de servidores, funciona como puntos de entrada multisitio para los equipos cliente. Sin embargo, un sitio de Active Directory no es necesario tener un punto de entrada.  
  
2.  Un punto de entrada multisitio solo puede asociarse con un único sitio de Active Directory. Cuando los equipos cliente que ejecutan Windows 8 se conectan a un punto de entrada concreto, se consideran como pertenecientes al sitio de Active Directory asociado con ese punto de entrada.  
  
3.  Se recomienda que cada sitio de Active Directory tiene un controlador de dominio. El controlador de dominio puede ser de solo lectura.  
  
4.  Si cada sitio de Active Directory contiene un controlador de dominio, el GPO para un servidor en el punto de entrada es administrado por uno de los controladores de dominio en el sitio de Active Directory asociado con el punto de conexión. Si no hay ningún controlador de dominio de escritura habilitados en ese sitio, el GPO para un servidor se administra en un controlador de dominio habilitado para escritura que se encuentra más cercano al primer servidor de acceso remoto que está configurado en el punto de entrada. Más próximo viene determinada por un vínculo de cálculo de costos. Tenga en cuenta que en este escenario, después de realizar los cambios de configuración que podría haber un retraso al replicar entre el controlador de dominio administrar lo GPO y el controlador de dominio de solo lectura en el sitio de Active Directory del servidor.  
  
5.  Los GPO de cliente y servidor de aplicaciones opcional que los GPO se administran en el controlador de dominio que se ejecuta como el emulador de controlador de dominio principal (PDC). Esto significa que los GPO no se administran necesariamente en el sitio de Active Directory que contiene el punto de entrada de cliente para que los clientes conectarse.  
  
6.  Si el controlador de dominio para un sitio de Active Directory es inaccesible, el servidor de acceso remoto se conectará a un controlador de dominio alternativo en el sitio (si está disponible). Si no es así, se conecta al controlador de dominio de otro sitio para recuperar un GPO actualizado y para autenticar a los clientes. En ambos casos, la consola de administración de acceso remoto y cmdlets de PowerShell no puede usarse para recuperar o modificar la configuración hasta que el controlador de dominio esté disponible. Tenga en cuenta lo siguiente:  
  
    1.  Si el servidor que se ejecuta como el emulador de PDC está disponible, debe designar un controlador de dominio disponible que se ha actualizado un GPO como el emulador de PDC.  
  
    2.  Si el controlador de dominio que administra un GPO de servidor no está disponible, use el cmdlet Set-DAEntryPointDC PowerShell para asociar un nuevo controlador de dominio con el punto de entrada. El nuevo controlador de dominio debe tener un GPO actualizado antes de ejecutar el cmdlet.  
  
## <a name="bkmk_2_2_SG"></a>2.2 Planear grupos de seguridad  
Durante la implementación de un único servidor con configuración avanzada, todos los equipos cliente tener acceso a la red interna a través de DirectAccess se recopilan en un grupo de seguridad. En una implementación multisitio, se usa este grupo de seguridad para que sólo los equipos de cliente de Windows 8. Para una implementación multisitio, los equipos cliente de Windows 7 se recopilarán en grupos de seguridad independientes para cada punto de entrada en la implementación multisitio. Por ejemplo, si anteriormente se agrupan todos los equipos cliente en el grupo clientes_da, ahora debe quitar los equipos de Windows 7 de ese grupo y colocarlos en un grupo de seguridad diferente. Por ejemplo, en los varios sitios de Active Directory, topología de puntos de entrada de varios, cree un grupo de seguridad para el punto de entrada de Estados Unidos (DA_Clients_US) y otra para el punto de entrada de Europa (DA_Clients_Europe). Coloque los equipos cliente de Windows 7 en los Estados Unidos en el grupo DA_Clients_US y cualquier ubicada en Europa en el grupo DA_Clients_Europe. Si no tiene los equipos cliente de Windows 7, no es necesario planear grupos de seguridad para equipos con Windows 7.  
  
Grupos de seguridad necesarios son los siguientes:  
  
-   Un grupo de seguridad para todos los equipos cliente de Windows 8. Se recomienda crear un grupo de seguridad único para estos clientes para cada dominio.  
  
-   Un grupo de seguridad único que contiene los equipos cliente de Windows 7 para cada punto de entrada. Se recomienda crear un grupo único para cada dominio. Por ejemplo: Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 objetos de directiva de grupo de planes  
Configuración de DirectAccess configurada durante la implementación de acceso remoto se recopila en GPO. La implementación de servidor único ya usa los GPO para los clientes de DirectAccess, el servidor de acceso remoto y, opcionalmente, para los servidores de aplicaciones. Una implementación multisitio requiere los siguientes GPO:  
  
-   Un GPO de servidor para cada punto de entrada.  
  
-   Un GPO para cada dominio que contenga equipos cliente que ejecutan Windows 8.  
  
-   Un GPO para cada punto de entrada y cada dominio que contenga equipos cliente de Windows 7.  
  
Los GPO se pueden configurar como sigue:  
  
-   **Automáticamente**-puede especificar que los GPO se crean automáticamente con el acceso remoto. Un nombre predeterminado se especifica para cada GPO y puede modificarse.  
  
-   **Manualmente**: puede usar los GPO que se han creado manualmente por el Administrador de Active Directory.  
  
> [!NOTE]  
> Después de configurar DirectAccess para que use unos GPO específicos, no puede configurarse para que use GPO distintos.  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 GPO creados automáticamente  
Ten en cuenta lo siguiente al usar GPO creados automáticamente:  
  
-   Los GPO creados automáticamente se aplican según los parámetros de ubicación y destino del vínculo, de la siguiente manera:  
  
    -   Para el GPO de servidor, los parámetros de la ubicación y el vínculo apuntan al dominio que contiene el servidor de acceso remoto.  
  
    -   Para los GPO de cliente, el destino de vínculo se establece en la raíz del dominio en el que se creó el GPO. Se crea un GPO para cada dominio que contenga equipos cliente y el GPO se vinculará a la raíz de cada dominio. .  
  
-   Para los GPO creados automáticamente, para aplicar la configuración de DirectAccess el acceso remoto administrador del servidor requiere los permisos siguientes:  
  
    -   Creación de GPO permisos para los dominios necesarios.  
  
    -   Permisos de vinculación para todas las raíces de dominios de clientes seleccionadas.  
  
    -   Se requiere el permiso para crear el filtro WMI para los GPO si DirectAccess se ha configurado para funcionar solo en equipos móviles.  
  
    -   Permisos de vinculación para las raíces de dominios asociados a los puntos de entrada (los dominios de GPO de servidor)  
  
    -   Se recomienda que el administrador de acceso remoto tenga permisos de lectura en los GPO en todos los dominios necesarios. Esto permite el acceso remoto comprobar que no existen los GPO con nombres duplicados al crear los GPO para la implementación multisitio.  
  
    -   Permisos de replicación de Active Directory en dominios asociados a puntos de entrada. Esto es porque cuando se agregan inicialmente los puntos de entrada, se crea el GPO de servidor del punto de entrada en el controlador de dominio más cercano a dicho punto de entrada. Sin embargo, dado que se admite la creación de vínculos en el emulador PDC solo, el GPO debe replicarse desde el controlador de dominio en el que se creó para el controlador de dominio que se ejecuta como el emulador de PDC antes de crear el vínculo.  
  
Tenga en cuenta que si no dispone de los permisos correctos para la replicación y vinculación de GPO, se emitirá una advertencia. La operación de acceso remoto continuará pero no se realizará la replicación y vinculación. Si un vínculo de advertencia se emite vínculos no se creará automáticamente, incluso después de que los permisos están en su lugar. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
### <a name="232-manually-created-gpos"></a>2.3.2 GPO creados manualmente  
Ten en cuenta lo siguiente al usar GPO creados manualmente:  
  
-   Los siguientes GPO deben crearse manualmente para la implementación multisitio:  
  
    -   **GPO de servidor**- un GPO de servidor para cada punto de entrada (en el dominio donde se encuentra el punto de entrada). Este GPO se aplicará en cada servidor de acceso remoto en el punto de entrada.  
  
    -   **(Windows 7) del GPO de cliente**- un GPO para cada punto de entrada y cada dominio que contenga equipos cliente de Windows 7 que se conectarán a puntos de entrada en la implementación multisitio. Por ejemplo Domain1\DA_W7_Clients_GPO_Europe; Domain2\DA_W7_Clients_GPO_Europe; Domain1\DA_W7_Clients_GPO_US; Domain2\DA_W7_Clients_GPO_US. Si no hay equipos cliente de Windows 7 se conectan a puntos de entrada, los GPO no son necesarios.  
  
-   No hay ningún requisito para crear equipos de cliente adicional de los GPO para Windows 8. Ya se creó un GPO para cada dominio que contenga equipos cliente cuando se implementó el servidor de acceso remoto único. En una implementación multisitio estos GPO de cliente funcionará como los clientes de los GPO para Windows 8.  
  
-   Los GPO deben existir antes de hacer clic en el botón de confirmación en los asistentes de implementación multisitio.  
  
-   Al usar GPO creados manualmente, se busca un vínculo al GPO en todo el dominio. Si el GPO no está vinculado en el dominio, se creará un vínculo automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
-   Al usar GPO creados manualmente, para aplicar la configuración de DirectAccess el administrador del servidor de acceso remoto necesita permisos totales (Editar, eliminar, modificar seguridad) en los GPO creados manualmente.  
  
Tenga en cuenta que si no dispone de los permisos correctos para la replicación y vinculación de GPO, se emitirá una advertencia. La operación de acceso remoto continuará pero no se realizará la replicación y vinculación. Si un vínculo de advertencia se emite vínculos no se creará automáticamente, incluso después de que los permisos están en su lugar. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 administrar GPO en un entorno de controladores multidominio  
Cada GPO se administrarán mediante un controlador de dominio específico, como sigue:  
  
-   El GPO de servidor es administrado por uno de los controladores de dominio en el sitio de Active Directory asociado con el servidor, o si los controladores de dominio de ese sitio son de solo lectura, un controlador de dominio habilitado para escritura más cercano al primer servidor en la entrada de punto.  
  
-   Los GPO de cliente se administrarán mediante el controlador de dominio que se ejecuta como el emulador de PDC.  
  
Si desea manualmente modifique la siguiente nota de la configuración de GPO:  
  
-   Para GPO, de servidor con el fin de identificar qué controlador de dominio está asociado con un punto de entrada específica, ejecute el cmdlet de PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>`.  
  
-   De forma predeterminada, cuando se realizan cambios con la red de los cmdlets de PowerShell o la consola de administración de directivas de grupo, se usa el controlador de dominio que actúa como el emulador de PDC.  
  
-   Si modifica la configuración en un controlador de dominio que no sea el controlador de dominio asociado con el punto de entrada (para el GPO de servidor) o en el emulador PDC (para los GPO de cliente), tenga en cuenta lo siguiente:  
  
    1.  Antes de modificar la configuración, asegúrese de que el controlador de dominio se replica con un GPO actualizado, y [copia de seguridad de la configuración de GPO](https://go.microsoft.com/fwlink/?LinkID=257928), antes de realizar cambios. Si no se actualiza el GPO, conflictos de combinación durante la replicación podrían producirse, resultante en una configuración de acceso remoto está dañada.  
  
    2.  Después de modificar la configuración, debe esperar para que los cambios se repliquen en el controlador de dominio que está asociado con los GPO. No realice cambios adicionales mediante la consola de administración de acceso remoto o los cmdlets de PowerShell de acceso remoto hasta que se complete la replicación. Si se edita un GPO en dos controladores de dominio diferente antes de que finalice la replicación, los conflictos de combinación pueden producirse, resultante en una configuración dañada  
  
-   Como alternativa puede cambiar la configuración predeterminada mediante el **cambiar el controlador de dominio** diálogo cuadro en la consola de administración de directivas de grupo, o mediante el **Open-NetGPO** cmdlet de PowerShell, por lo que los cambios realizadas mediante la consola o los cmdlets de red utiliza el controlador de dominio que especifique.  
  
    1.  Para hacer esto en la consola de administración de directivas de grupo, haga clic en el contenedor de sitios o de dominio y haga clic en **cambiar el controlador de dominio**.  
  
    2.  Para hacer esto en PowerShell, especifique el parámetro DomainController del cmdlet Open-NetGPO. Por ejemplo, para permitir que los perfiles públicos y privados en Firewall de Windows en un GPO llamado _Europe domain1\DA_Server_GPO mediante un controlador de dominio llamado europe-dc.corp.contoso.com, realice lo siguiente:  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>Modificar la asociación del controlador de dominio  
Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. En algunos escenarios, es posible que se requiere para asignar un controlador de dominio diferentes para un GPO:  
  
-   **Mantenimiento del controlador de dominio y el tiempo de inactividad**-cuando el controlador de dominio que administra un GPO no está disponible, es posible que deba administrar GPO en un controlador de dominio diferente.  
  
-   **Optimización de distribución de configuración**-después de que cambie la infraestructura de red, puede ser necesario para administrar lo GPO de servidor de un punto de entrada en un controlador de dominio en el mismo sitio de Active Directory que el punto de entrada.   
  
## <a name="bkmk_2_4_DNS"></a>2.4 planear DNS  
Al planear DNS para una implementación multisitio, tenga en cuenta lo siguiente:  
  
1.  Los equipos cliente usan la dirección ConnectTo para conectarse al servidor de acceso remoto. Cada punto de entrada en la implementación requiere una dirección de ConnectTo diferente. Cada dirección ConnectTo del punto de entrada debe estar disponible en el DNS público y la dirección que elijas debe coincidir con el nombre del sujeto del certificado IP-HTTPS implementes para la conexión IP-HTTPS.   
  
2.  Además, el acceso remoto aplica enrutamiento simétrico; por lo tanto, solo los clientes Teredo e IP-HTTPS pueden conectarse cuando se habilita una implementación multisitio. Para permitir que los clientes nativos de IPv6 para conectarse, debe resolver la dirección ConnectTo (la dirección URL IP-HTTPS) a ambas externo (orientada a Internet) IPv6 e IPv4 direcciones en el servidor de acceso remoto.  
  
3.  El acceso remoto crea un sondeo web predeterminado que los equipos cliente de DirectAccess usan para comprobar la conectividad de la red interna. Durante la configuración del servidor solo de los siguientes nombres deben se han registrado en DNS:  
  
    1.  DirectAccess-WebProbeHost: debe resolver la dirección IPv4 interna del servidor de acceso remoto, o la dirección IPv6 en un entorno de solo IPv6.  
  
    2.  DirectAccess-CorpConnectivityHost: debe resolver para una dirección de host local (bucle invertido) (ya sea:: 1 o 127.0.0.1, dependiendo de si se implementa IPv6 en la red corporativa).  
  
    En una implementación multisitio, se debe crear una entrada DNS adicional para directaccess-WebProbeHost para cada punto de entrada. Al agregar un punto de entrada, acceso remoto intenta crear automáticamente esta entrada de directaccess-WebProbeHost adicionales. Sin embargo, si se produce un error se mostrará una advertencia y debe crear manualmente la entrada.  
  
    > [!NOTE]  
    > Cuando está habilitada la limpieza de DNS en la infraestructura DNS, se recomienda deshabilitar la limpieza de las entradas DNS creadas automáticamente el acceso remoto.  
  

  



