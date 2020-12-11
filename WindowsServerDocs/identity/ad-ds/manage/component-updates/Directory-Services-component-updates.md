---
description: 'Más información acerca de: actualizaciones de componentes de servicios de directorio'
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Actualizaciones de componentes de Servicios de directorio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a5836f3dda9615a449c89fe130798566e7481906
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049663"
---
# <a name="directory-services-component-updates"></a>Actualizaciones de componentes de Servicios de directorio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows

> [!NOTE]
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.

En esta lección se explican las actualizaciones de componentes de servicios de directorio en Windows Server 2012 R2.

## <a name="what-you-will-learn"></a>Aprendizaje
Explique las siguientes nuevas actualizaciones de componentes de servicios de directorio:

-   Explique las siguientes nuevas actualizaciones de componentes de servicios de directorio:

    -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)

    -   [Degradación de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)

    -   [Cambios en el optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)

    -   [Mejoras en el evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)

    -   [Mejora del rendimiento de replicación de Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)

## <a name="domain-and-forest-functional-levels"></a><a name="BKMK_FL"></a>Niveles funcionales de dominios y bosques

### <a name="overview"></a>Información general
En esta sección se proporciona una breve introducción a los cambios de nivel funcional del bosque y del dominio.

### <a name="new-dfl-and-ffl"></a>New nivel funcional y FFL
Con la versión, hay nuevos niveles funcionales de dominio y bosque:

-   Nivel funcional del bosque: Windows Server 2012 R2

-   Nivel funcional del dominio: Windows Server 2012 R2

### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>El nivel funcional de dominio de Windows Server 2012 R2 habilita la compatibilidad para lo siguiente:

1.  Protecciones del controlador de dominio para *usuarios protegidos*

    *Los usuarios protegidos* que se autentiquen en un dominio de Windows Server 2012 R2 ya **no** podrán:

    -   Autenticarse mediante la autenticación NTLM

    -   Usar los conjuntos de cifrado DES o RC4 en la autenticación previa de Kerberos

    -   Delegarse mediante una delegación con o sin restricciones

    -   Renovar los vales de usuario (TGT) más allá de las 4 horas de vigencia inicial

2.  Authentication Policies

    Nuevas directivas de Active Directory basadas en bosques que se pueden aplicar a las cuentas de dominios de Windows Server 2012 R2 para controlar a qué hosts puede iniciar sesión una cuenta y aplicar condiciones de control de acceso para la autenticación en los servicios que se ejecutan como una cuenta

3.  Authentication Policy Silos

    Nuevo objeto de Active Directory basado en bosques que puede crear una relación entre el usuario, el servicio administrado y las cuentas de equipo que se usarán para clasificar cuentas para las directivas de autenticación o para el aislamiento de autenticación.

Consulte How to Configure Protected accounts para obtener más información.

Además de las características anteriores, el nivel funcional del dominio de Windows Server 2012 R2 garantiza que cualquier controlador de dominio del dominio ejecute Windows Server 2012 R2.
El nivel funcional del bosque de Windows Server 2012 R2 no proporciona características nuevas, pero garantiza que cualquier dominio nuevo creado en el bosque funcionará automáticamente en el nivel funcional del dominio de Windows Server 2012 R2.

### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>NIVEL funcional mínimo aplicado en la creación de un nuevo dominio
Windows Server 2008 nivel funcional es el nivel funcional mínimo admitido en la creación de nuevos dominios.

> [!NOTE]
> El desuso de FRS se consigue quitando la capacidad de instalar un nuevo dominio con un nivel funcional de dominio inferior a Windows Server 2008 con Administrador del servidor o a través de Windows PowerShell.

### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reducción de los niveles funcionales de dominios y bosques
Los niveles funcionales del bosque y del dominio se establecen en Windows Server 2012 R2 de forma predeterminada en el nuevo dominio y en la creación de un nuevo bosque, pero se pueden reducir con Windows PowerShell.

Para aumentar o reducir el nivel funcional del bosque mediante Windows PowerShell, use el cmdlet **set-ADForestMode** .

**Para establecer el valor de contoso.com FFL en el modo de Windows Server 2008:**

```sql
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com
```

Para aumentar o reducir el nivel funcional del dominio mediante Windows PowerShell, use el cmdlet Set-ADDomainMode.

**Para establecer el modo de contoso.com nivel funcional en Windows Server 2008:**

```powershell
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com
```

La promoción de un controlador de dominio que ejecuta Windows Server 2012 R2 como réplica adicional en un dominio existente que ejecuta 2003 nivel funcional funciona.

Creación de un nuevo dominio en un bosque existente

![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)

### <a name="adprep"></a>ADPREP
No hay nuevas operaciones de bosque o dominio en esta versión.

Estos archivos. ldf contienen cambios de esquema para el **servicio de registro de dispositivos**.

1.  Sch59

2.  Sch61

3.  Sch62

4.  Sch63

5.  Sch64

6.  Sch65

7.  Sch67

**Carpetas de trabajo:**

1.  Sch66

**MSODS**

1.  Sch60

**Silos y directivas de autenticación**

1.  Sch68

2.  Sch69

## <a name="deprecation-of-ntfrs"></a><a name="BKMK_NTFRS"></a>Degradación de NTFRS

### <a name="overview"></a>Información general
FRS está en desuso en Windows Server 2012 R2.  La degradación de FRS se logra aplicando un nivel funcional de dominio mínimo (nivel funcional) de Windows Server 2008.  Esta aplicación solo está presente si el nuevo dominio se crea con Administrador del servidor o Windows PowerShell.

Use el parámetro-DomainMode con los cmdlets Install-ADDSForest o Install-ADDSDomain para especificar el nivel funcional del dominio.  Los valores admitidos para este parámetro pueden ser un entero válido o un valor de cadena enumerado correspondiente. Por ejemplo, para establecer el nivel de modo de dominio en Windows Server 2008 R2, puede especificar un valor de 4 o "Win2008R2".  Al ejecutar estos cmdlets desde el servidor 2012 R2, los valores válidos son los de Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) Windows Server 2012 (5, Win2012) y Windows Server 2012 R2 (6, Win2012R2). El nivel funcional del dominio no puede ser inferior que el del bosque, pero puede ser superior.  Dado que FRS está en desuso en esta versión, Windows Server 2003 (2, Win2003) no es un parámetro reconocido con estos cmdlets cuando se ejecuta desde Windows Server 2012 R2.

![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)

![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)

## <a name="ldap-query-optimizer-changes"></a><a name="BKMK_LDAPQuery"></a>Cambios del optimizador de consultas LDAP

### <a name="overview"></a>Información general
El algoritmo del optimizador de consultas LDAP se ha reevaluado y optimizado aún más.  El resultado es la mejora del rendimiento en la eficiencia de búsqueda LDAP y el tiempo de búsqueda LDAP de consultas complejas.

> [!NOTE]
> <strong>Del Desarrollador:</strong>mejoras en el rendimiento de las búsquedas a través de las mejoras en la asignación de consulta LDAP a consulta de ese.  Los filtros LDAP que superen cierto nivel de complejidad impiden la selección de índices optimizados, lo que da lugar a un rendimiento reducido drásticamente (1000 veces o más). Este cambio altera la manera en que se seleccionan índices para las consultas LDAP para evitar este problema.
>
> [!NOTE]
> Una revisión completa del algoritmo del optimizador de consultas LDAP, lo que da como resultado:
>
> -   Tiempos de búsqueda más rápidos
> -   Las mejoras de eficiencia permiten a los controladores de DC hacer más
> -   Menos llamadas de soporte en relación con los problemas de rendimiento de AD
> -   Copia de seguridad en Windows Server 2008 R2 (KB 2862304)

### <a name="background"></a>Información previa
La capacidad de buscar Active Directory es un servicio principal proporcionado por los controladores de dominio.  Otros servicios y aplicaciones de línea de negocio se basan en las búsquedas de Active Directory.  Las operaciones empresariales pueden dejar de funcionar si esta característica no está disponible.  Como servicio principal y de uso intensivo, es imperativo que los controladores de dominio controlen el tráfico de búsqueda LDAP de manera eficaz.  El algoritmo del optimizador de consultas LDAP intenta hacer que las búsquedas LDAP sean eficientes como sea posible asignando filtros de búsqueda LDAP a un conjunto de resultados que se puede satisfacer mediante registros ya indexados en la base de datos.  Este algoritmo se volvió a evaluar y optimizar aún más.  El resultado es la mejora del rendimiento en la eficiencia de búsqueda LDAP y el tiempo de búsqueda LDAP de consultas complejas.

### <a name="details-of-change"></a>Detalles de cambio
Una búsqueda LDAP contiene:

-   Una ubicación (encabezado de NC, unidad organizativa, objeto) dentro de la jerarquía para comenzar la búsqueda

-   Un filtro de búsqueda

-   Lista de atributos que se van a devolver

El proceso de búsqueda se puede resumir de la siguiente manera:

1.  Simplifique el filtro de búsqueda si es posible.

2.  Seleccione un conjunto de claves de índice que devolverá el conjunto más pequeño incluido.

3.  Realice una o varias intersecciones de las claves de índice para reducir el conjunto descrito.

4.  Para cada registro del conjunto descrito, evalúe la expresión de filtro, así como la seguridad. Si el filtro se evalúa como TRUE y se concede el acceso, devuelva este registro al cliente.

El trabajo de optimización de consultas LDAP modifica los pasos 2 y 3 para reducir el tamaño del conjunto descrito. Más concretamente, la implementación actual selecciona claves de índice duplicadas y realiza intersecciones redundantes.

### <a name="comparison-between-old-and-new-algorithm"></a>Comparación entre el algoritmo antiguo y el nuevo
El destino de la búsqueda LDAP ineficaz en este ejemplo es un controlador de dominio de Windows Server 2012.  La búsqueda se completa en aproximadamente 44 segundos como resultado de no encontrar un índice más eficaz.

```
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt

Using server: WINSRV-DC1.blue.contoso.com:389

<removed search results>

Statistics
=====
Elapsed Time: 44640 (ms)
Returned 324 entries of 553896 visited - (0.06%)

Used Filter:
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )

Used Indices:
 DNT_index:516615:N

Pages Referenced          : 4619650
Pages Read From Disk      : 973
Pages Pre-read From Disk  : 180898
Pages Dirtied             : 0
Pages Re-Dirtied          : 0
Log Records Generated     : 0
Log Record Bytes Generated: 0
```

### <a name="sample-results-using-the-new-algorithm"></a>Resultados de ejemplo con el nuevo algoritmo
En este ejemplo se repite exactamente la misma búsqueda que la anterior pero tiene como destino un controlador de dominio de Windows Server 2012 R2.  La misma búsqueda se completa en menos de un segundo debido a las mejoras en el algoritmo del optimizador de consultas LDAP.

```
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt

Using server: winblueDC1.blue.contoso.com:389

.<removed search results>

Statistics
=====
Elapsed Time: 672 (ms)
Returned 324 entries of 648 visited - (50.00%)

Used Filter:
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )

Used Indices:
 idx_userPrincipalName:648:N
 idx_postalCode:323:N
 idx_cn:1:N

Pages Referenced          : 15350
Pages Read From Disk      : 176
Pages Pre-read From Disk  : 2
Pages Dirtied             : 0
Pages Re-Dirtied          : 0
Log Records Generated     : 0
Log Record Bytes Generated: 0
```

-   Si no puede optimizar el árbol:

    -   Por ejemplo: una expresión en el árbol estaba sobre una columna no indizada

    -   Grabe una lista de índices que impidan la optimización

    -   Expuesto a través del seguimiento de ETW y el ID. de evento 1644

        ![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)

### <a name="to-enable-the-stats-control-in-ldp"></a><a name="BKMK_EnableStats"></a>Para habilitar el control de estadísticas en LDP

1.  Abra LDP.exe y conéctese y enlace con un controlador de dominio.

2.  En el menú **Opciones** , haga clic en **controles**.

3.  En el cuadro de diálogo controles, expanda el menú desplegable **cargar predefinido** , haga clic en **estadísticas de búsqueda** y, a continuación, haga clic en **Aceptar**.

    ![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)

4.  En el menú **examinar** , haga clic en **Buscar** .

5.  En el cuadro de diálogo Buscar, seleccione el botón **Opciones** .

6.  Asegúrese de que la casilla **extendido** está activada en el cuadro de diálogo Opciones de búsqueda y seleccione **Aceptar**.

    ![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)

### <a name="try-this-use-ldp-to-return-query-statistics"></a>Pruebe esto: usar LDP para devolver estadísticas de consulta
Realice lo siguiente en un controlador de dominio o en un cliente o servidor unido a un dominio que tenga instaladas las herramientas de AD DS.  Repita el siguiente destino con el controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2.

1.  Revise el artículo [sobre la creación de aplicaciones habilitadas para Microsoft ad más eficaces](/previous-versions/ms808539(v=msdn.10)) y vuelva a hacer referencia a ella según sea necesario.

2.  Usar LDP, habilitar las estadísticas de búsqueda (vea [para habilitar el control de estadísticas en LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))

3.  Realice varias búsquedas LDAP y observe la información estadística en la parte superior de los resultados.  Repetirá la misma búsqueda en otras actividades, por lo que deberá documentarlas en un archivo de texto del Bloc de notas.

4.  Realizar una búsqueda LDAP que el optimizador de consultas debe poder optimizar debido a los índices de atributos

5.  Intente crear una búsqueda que tarde mucho tiempo en completarse (es posible que desee aumentar la opción de **límite de tiempo** para que la búsqueda no agote el tiempo de espera).

### <a name="additional-resources"></a>Recursos adicionales
[¿Qué son las búsquedas Active Directory?](/previous-versions/windows/it-pro/windows-server-2003/cc783845(v=ws.10))

[Cómo funcionan las búsquedas de Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc755809(v=ws.10))

[Creación de aplicaciones de Microsoft Active Directory-Enabled más eficaces](/previous-versions/ms808539(v=msdn.10))

[951581](https://support.microsoft.com/kb/951581) las consultas LDAP se ejecutan más lentamente de lo esperado en el servicio de directorio ad o LDS/Adam y el ID. de evento 1644 se puede registrar

## <a name="1644-event-improvements"></a><a name="BKMK_1644"></a>Mejoras en el evento 1644

### <a name="overview"></a>Información general
Esta actualización agrega estadísticas de resultados de búsqueda LDAP adicionales al ID. de evento 1644 para ayudar a solucionar problemas.  Además, hay un nuevo valor del registro que se puede usar para habilitar el registro en un umbral basado en el tiempo.  Estas mejoras están disponibles en Windows Server 2012 y Windows Server 2008 R2 SP1 a través de KB [2800945](https://support.microsoft.com/kb/2800945) y estarán disponibles para windows Server 2008 SP2.

> [!NOTE]
> -   Se han agregado estadísticas de búsqueda LDAP adicionales al ID. de evento 1644 para ayudar a solucionar problemas de búsquedas LDAP ineficaces o costosas.
> -   Ahora puede especificar un umbral de tiempo de búsqueda (por ejemplo, Registra el evento 1644 en las búsquedas que tardan más de 100 ms) en lugar de especificar los valores de umbral de resultados de búsqueda caros e ineficaces

### <a name="background"></a>Información previa
Al solucionar problemas de rendimiento de Active Directory, se hace evidente que la actividad de búsqueda LDAP puede contribuir al problema.  Decide habilitar el registro para que pueda ver consultas LDAP costosas o ineficaces procesadas por el controlador de dominio.  Para habilitar el registro, debe establecer el valor de diagnóstico de ingeniería de campo y, opcionalmente, puede especificar los valores de umbral de resultados de búsqueda caros e ineficaces.  Al habilitar el nivel de registro de ingeniería de campo en un valor de 5, cualquier búsqueda que cumpla estos criterios se registra en el registro de eventos de servicios de directorio con un ID. de evento 1644.

El evento contiene:

-   Puerto y dirección IP del cliente

-   Nodo inicial

-   Filtrar

-   Ámbito de búsqueda

-   Selección de atributos

-   Controles de servidor

-   Entradas visitadas

-   Entradas devueltas

Sin embargo, los datos de clave no se encuentran en el evento, como la cantidad de tiempo empleado en la operación de búsqueda y el índice (si existe) que se ha usado.

#### <a name="additional-search-statistics-added-to-event-1644"></a>Estadísticas de búsqueda adicionales agregadas al evento 1644

-   Índices usados

-   Páginas a las que se hace referencia

-   Páginas leídas del disco

-   Páginas preleídas del disco

-   Limpiar páginas modificadas

-   Páginas desfasadas modificadas

-   Tiempo de búsqueda

-   Atributos que evitan la optimización

#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuevo valor de registro de umbral basado en tiempo para el registro del evento 1644
En lugar de especificar los valores de umbral de resultados de búsqueda caros e ineficaces, puede especificar el umbral de tiempo de búsqueda.  Si desea registrar todos los resultados de búsqueda que tardaron 50 ms o superior, debe especificar 50 decimal/32 Hex (además de establecer el valor de ingeniería de campo).

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]
"Search Time Threshold (msecs)"=dword:00000032
```

#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparación del ID. de evento anterior y nuevo 1644
OLD

![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)

NEW

![actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)

#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Pruebe esto: usar el registro de eventos para devolver estadísticas de consulta

1.  Repita el siguiente destino con el controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2. Observe el ID. de evento 1644s en ambos controladores de eventos después de cada búsqueda.

2.  Con regedit, habilite el registro de ID. de evento 1644 con un umbral basado en el tiempo en el DC de Windows Server 2012 R2 y el método anterior en el controlador de dominio de Windows Server 2012.

3.  Realice varias búsquedas LDAP que superen el umbral y observe la información estadística en la parte superior de los resultados.  Use las consultas LDAP que ha documentado anteriormente y repita las mismas búsquedas.

4.  Realice una búsqueda LDAP que el optimizador de consultas no pueda optimizar porque uno o varios atributos no están indizados.

## <a name="active-directory-replication-throughput-improvement"></a><a name="BKMK_ADRepl"></a>Mejora del rendimiento de la replicación de Active Directory

### <a name="overview"></a>Información general
La replicación de AD usa RPC para el transporte de replicación. De forma predeterminada, RPC usa un búfer de transmisión de 8K y un tamaño de paquete de 5 k. Esto tiene el efecto neto, en el que la instancia de envío transmitirá tres paquetes (aproximadamente 15 k de datos) y tendrá que esperar un viaje de ida y vuelta de red antes de enviar más. Suponiendo un tiempo de ida y vuelta de 3ms, el mayor rendimiento sería en torno a 40 Mbps, incluso en redes 1 Gbps o 10 Gbps.

> [!NOTE]
> -   Esta actualización ajusta el rendimiento máximo de la replicación de AD de 40 Mbps a aproximadamente 600 Mbps.
>
>     -   Aumenta el tamaño del búfer de envío de RPC, lo que reduce el número de recorridos de ida y vuelta de red.
> -   El efecto será más perceptible en la red de alta velocidad y latencia alta.

Estas actualizaciones aumentan el rendimiento máximo de alrededor de 600 Mbps al cambiar el tamaño del búfer de envío de RPC de 8 KB a 256 KB.  Este cambio permite que el tamaño de la ventana TCP crezca más allá de 8 KB, lo que reduce el número de recorridos de ida y vuelta de red.

> [!NOTE]
> No hay ninguna configuración configurable para modificar este comportamiento.

### <a name="additional-resources"></a>Recursos adicionales
[Cómo funciona el modelo de replicación de Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc772726(v=ws.10))

