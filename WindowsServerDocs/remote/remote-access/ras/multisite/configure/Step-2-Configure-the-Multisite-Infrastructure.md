---
title: Paso 2 configuración de la infraestructura multisitio
description: Obtenga información acerca de cómo configurar una implementación multisitio. hay una serie de pasos necesarios para modificar la configuración de la infraestructura de red.
manager: brianlic
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 04181093bac2bb70d9832e88dd90df79155ad5d9
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941741"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Paso 2 configuración de la infraestructura multisitio

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

Para configurar una implementación multisitio, hay una serie de pasos necesarios para modificar la configuración de la infraestructura de red, entre las que se incluyen: configurar sitios de Active Directory adicionales y controladores de dominio, configurar grupos de seguridad adicionales y configurar objetos de directiva de grupo (GPO) si no se usan GPO configurados automáticamente.

|Tarea|Descripción|
|----|--------|
|2.1. Configurar sitios de Active Directory adicionales|Configurar sitios de Active Directory adicionales para la implementación.|
|2.2. Configurar controladores de dominio adicionales|Configure controladores de dominio de Active Directory adicionales según sea necesario.|
|2.3. Configurar grupos de seguridad|Configure los grupos de seguridad para todos los equipos cliente de Windows 7.|
|2.4. Configurar GPO|Configure objetos de directiva de grupo adicionales según sea necesario.|

> [!NOTE]
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [Uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="21-configure-additional-active-directory-sites"></a><a name="BKMK_ConfigAD"></a>2.1. Configurar sitios de Active Directory adicionales
Todos los puntos de entrada pueden residir en un único sitio Active Directory. Por lo tanto, se requiere al menos un sitio Active Directory para la implementación de servidores de acceso remoto en una configuración multisitio. Utilice este procedimiento si necesita crear el primer Active Directory sitio o si desea usar sitios de Active Directory adicionales para la implementación multisitio. Use el complemento sitios y servicios de Active Directory para crear nuevos sitios en la red de la organización.

Para completar este procedimiento, es necesario pertenecer al grupo **administradores de empresas** del bosque o al grupo **Admins** . del dominio del dominio raíz del bosque, o un grupo equivalente. Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

Para obtener más información, consulte [Agregar un sitio al bosque](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732761(v=ws.11)).

### <a name="to-configure-additional-active-directory-sites"></a>Para configurar sitios de Active Directory adicionales

1.  En el controlador de dominio principal, haga clic en **Inicio** y, a continuación, haga clic en **Active Directory sitios y servicios**.

2.  En la consola sitios y servicios de Active Directory, en el árbol de consola, haga clic con el botón secundario en **sitios** y, a continuación, haga clic en **nuevo sitio**.

3.  En el cuadro de diálogo **nuevo objeto-sitio** , en el cuadro **nombre** , escriba un nombre para el nuevo sitio.

4.  En **nombre de vínculo**, haga clic en un objeto de vínculo a sitios y, a continuación, haga clic en **Aceptar** dos veces.

5.  En el árbol de consola, expanda **sitios**, haga clic con el botón secundario en **subredes** y, a continuación, haga clic en **nueva subred**.

6.  En el cuadro de diálogo **nuevo objeto-subred** , en **prefijo**, escriba el prefijo de subred IPv4 o IPv6, en la lista **Seleccione un objeto de sitio para este prefijo** , haga clic en el sitio para asociarlo a esta subred y, a continuación, haga clic en **Aceptar**.

7.  Repita los pasos 5 y 6 hasta que haya creado todas las subredes necesarias en la implementación.

8.  Cierre Sitios y servicios de Active Directory.

:::image type="icon" source="../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif":::**_<em>Comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Para instalar la característica de Windows "Active Directory módulo para Windows PowerShell":

```
Install-WindowsFeature "Name RSAT-AD-PowerShell
```

o bien, agregue el "complemento de PowerShell de Active Directory" a través de OptionalFeatures.

Si ejecuta los siguientes cmdlets en Windows 7 o Windows Server 2008 R2, se debe importar el módulo de PowerShell de Active Directory:

```
Import-Module ActiveDirectory
```

Para configurar un sitio de Active Directory denominado "Second-site" con el DEFAULTIPSITELINK integrado:

```
New-ADReplicationSite -Name "Second-Site"
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}
```

Para configurar subredes IPv4 e IPv6 para el segundo sitio:

```
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"
```

## <a name="22-configure-additional-domain-controllers"></a><a name="BKMK_AddDC"></a>2.2. Configurar controladores de dominio adicionales
Para configurar una implementación multisitio en un único dominio, se recomienda tener al menos un controlador de dominio grabable para cada sitio de la implementación.

Para llevar a cabo este procedimiento, debe ser, como mínimo, miembro del grupo Admins. del dominio en el dominio en el que se va a instalar el controlador de dominio.

Para obtener más información, consulte [instalación de un controlador de dominio adicional](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc733027(v=ws.10)).

### <a name="to-configure-additional-domain-controllers"></a>Para configurar controladores de dominio adicionales

1.  En el servidor que actuará como controlador de dominio, en _ * Administrador del servidor * *, en el **Panel**, haga clic en **Agregar roles y características**.

2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor

3.  En la página **Seleccionar roles de servidor** , seleccione **Active Directory Domain Services**. Haga clic en **Agregar características** cuando se le pida y, a continuación, haga clic en **siguiente** tres veces.

4.  En la página **Confirmación**, haz clic en **Instalar**.

5.  Cuando la instalación se complete correctamente, haga clic en **promover este servidor a controlador de dominio**.

6.  En el Asistente para configuración de Active Directory Domain Services, en la página **configuración de implementación** , haga clic en **Agregar un controlador de dominio a un dominio existente**.

7.  En **dominio**, escriba el nombre de dominio; por ejemplo, corp.contoso.com.

8.  En **proporcione las credenciales para realizar esta operación**, haga clic en **cambiar**. En el cuadro de diálogo **seguridad de Windows** , proporcione el nombre de usuario y la contraseña de una cuenta que pueda instalar el controlador de dominio adicional. Para instalar un controlador de dominio adicional, debe ser miembro del grupo Administradores de empresas o del grupo Administradores de dominio. Cuando termine de proporcionar las credenciales, haga clic en **Siguiente**.

9. En la página **Opciones del controlador de dominio** , haga lo siguiente:

    1.  Realice las selecciones siguientes:

        -   **Servidor de sistema de nombres de dominio (DNS)**: esta opción está seleccionada de forma predeterminada para que el controlador de dominio pueda funcionar como un servidor de sistema de nombres de dominio (DNS). Si no desea que el controlador de dominio sea un servidor DNS, desactive esta opción.

            Si el rol del servidor DNS no está instalado en el emulador del controlador de dominio principal (PDC) en el dominio raíz del bosque, la opción para instalar el servidor DNS en un controlador de dominio adicional no está disponible. Como solución alternativa en esta situación, puede instalar el rol de servidor DNS antes o después de la instalación de AD DS.

            > [!NOTE]
            > Si selecciona la opción para instalar el servidor DNS, es posible que reciba un mensaje que indica que no se pudo crear una delegación DNS para el servidor DNS y que debe crear manualmente una delegación DNS en el servidor DNS para garantizar una resolución de nombres confiable. Si va a instalar un controlador de dominio adicional en el dominio raíz del bosque o en un dominio raíz del árbol, no es necesario que cree la delegación DNS. En este caso, haga clic en **sí** y omita el mensaje.

        -   **Catálogo global (GC)**"esta opción está seleccionada de forma predeterminada. Agregue el catálogo global y las particiones del directorio de solo lectura al controlador de dominio, y activa la funcionalidad de búsqueda del catálogo global.

        -   **Controlador de dominio de solo lectura (RODC)**"esta opción no está seleccionada de forma predeterminada. Hace que el controlador de dominio adicional sea de solo lectura; es decir, hace que el controlador de dominio sea un RODC.

    2.  En **nombre del sitio**, seleccione un sitio de la lista.

    3.  En **Escriba la contraseña de modo de restauración de servicios de directorio (DSRM)**, en **contraseña** y **Confirmar contraseña**, escriba una contraseña segura dos veces y, a continuación, haga clic en **siguiente**. Esta contraseña debe usarse para iniciar AD DS en DSRM para las tareas que deben realizarse sin conexión.

10. En la página **Opciones de DNS** , active la casilla **Actualizar delegación DNS** si desea actualizar la delegación DNS durante la instalación del rol y, a continuación, haga clic en **siguiente**.

11. En la página **opciones adicionales** , escriba o busque las ubicaciones de volumen y carpeta para el archivo de base de datos, los archivos de registro del servicio de directorio y los archivos de volumen del sistema (SYSVOL). Especifique las opciones de replicación según sea necesario y, a continuación, haga clic en **siguiente**.

12. En la página **revisar opciones** , revise las opciones de instalación y, a continuación, haga clic en **siguiente**.

13. En la página **comprobación de requisitos previos** , una vez validados los requisitos previos, haga clic en **instalar**.

14. Espere hasta que el asistente complete la configuración y, a continuación, haga clic en **cerrar**.

15. Reinicie el equipo si no se reinició automáticamente.

## <a name="23-configure-security-groups"></a><a name="BKMK_ConfigSG"></a>2.3. Configurar grupos de seguridad
Una implementación multisitio requiere un grupo de seguridad adicional para equipos cliente de Windows 7 para cada punto de entrada de la implementación que permita el acceso a los equipos cliente de Windows 7. Si hay varios dominios que contienen equipos cliente de Windows 7, se recomienda crear un grupo de seguridad en cada dominio para el mismo punto de entrada. Como alternativa, se puede usar un grupo de seguridad universal que contenga los equipos cliente de ambos dominios. Por ejemplo, en un entorno con dos dominios, si desea permitir el acceso a los equipos cliente de Windows 7 en los puntos de entrada 1 y 3, pero no en el punto de entrada 2, cree dos nuevos grupos de seguridad que contengan los equipos cliente de Windows 7 para cada punto de entrada de cada uno de los dominios.

### <a name="to-configure-additional-security-groups"></a>Para configurar grupos de seguridad adicionales

1.  En el controlador de dominio principal, haga clic en **Inicio** y, a continuación, en **Active Directory usuarios y equipos**.

2.  En el árbol de consola, haga clic con el botón secundario en la carpeta en la que desea agregar un nuevo grupo, por ejemplo, corp.contoso.com/Users. Elija **Nuevo** y haga clic en **Grupo**.

3.  En el cuadro de diálogo **nuevo objeto-grupo** , en **nombre de grupo**, escriba el nombre del nuevo grupo, por ejemplo, Win7_Clients_Entrypoint1.

4.  En **ámbito de grupo**, haga clic en **universal**, en **tipo de grupo**, haga clic en **seguridad** y, a continuación, haga clic en **Aceptar**.

5.  Para agregar equipos al nuevo grupo de seguridad, haga doble clic en el grupo de seguridad y, en el<cuadro de diálogo **propiedades de Group_Name>** , haga clic en la pestaña **miembros** .

6.  En la pestaña **Miembros**, haga clic en **Agregar**.

7.  Seleccione los equipos con Windows 7 que se van a agregar a este grupo de seguridad y, a continuación, haga clic en **Aceptar**.

8.  Repita este procedimiento para crear un grupo de seguridad para cada punto de entrada según sea necesario.

:::image type="icon" source="../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif":::**_<em>Comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Para instalar la característica de Windows "Active Directory módulo para Windows PowerShell":

```
Install-WindowsFeature "Name RSAT-AD-PowerShell
```

o bien, agregue el "complemento de PowerShell de Active Directory" a través de OptionalFeatures.

Si ejecuta los siguientes cmdlets en Windows 7 o Windows Server 2008 R2, se debe importar el módulo de PowerShell de Active Directory:

```
Import-Module ActiveDirectory
```

Para configurar un grupo de seguridad denominado Win7_Clients_Entrypoint1 y para agregar un equipo cliente denominado cliente2:

```
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$
```

## <a name="24-configure-gpos"></a><a name="ConfigGPOs"></a>2.4. Configurar GPO
Una implementación de acceso remoto multisitio requiere los siguientes objetos de directiva de grupo:

-   Un GPO para cada punto de entrada para el servidor de acceso remoto.

-   Un GPO para cualquier equipo cliente de Windows 8 para cada dominio.

-   Un GPO en cada dominio que contiene equipos cliente de Windows 7 para cada punto de entrada configurado para admitir clientes de Windows 7.

    > [!NOTE]
    > Si no tiene ningún equipo cliente de Windows 7, no es necesario crear GPO para equipos con Windows 7.

Al configurar el acceso remoto, el asistente crea automáticamente los objetos directiva de grupo necesarios si no existen. Si no tiene los permisos necesarios para crear directiva de grupo objetos, deben crearse antes de configurar el acceso remoto. El administrador de DirectAccess debe tener permisos completos en los GPO (editar + modificar seguridad + eliminar).

> [!IMPORTANT]
> Después de crear manualmente los GPO para el acceso remoto, debe permitir el tiempo suficiente para Active Directory y la replicación DFS en el controlador de dominio del sitio Active Directory que está asociado con el servidor de acceso remoto. Si el acceso remoto crea automáticamente los objetos directiva de grupo, no es necesario ningún tiempo de espera.

Para crear directiva de grupo objetos, vea [crear y editar un objeto Directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).

### <a name="domain-controller-maintenance-and-downtime"></a><a name="DCMaintandDowntime"></a>Mantenimiento y tiempo de inactividad del controlador de dominio
Cuando un controlador de dominio que se ejecuta como el emulador de PDC o controladores de dominio que administran los GPO de servidor experimentan tiempo de inactividad, no es posible cargar o modificar la configuración de acceso remoto. Esto no afecta a la conectividad de cliente si hay otros controladores de dominio disponibles.

Para cargar o modificar la configuración de acceso remoto, puede transferir el rol de emulador de PDC a un controlador de dominio diferente para los GPO de cliente o de servidor de aplicaciones. en el caso de los GPO de servidor, cambie los controladores de dominio que administran los GPO de servidor.

> [!IMPORTANT]
> Esta operación solo la puede realizar un administrador de dominio. El impacto de cambiar el controlador de dominio principal no se limita al acceso remoto; por lo tanto, tenga cuidado al transferir el rol de emulador de PDC.

> [!NOTE]
> Antes de modificar la Asociación del controlador de dominio, asegúrese de que todos los GPO de la implementación de acceso remoto se han replicado en todos los controladores de dominio del dominio. Si el GPO no está sincronizado, los cambios de configuración recientes pueden perderse después de modificar la Asociación del controlador de dominio, lo que puede dar lugar a una configuración dañada. Para comprobar la sincronización de GPO, consulte comprobar el estado de la [infraestructura de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134176(v=ws.11)).

#### <a name="to-transfer-the-pdc-emulator-role"></a><a name="TransferPDC"></a>Para transferir el rol de emulador de PDC

1.  En la pantalla _ *iniciar**, escriba **DSA. msc** y, a continuación, presione Entrar.

2.  En el panel izquierdo de la consola de Active Directory usuarios y equipos, haga clic con el botón secundario en **Active Directory usuarios y equipos** y, a continuación, haga clic en **cambiar controlador de dominio**. En el cuadro de diálogo cambiar servidor de directorio, haga clic en **este controlador de dominio o en AD LDS instancia**, en la lista, haga clic en el controlador de dominio que será el nuevo titular de la función y, a continuación, haga clic en **Aceptar**.

    > [!NOTE]
    > Debe realizar este paso si no está en el controlador de dominio al que desea transferir el rol. No lleve a cabo este paso si ya está conectado al controlador de dominio al que desea transferir el rol.

3.  En el árbol de consola, haga clic con el botón secundario en **Active Directory usuarios y equipos**, seleccione **todas las tareas** y, a continuación, haga clic en **maestros de operaciones**.

4.  En el cuadro de diálogo maestros de operaciones, haga clic en la pestaña **PDC** y, a continuación, haga clic en **cambiar**.

5.  Haga clic en **sí** para confirmar que desea transferir el rol y, a continuación, haga clic en **cerrar**.

#### <a name="to-change-the-domain-controller-that-manages-server-gpos"></a><a name="ChangeDC"></a>Para cambiar el controlador de dominio que administra los GPO de servidor

-   Ejecute el cmdlet de Windows PowerShell  [set-DAEntryPointDC](/powershell/module/remoteaccess/set-daentrypointdc) en el servidor de acceso remoto y especifique el nombre del controlador de dominio inaccesible para el parámetro *ExistingDC* . Este comando modifica la Asociación del controlador de dominio para los GPO de servidor de los puntos de entrada administrados actualmente por ese controlador de dominio.

    -   Para reemplazar el controlador de dominio inaccesible "dc1.corp.contoso.com" con el controlador de dominio "dc2.corp.contoso.com", haga lo siguiente:

        ```
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire
        ```

    -   Para reemplazar el controlador de dominio no accesible "dc1.corp.contoso.com" por un controlador de dominio del sitio Active Directory más cercano al servidor de acceso remoto "DA1.corp.contoso.com", haga lo siguiente:

        ```
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
        ```

### <a name="change-two-or-more-domain-controllers-that-manage-server-gpos"></a><a name="ChangeTwoDCs"></a>Cambiar dos o más controladores de dominio que administran GPO de servidor
En un número mínimo de casos, dos o más controladores de dominio que administran GPO de servidor no están disponibles. Si esto ocurre, se necesitan más pasos para cambiar la Asociación del controlador de dominio para los GPO de servidor.

La información de asociación del controlador de dominio se almacena en el registro de los servidores de acceso remoto y en todos los GPO de servidor. En el ejemplo siguiente, hay dos puntos de entrada con dos servidores de acceso remoto, "DA1" en "Entry Point 1" y "DA2" en "Entry Point 2". El GPO de servidor de "punto de entrada 1" se administra en el controlador de dominio "DC1", mientras que el GPO de servidor de "punto de entrada 2" se administra en el controlador de dominio "DC2". "DC1" y "DC2" no están disponibles. Un tercer controlador de dominio sigue estando disponible en el dominio, "DC3", y los datos de "DC1" y "DC2" ya se han replicado en "DC3".

![Configuración de la infraestructura multisitio](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)

##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Para cambiar dos o más controladores de dominio que administran GPO de servidor

1.  Para reemplazar el controlador de dominio no disponible "DC2" por el controlador de dominio "DC3", ejecute el siguiente comando:

    ```
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue
    ```

    Este comando actualiza la Asociación del controlador de dominio para el GPO de servidor "Entry Point 2" en el registro de DA2 y en el propio GPO de servidor "Entry Point 2". sin embargo, no actualiza el GPO de servidor "punto de entrada 1" porque el controlador de dominio que lo administra no está disponible.

    > [!TIP]
    > Este comando usa el valor continue para el parámetro *ErrorAction* , que actualiza el GPO de servidor "Entry Point 2" a pesar de que no se pudo actualizar el GPO de servidor "Entry Point 1".

    La configuración resultante se muestra en el diagrama siguiente.

    ![Diagrama que muestra la configuración resultante.](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)

2.  Para reemplazar el controlador de dominio no disponible "DC1" por el controlador de dominio "DC3", ejecute el siguiente comando:

    ```
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue
    ```

    Este comando actualiza la Asociación del controlador de dominio para el GPO de servidor "punto de entrada 1" en el registro de DA1 y en los GPO de servidor "punto de entrada 1" y "punto de entrada 2". La configuración resultante se muestra en el diagrama siguiente.

    ![Diagrama que muestra la Asociación de controladores de dominio de actualización.](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)

3.  Para sincronizar la Asociación del controlador de dominio para el GPO de servidor "punto de entrada 2" en el GPO de servidor "punto de entrada 1", ejecute el comando para reemplazar "DC2" por "DC3" y especifique el servidor de acceso remoto cuyo GPO de servidor no esté sincronizado, en este caso "DA1", para el parámetro *ComputerName* .

    ```
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue
    ```

    La configuración final se muestra en el diagrama siguiente.

    ![Diagrama que muestra la configuración final.](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)

### <a name="optimization-of-configuration-distribution"></a><a name="ConfigDistOptimization"></a>Optimización de la distribución de la configuración
Al realizar cambios de configuración, los cambios se aplican solo después de que los GPO de servidor se propaguen a los servidores de acceso remoto. Para reducir el tiempo de distribución de la configuración, el acceso remoto selecciona automáticamente un controlador de dominio de escritura [más cercano al servidor de acceso remoto](/previous-versions/windows/it-pro/windows-2000-server/cc978016(v=technet.10)) al crear su GPO de servidor.

En algunos escenarios, puede ser necesario modificar manualmente el controlador de dominio que administra un GPO de servidor para optimizar el tiempo de distribución de la configuración:

-   No había ningún controlador de dominio de escritura en el sitio Active Directory de un servidor de acceso remoto en el momento de agregarlo como punto de entrada. Ahora se está agregando un controlador de dominio de escritura al sitio Active Directory del servidor de acceso remoto.

-   Un cambio de dirección IP, o un cambio de sitios Active Directory y subredes puede haber pasado el servidor de acceso remoto a un sitio de Active Directory diferente.

-   La Asociación del controlador de dominio para un punto de entrada se modificó manualmente debido al trabajo de mantenimiento en un controlador de dominio y ahora el controlador de dominio vuelve a estar en línea.

En estos casos, ejecute el cmdlet de PowerShell `Set-DAEntryPointDC` en el servidor de acceso remoto y especifique el nombre del punto de entrada que desea optimizar mediante el parámetro *EntryPointName*. Debe hacer esto solo después de que los datos del GPO del controlador de dominio que almacena actualmente el GPO de servidor ya se hayan replicado por completo en el nuevo controlador de dominio deseado.

> [!NOTE]
> Antes de modificar la Asociación del controlador de dominio, asegúrese de que todos los GPO de la implementación de acceso remoto se han replicado en todos los controladores de dominio del dominio. Si el GPO no está sincronizado, los cambios de configuración recientes pueden perderse después de modificar la Asociación del controlador de dominio, lo que puede dar lugar a una configuración dañada. Para comprobar la sincronización de GPO, consulte comprobar el estado de la [infraestructura de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134176(v=ws.11)).

Para optimizar el tiempo de distribución de la configuración, realice una de las acciones siguientes:

-   Para administrar el GPO de servidor del punto de entrada "punto de entrada 1" en un controlador de dominio del sitio Active Directory más cercano al servidor de acceso remoto "DA1.corp.contoso.com", ejecute el siguiente comando:

    ```
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
    ```

-   Para administrar el GPO de servidor del punto de entrada "punto de entrada 1" en el controlador de dominio "dc2.corp.contoso.com", ejecute el siguiente comando:

    ```
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
    ```

    > [!NOTE]
    > Al modificar el controlador de dominio asociado a un punto de entrada específico, debe especificar un servidor de acceso remoto que sea miembro de ese punto de entrada para el parámetro *ComputerName* .

## <a name="see-also"></a><a name="BKMK_Links"></a>Otras referencias

-   [Paso 3: configurar la implementación multisitio](Step-3-Configure-the-Multisite-Deployment.md)
-   [Paso 1: implementar una implementación de acceso remoto de un solo servidor](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)
