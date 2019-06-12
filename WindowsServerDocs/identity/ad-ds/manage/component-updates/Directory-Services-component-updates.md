---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Actualizaciones de componentes de Servicios de directorio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fe27b61abe196a2148ced18806be904ebd555fcc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442893"
---
# <a name="directory-services-component-updates"></a>Actualizaciones de componentes de Servicios de directorio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
Esta lección explica las actualizaciones de componentes de servicios de directorio en Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Qué aprenderá  
Explica las nuevas actualizaciones de componentes de servicios de directorio siguientes:  
  
-   Explica las nuevas actualizaciones de componentes de servicios de directorio siguientes:  
  
    -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Degradación de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Cambios en el optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Mejoras en el evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Mejora del rendimiento de replicación de Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Niveles funcionales de dominios y bosques  
  
### <a name="overview"></a>Información general  
La sección proporciona una breve introducción a los cambios de nivel funcionales de dominio y bosque.  
  
### <a name="new-dfl-and-ffl"></a>Nuevo nivel funcional del dominio y FFL  
Con la versión, hay nuevos niveles funcionales de bosque y dominio:  
  
-   Nivel funcional de bosque: Windows Server 2012 R2  
  
-   Nivel funcional del dominio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 nivel funcional del dominio habilita la compatibilidad para lo siguiente:  
  
1.  Protecciones de lado del controlador de dominio para *usuarios protegidos*  
  
    *Los usuarios protegidos* puede autenticar a un dominio de Windows Server 2012 R2 **ya no**:  
  
    -   Autenticar con la autenticación NTLM  
  
    -   Usar conjuntos de cifrado DES o RC4 en la autenticación Kerberos previa  
  
    -   Se puede delegar con delegación limitada o no  
  
    -   Renovación de vales de usuario (TGT) más allá de la duración inicial de 4 horas  
  
2.  Authentication Policies  
  
    Directivas de Active Directory basado en un bosque nuevo que se pueden aplicar a cuentas en dominios de Windows Server 2012 R2 para controlar qué hosts una cuenta puede inicio de sesión de y se aplican las condiciones de control de acceso para la autenticación a servicios que se ejecutan como una cuenta  
  
3.  Authentication Policy Silos  
  
    Objeto de Active Directory basado en un bosque nuevo que puede crear una relación entre el usuario, servicios administrados y las cuentas de equipo que se utilizará para clasificar las cuentas para las directivas de autenticación o para el aislamiento de la autenticación.  
  
Vea cómo configurar cuentas protegidas para obtener más información.  
  
Además de las características anteriores, el nivel funcional del dominio de Windows Server 2012 R2 garantiza que cualquier controlador de dominio en el dominio ejecuta Windows Server 2012 R2.  
El nivel funcional del bosque de Windows Server 2012 R2 no proporciona características nuevas, pero asegura que todo dominio nuevo creado en el bosque funcione automáticamente en el nivel funcional del dominio de Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Nivel funcional mínimo aplicado en la creación del nuevo dominio  
Nivel funcional de dominio de Windows Server 2008 es el nivel funcional mínimo compatible con la creación del nuevo dominio.  
  
> [!NOTE]  
> El desuso de FRS se consigue mediante la eliminación de la capacidad para instalar un nuevo dominio con un nivel funcional del dominio inferior a Windows Server 2008 con el administrador del servidor o a través de Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reducir los niveles funcionales de bosque y dominio  
Los niveles funcionales de bosque y dominio se establecen en Windows Server 2012 R2 de forma predeterminada en la nueva creación de dominio y bosque nuevo, pero pueden reducirse mediante Windows PowerShell.  
  
Para subir o bajar el nivel funcional del bosque mediante Windows PowerShell, use el **Set-ADForestMode** cmdlet.  
  
**Para establecer el "contoso.com" FFL en modo Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Para subir o bajar el nivel funcional del dominio mediante Windows PowerShell, use el cmdlet Set-ADDomainMode.  
  
**Para establecer el nivel funcional del dominio contoso.com en modo Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
La promoción de un controlador de dominio que ejecuta Windows Server 2012 R2 como una réplica adicional en un dominio existente que ejecuta 2003 DFL funciona.  
  
Creación del nuevo dominio en un bosque existente  
  
![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
No hay ningún nuevo bosque o las operaciones de dominio en esta versión.  
  
Estos archivos .ldf contienen cambios de esquema para el **Device Registration Service**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Carpetas de trabajo:**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**Silos y directivas de autenticación**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Degradación de NTFRS  
  
### <a name="overview"></a>Información general  
FRS está en desuso en Windows Server 2012 R2.  El desuso de FRS se logra aplicando un nivel funcional del dominio mínimo (DFL) de Windows Server 2008.  Esta aplicación está presente sólo si el nuevo dominio se crea mediante el administrador del servidor o Windows PowerShell.  
  
Utilice el parámetro - DomainMode con el Install-ADDSForest o los cmdlets Install-ADDSDomain para especificar el nivel funcional del dominio.  Los valores admitidos para este parámetro pueden ser un entero válido o un valor correspondiente de la cadena enumerada. Por ejemplo, para establecer el nivel del modo de dominio a Windows Server 2008 R2, puede especificar un valor de 4 o "Win2008R2".  Al ejecutar estos cmdlets de Server 2012 R2 válidos los valores incluyen para Windows Server 2008 (3, Win2008) (4, Win2008R2) de Windows Server 2008 R2 (5, Win2012) de Windows Server 2012 y Windows Server 2012 R2 (6, Win2012R2). El nivel funcional del dominio no puede ser inferior que el del bosque, pero puede ser superior.  Puesto que FRS está en desuso en esta versión, Windows Server 2003 (2, Win2003) no es un parámetro reconocido con estos cmdlets cuando se ejecuta desde Windows Server 2012 R2.  
  
![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Cambios del optimizador de consultas LDAP  
  
### <a name="overview"></a>Información general  
El algoritmo de optimizador de consultas LDAP se vuelven a evaluar y se optimiza aún más.  El resultado es la mejora del rendimiento en tiempo de búsqueda LDAP de las consultas complejas y la eficacia de la búsqueda LDAP.  
  
> [!NOTE]
> <strong>Parte del desarrollador:</strong>consultar las mejoras de rendimiento de las búsquedas a través de mejoras en la asignación de LDAP para consultar ESE.  Filtros LDAP más allá de un cierto nivel de complejidad impedir la selección de índice optimizado, lo que disminuye considerablemente el rendimiento (x 1000 o más). Este cambio modifica la manera en que se seleccionan índices para las consultas LDAP evitar este problema.  
> 
> [!NOTE]
> Una revisión completa del algoritmo de optimizador de consultas LDAP, lo que da lugar:  
> 
> -   Tiempos de búsqueda más rápidos  
> -   Mayor eficacia permite hacer más de los controladores de dominio  
> -   Menos llamadas de soporte técnico se emite en cuanto a rendimiento de AD  
> -   Realizar una copia de portada a Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Background  
La capacidad de buscar en Active Directory es un servicio central proporcionado por los controladores de dominio.  Otros servicios y aplicaciones empresariales se basan en las búsquedas de Active Directory.  Pueden dejar las operaciones empresariales detenerse si esta característica no está disponible.  Como un núcleo y el servicio muy utilizado, es imperativo que los controladores de dominio administran eficazmente el tráfico de búsqueda LDAP.  El algoritmo de optimizador de consultas LDAP intenta realizar búsquedas LDAP eficaces como sea posible mediante la asignación de filtros de búsqueda LDAP a un conjunto de resultados que se puede satisfacer mediante registros ya está indizados en la base de datos.  Este algoritmo se vuelven a evaluar y se optimiza aún más.  El resultado es la mejora del rendimiento en tiempo de búsqueda LDAP de las consultas complejas y la eficacia de la búsqueda LDAP.  
  
### <a name="details-of-change"></a>Detalles del cambio  
Contiene una búsqueda LDAP:  
  
-   Una ubicación (encabezado NC, unidad organizativa, objeto) dentro de la jerarquía para comenzar la búsqueda  
  
-   Un filtro de búsqueda  
  
-   Una lista de atributos que se va a devolver  
  
El proceso de búsqueda se puede resumir como sigue:  
  
1.  Simplifique el filtro de búsqueda si es posible.  
  
2.  Seleccione un conjunto de claves de índice que devolverá el conjunto más pequeño cubierto.  
  
3.  Realice uno o más las intersecciones de las claves de índice, para reducir el conjunto cubierto.  
  
4.  Para cada registro en el conjunto de cubierta, evaluar la expresión de filtro, así como la seguridad. Si el filtro se evalúa como TRUE y se concede el acceso, a continuación, devolver este registro al cliente.  
  
El trabajo de optimización de consulta LDAP modifica los pasos 2 y 3, para reducir el tamaño del conjunto cubierto. Más concretamente, la implementación actual selecciona las claves de índice duplicadas y realiza intersecciones redundantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparación entre el algoritmo antiguo y nuevo  
El destino de la búsqueda LDAP ineficaz en este ejemplo es un controlador de dominio de Windows Server 2012.  La búsqueda se completa en aproximadamente 44 segundos porque no se encuentra un índice más eficaz.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Los resultados de ejemplo con el nuevo algoritmo  
En este ejemplo se repite la misma búsqueda exacta que anteriormente pero tiene como destino un controlador de dominio de Windows Server 2012 R2.  La misma búsqueda completa en menos de un segundo debido a las mejoras en el algoritmo de optimizador de consultas LDAP.  
  
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
  
-   Si no se puede optimizar el árbol:  
  
    -   Por ejemplo: una expresión en el árbol de la era a través de una columna no indizada  
  
    -   Grabar una lista de índices que impiden la optimización  
  
    -   Expone a través de seguimiento de ETW y el identificador de evento 1644  
  
        ![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Para habilitar el control de estadísticas de LDP  
  
1.  Abra LDP.exe y conéctese y enlácese a un controlador de dominio.  
  
2.  En el **opciones** menú, haga clic en **controles**.  
  
3.  En el cuadro de diálogo de controles, expanda el **Load Predefined** menú desplegable, haga clic en **estadísticas de búsqueda** y, a continuación, haga clic en **Aceptar**.  
  
    ![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  En el **examinar** menú, haga clic en **búsqueda**  
  
5.  En el cuadro de diálogo de búsqueda, seleccione el **opciones** botón.  
  
6.  Asegúrese del **extendido** casilla está activada en el cuadro de diálogo Opciones de búsqueda y seleccione **Aceptar**.  
  
    ![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Pruebe lo siguiente: Usar LDP para devolver estadísticas de consultas  
Realice lo siguiente en un controlador de dominio o desde un cliente unido al dominio o servidor que tenga instaladas las herramientas de AD DS.  Repita los siguientes destinatarios de su controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2.  
  
1.  Revise el ["Crear más eficaz Microsoft aplicaciones habilitadas para AD"](https://msdn.microsoft.com/library/ms808539.aspx) artículo y la consulte según sea necesario.  
  
2.  Uso de LDP, habilitar las estadísticas de búsqueda (vea [para habilitar el control de estadísticas en LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Realizar varias búsquedas LDAP y observe la información estadística en la parte superior de los resultados.  Se repetirá la misma búsqueda en otras actividades documentarán hasta en un archivo de texto el Bloc de notas.  
  
4.  Realizar una búsqueda LDAP que el optimizador de consultas debe ser capaz de optimizar debido a los índices de atributos  
  
5.  Intenta construir una búsqueda que tarda mucho tiempo en completarse (es posible que desee aumentar el **límite de tiempo** opción forma la búsqueda no no en el tiempo de espera).  
  
### <a name="additional-resources"></a>Recursos adicionales  
[¿Cuáles son las búsquedas de Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Cómo el trabajo de búsquedas de Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Creación de aplicaciones habilitadas para directorio de Microsoft Active más eficientes](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) consultas LDAP se ejecutan más lentamente de lo esperado en Active Directory o se pueden registrar el servicio de directorio LDS/ADAM y 1644 de Id. de evento  
  
## <a name="BKMK_1644"></a>Mejoras en el evento 1644  
  
### <a name="overview"></a>Información general  
Esta actualización agrega estadísticas adicionales de resultados de búsqueda LDAP al identificador de evento 1644 para ayudar a solucionar problemas.  Además, hay un nuevo valor del registro que se puede usar para habilitar el registro de un umbral de tiempo.  Estas mejoras se realizaron disponibles en Windows Server 2012 y Windows Server 2008 R2 SP1 a través de la KB [2800945](https://support.microsoft.com/kb/2800945) y estará disponible para Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Estadísticas de búsqueda LDAP adicionales se agregan al identificador de evento 1644 para ayudar a solucionar problemas de búsquedas LDAP ineficaces o costosas  
> -   Ahora puede especificar un umbral de tiempo de búsqueda (p ej. Evento 1644 del registro para las búsquedas tarda más de 100 ms) en lugar de especificar los valores de umbral de un resultado de búsqueda costoso e ineficiente  
  
### <a name="background"></a>Background  
Durante la solución de problemas de rendimiento de Active Directory, resulta evidente que la actividad de búsqueda LDAP que pueda estar contribuyendo al problema.  Decide habilitar el registro para que puedan ver caros o ineficientes consultas LDAP procesadas por el controlador de dominio.  Con el fin de habilitar el registro, debe establecer el valor de diagnósticos de ingeniería de campo y, opcionalmente, puede especificar los valores de umbral de los resultados de búsqueda ineficaz y costosa.  Tras la habilitación de la ingeniería de campo de nivel de registro en un valor de 5, cualquier búsqueda que cumpla estos criterios se registra en el registro de eventos de servicios de directorio con un identificador de evento 1644.  
  
El evento contiene:  
  
-   Cliente IP y puerto  
  
-   Nodo de inicio  
  
-   Filtro  
  
-   Ámbito de búsqueda  
  
-   Selección de atributo  
  
-   Controles de servidor  
  
-   Entradas visitadas  
  
-   Entradas devueltas  
  
Sin embargo, los datos claves falta en el evento, como la cantidad de tiempo invertido en la operación de búsqueda y (si existe) se usó el índice.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Estadísticas de búsqueda adicional agregadas al evento 1644  
  
-   Índices usados  
  
-   Páginas que se hace referenciadas  
  
-   Páginas leídas del disco  
  
-   Páginas almacenadas leídas desde el disco con anticipación  
  
-   Páginas limpias modificadas  
  
-   Puede modificar las páginas desfasadas  
  
-   Tiempo de búsqueda  
  
-   Impedir la optimización de atributos  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuevo valor del registro de umbral de duración definida para el registro de evento 1644  
En lugar de especificar los valores de umbral de un resultado de búsqueda costoso e ineficiente, puede especificar el umbral de tiempo de búsqueda.  Si quisiera iniciar todos los resultados de búsqueda que tardaron 50 ms o mayor, debería especificar 50 decimal o hexadecimal de 32 (además de establecer el valor de ingeniería de campo).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparación de la anterior y nuevo identificador de evento 1644  
ANTIGUO  
  
![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NUEVO  
  
![las actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Pruebe lo siguiente: Utilice el registro de eventos para devolver estadísticas de consultas  
  
1.  Repita los siguientes destinatarios de su controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2. Observe el evento ID 1644s en ambos controladores de dominio después de cada búsqueda.  
  
2.  Utilizando regedit, habilite el registro de identificador de evento 1644 con un umbral de duración definida en el controlador de dominio de Windows Server 2012 R2 y el método anterior en el controlador de dominio de Windows Server 2012.  
  
3.  Realizar varias búsquedas LDAP que superen el umbral y observar la información estadística en la parte superior de los resultados.  Usar las consultas LDAP que ha anotado anteriormente y repetir las mismas búsquedas.  
  
4.  Realizar una búsqueda LDAP que el optimizador de consultas no es capaz de optimizar porque uno o varios atributos no están indizados.  
  
## <a name="BKMK_ADRepl"></a>Mejora del rendimiento de replicación de Active Directory  
  
### <a name="overview"></a>Información general  
La replicación de AD utiliza RPC para su transporte de replicación. De forma predeterminada, RPC usa un búfer de transmisión de 8K y un tamaño de paquete de 5K. Esto tiene el efecto neto donde la instancia envío transmitir paquetes de tres (aproximadamente 15K que vale la pena de datos) y, a continuación, tiene que esperar una red de ida y vuelta antes de enviar más. Suponiendo que un 3ms tiempo de ida y vuelta, el mayor rendimiento sería alrededor de 40 Mbps, incluso en 1 Gbps o 10 Gbps redes.  
  
> [!NOTE]  
> -   Esta actualización ajusta el rendimiento máximo de la replicación de AD de 40 Mbps a unos 600 Mbps.  
>   
>     -   Aumenta el tamaño del búfer de envío RPC lo que reduce el número de red de ida y vuelta  
> -   El efecto será el más notable de alta velocidad, red de latencia alta.  
  
Estas actualizaciones aumentan el rendimiento máximo a unos 600 Mbps cambiando el tamaño del búfer de envío RPC de 8K a 256KB.  Este cambio permite que el tamaño de ventana TCP crecer más allá de 8 KB, lo que reduce el número de red de ida y vuelta.  
  
> [!NOTE]  
> No hay ningún valor configurable para modificar este comportamiento.  
  
### <a name="additional-resources"></a>Recursos adicionales  
[Cómo funciona el modelo de replicación de Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


