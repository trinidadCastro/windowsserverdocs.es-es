---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Actualizaciones de componentes de servicios de directorio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>Actualizaciones de componentes de servicios de directorio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
En esta lección explica las actualizaciones de componentes de servicios de directorio en Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Lo que aprenderá  
Se explican las nuevas actualizaciones de componente de servicios de directorio siguientes:  
  
-   Se explican las nuevas actualizaciones de componente de servicios de directorio siguientes:  
  
    -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Desuso de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Cambios del optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Mejoras de evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Mejora de rendimiento de replicación de directorio activo](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Niveles funcionales de dominios y bosques  
  
### <a name="overview"></a>Introducción  
La sección proporciona una breve introducción a los cambios de nivel funcionales de bosque y dominio.  
  
### <a name="new-dfl-and-ffl"></a>FFL y DFL nuevo  
Con el lanzamiento, existen nuevos niveles funcionales de bosque y dominio:  
  
-   Nivel funcional del bosque: Windows Server 2012 R2  
  
-   Nivel funcional de dominio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 nivel funcional del dominio habilita la compatibilidad con las siguientes acciones:  
  
1.  Protecciones de DC lado para *usuarios protegido*  
  
    *Protegido a los usuarios* puede autenticar a un dominio de Windows Server 2012 R2 **ya no**:  
  
    -   Autenticar con la autenticación NTLM  
  
    -   Usar conjuntos de cifrado DES o RC4 de autenticación previa de Kerberos  
  
    -   Delegarse con la delegación restringida o sin restricciones  
  
    -   Renovación de vales (TGT) de usuario más allá de la duración inicial 4 horas  
  
2.  Directivas de autenticación  
  
    Bosque nuevo Active Directory directivas basadas que pueden aplicarse a cuentas en los dominios de Windows Server 2012 R2 para controlar qué hosts en una cuenta puede sesión desde y aplicar las condiciones de control de acceso para la autenticación a los servicios que se ejecutan con una cuenta  
  
3.  Silos de directiva de autenticación  
  
    Basado en el bosque nuevo objeto de Active Directory que puede crear una relación entre cuentas de usuario, un servicio administrado y equipo para usarse para clasificar las cuentas de directivas de autenticación o de aislamiento de autenticación.  
  
Consulta cómo configurar cuentas protegido para obtener más información.  
  
Además de las características anteriores, el nivel funcional de dominio de Windows Server 2012 R2 garantiza que cualquier controlador de dominio en el dominio ejecuta Windows Server 2012 R2.  
El nivel funcional del bosque de Windows Server 2012 R2 no proporciona ninguna característica nueva, pero garantiza que cualquier nuevo dominio creado en el bosque funcionará automáticamente en el nivel funcional de dominio de Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Mínimo DFL aplica en la nueva creación de dominio  
Windows Server 2008 DFL es el nivel funcional mínimo compatible en la nueva creación de dominio.  
  
> [!NOTE]  
> La degradación de FRS se logra mediante la eliminación de la capacidad de instalar un nuevo dominio con un nivel funcional del dominio inferior a Windows Server 2008 con el administrador del servidor o a través de Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reduce los niveles funcionales de bosque y dominio  
Los niveles funcionales de bosque y dominio se establecen en Windows Server 2012 R2 en la nueva creación de dominio y bosque nuevo de forma predeterminada, pero pueden reducirse mediante Windows PowerShell.  
  
Para subir o bajar el nivel funcional del bosque mediante Windows PowerShell, usa el **conjunto ADForestMode** cmdlet.  
  
**Para establecer el contoso.com FFL en modo de Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Para subir o bajar el nivel funcional del dominio mediante Windows PowerShell, usa el cmdlet de Set-ADDomainMode.  
  
**Para establecer el contoso.com DFL en modo de Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Promoción de un controlador de dominio que ejecutan Windows Server 2012 R2 como una réplica adicional en un dominio existente ejecutando 2003 DFL funciona.  
  
Creación de dominio nuevo en un bosque existente  
  
![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
No existen bosque nuevo o las operaciones de dominio en esta versión.  
  
Estos archivos .ldf contengan los cambios de esquema para la **servicio de registro de dispositivo**.  
  
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
  
## <a name="BKMK_NTFRS"></a>Desuso de NTFRS  
  
### <a name="overview"></a>Introducción  
FRS está en desuso en Windows Server 2012 R2.  La degradación de FRS se logra aplicando mínimo nivel funcional de dominio (DFL) de Windows Server 2008.  Esta aplicación está presente solo si el nuevo dominio se crea mediante el administrador del servidor o Windows PowerShell.  
  
Usa el parámetro - DomainMode con los cmdlets Install-ADDSForest o Install-ADDSDomain para especificar el nivel funcional del dominio.  Los valores admitidos para este parámetro se pueden ser un entero o un valor de cadena enumerado correspondiente. Por ejemplo, para establecer el nivel de modo de dominio en Windows Server 2008 R2, puede especificar el valor de 4 o "Win2008R2".  Cuando se ejecuta estos cmdlets desde Server 2012 R2 válido valores son los de Windows Server 2008 (3, Win2008) (4, Win2008R2) de Windows Server 2008 R2 (5, Win2012) de Windows Server 2012 y Windows Server 2012 R2 (6, Win2012R2). El nivel funcional del dominio no puede ser menor que el nivel funcional del bosque, pero puede ser mayor.  Dado que FRS está en desuso en esta versión, Windows Server 2003 (2, Windows 2003) no es un parámetro reconocido con estos cmdlets cuando se ejecuta en Windows Server 2012 R2.  
  
![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Cambios del optimizador de consultas LDAP  
  
### <a name="overview"></a>Introducción  
El algoritmo de optimizador de consulta LDAP se vuelve a evaluar y aún más optimizado.  El resultado es la mejora de rendimiento en la eficacia de la búsqueda LDAP y la hora de búsqueda LDAP de consultas complejas.  
  
> [!NOTE]  
> **El desarrollador:**mejoras en el rendimiento de las búsquedas a través de mejoras en la asignación de LDAP de consulta para consultar ESE.  Filtros LDAP más allá de un cierto nivel de complejidad evitar la selección de índice optimizada, lo que resulta en drásticamente una disminución del rendimiento (x 1000 o más). Este cambio altera la forma en que se seleccionan índices para consultas LDAP evitar este problema.  
  
> [!NOTE]  
> Una revisión completa el algoritmo de optimizador de consulta LDAP, lo que resulta en:  
>   
> -   Tiempos de búsqueda  
> -   Eficiencia permitir que los controladores de dominio hacer más cosas  
> -   Menos llamadas de soporte técnico problemas en cuanto a rendimiento de los anuncios  
> -   Atrás migrado a Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>En segundo plano  
La capacidad de buscar en Active Directory es un servicio de core proporcionado por los controladores de dominio.  Otros servicios y aplicaciones empresariales dependen de las búsquedas de Active Directory.  Las operaciones empresariales pueden dejar de detenerse si esta característica no está disponible.  Como un servicio muy usa y core, es imprescindible que los controladores de dominio controlan el tráfico de la búsqueda LDAP eficaz.  El algoritmo de optimizador de consulta LDAP intenta realizar búsquedas LDAP eficientes como sea posible mediante la asignación de filtros de búsqueda LDAP a un conjunto de resultados que se puede satisfacer a través de registros que ya se indexan en la base de datos.  Este algoritmo se vuelve a evaluar y aún más optimizada.  El resultado es la mejora de rendimiento en la eficacia de la búsqueda LDAP y la hora de búsqueda LDAP de consultas complejas.  
  
### <a name="details-of-change"></a>Detalles del cambio  
Contiene una búsqueda LDAP:  
  
-   Una ubicación (encabezado NC, unidad organizativa, objeto) dentro de la jerarquía para iniciar la búsqueda  
  
-   Un filtro de búsqueda  
  
-   Una lista de atributos que se devuelven  
  
El proceso de búsqueda puede resumirse de la siguiente manera:  
  
1.  Simplificar el filtro de búsqueda si es posible.  
  
2.  Selecciona un conjunto de claves de índice que devolverá el conjunto cubierto más pequeño.  
  
3.  Realiza uno o más intersecciones de claves de índice, para reducir el conjunto de cobertura.  
  
4.  Para cada registro en el conjunto de cobertura, evaluar la expresión de filtro, así como la seguridad. Si el filtro se evalúa en TRUE y conceder el acceso, a continuación, devuelve este registro al cliente.  
  
El trabajo de optimización de consulta LDAP modifica los pasos 2 y 3, para reducir el tamaño del conjunto de cobertura. Más concretamente, la actual implementación selecciona duplicados indexar claves y realiza intersecciones redundante.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparación entre algoritmo antiguo y nuevo  
El destino de la búsqueda LDAP ineficiente en este ejemplo es un controlador de dominio de Windows Server 2012.  La búsqueda se completa en aproximadamente 44 segundos porque no se encuentra un índice más eficaz.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Muestra los resultados mediante el algoritmo nuevo  
Este ejemplo repite la misma búsqueda exactamente como se indicó anteriormente, pero está destinada a un controlador de dominio de Windows Server 2012 R2.  La búsqueda misma completa en menos de un segundo debido a las mejoras en el algoritmo de optimizador de consulta LDAP.  
  
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
  
    -   Por ejemplo: una expresión en el árbol se encontraba sobre una columna no está indizada  
  
    -   Grabar una lista de índices que impiden que optimización  
  
    -   Expuesta mediante el seguimiento de ETW y 1644 de identificador de evento  
  
        ![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Para habilitar el control de estadísticas en LDP  
  
1.  Abrir LDP.exe y conectar y enlazar a un controlador de dominio.  
  
2.  En la **opciones** menú, haz clic en **controles**.  
  
3.  En el cuadro de diálogo de controles, expande el **carga predefinidos** menú desplegable, haz clic en **estadísticas de búsqueda** y, a continuación, haz clic en **Aceptar**.  
  
    ![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  En la **examinar** menú, haz clic en **búsqueda**  
  
5.  En el cuadro de búsqueda, selecciona el **opciones** botón.  
  
6.  Garantizar la **Extended** casilla está activada en el cuadro de diálogo Opciones de búsqueda y selecciona **Aceptar**.  
  
    ![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Lo prueba siguiente: Uso LDP para devolver las estadísticas de consulta  
Realizar lo siguiente en un controlador de dominio, o desde un cliente del dominio o el servidor que tenga instaladas las herramientas de AD DS.  Repite los siguientes destinados a tu controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2.  
  
1.  Revisa la ["Crear más eficiente Microsoft aplicaciones habilitadas para AD"](https://msdn.microsoft.com/library/ms808539.aspx) del artículo y hacer referencia a ella según sea necesario.  
  
2.  Uso de LDP, habilitar las estadísticas de búsqueda (consulta [para habilitar el control de estadísticas en LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Realizar varias búsquedas LDAP y observa la información estadística en la parte superior de los resultados.  Tendrá que repetir la búsqueda en sí mismo actividades documentarán por lo tanto, en un archivo de texto el Bloc de notas.  
  
4.  Realizar una búsqueda LDAP que el optimizador de consultas debe ser capaces de optimizar debido a los índices de atributos  
  
5.  Intenta crear una búsqueda que tarda demasiado tiempo en completarse (quieres aumentar el **límite de tiempo** opción para la búsqueda hace tiempo de espera).  
  
### <a name="additional-resources"></a>Recursos adicionales  
[¿Cuáles son las búsquedas de Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Cómo trabajo búsquedas en Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Crear más eficientes aplicaciones de Microsoft Active Directory habilitado](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) consultas LDAP se ejecutan más lento de lo esperado en el anuncio o se pueden grabar el servicio de directorio LDS/ADAM y 1644 de identificador de evento  
  
## <a name="BKMK_1644"></a>Mejoras de evento 1644  
  
### <a name="overview"></a>Introducción  
Esta actualización agrega estadísticas de resultados de búsqueda LDAP adicionales al evento 1644 identificador para ayudar a solucionar problemas.  Además, hay un nuevo valor del registro que se puede usar para habilitar el registro en un umbral de tiempo.  Estas mejoras se realizaron disponibles en Windows Server 2012 y Windows Server 2008 R2 SP1 mediante KB [2800945](https://support.microsoft.com/kb/2800945) y estarán disponibles para Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Estadísticas de búsqueda LDAP adicionales se agregan al evento 1644 identificador para ayudar a solucionarlos ineficientes o costosas búsquedas LDAP  
> -   Ahora puedes especificar un umbral de tiempo de búsqueda (por ej. Registro de eventos 1644 para búsquedas tardando más de 100 ms) en lugar de especificar los valores de umbral de resultados de búsqueda costoso e ineficiente  
  
### <a name="background"></a>En segundo plano  
Para la solución de problemas de rendimiento de Active Directory, se hace evidente que actividad de búsqueda LDAP puede estar causando el problema.  Tú decides a habilitar el registro para que pueda ver costosas o ineficientes consultas LDAP procesadas por el controlador de dominio.  Para habilitar el registro, debes establecer el valor de diagnósticos de ingeniería de campo y, opcionalmente, puede especificar los valores de umbral de resultados de búsqueda ineficiente costoso.  Al habilitar la ingeniería de campo de nivel de registro en un valor de 5, cualquier búsqueda que cumpla estos criterios se registra en el registro de eventos de servicios de directorio con un 1644 de identificador de evento.  
  
El evento contiene:  
  
-   Cliente IP y puerto  
  
-   Nodo de inicio  
  
-   Filtro  
  
-   Ámbito de búsqueda  
  
-   Selección de atributo  
  
-   Controles de servidor  
  
-   Entradas visitadas  
  
-   Entradas devueltas  
  
Sin embargo, datos de la clave están que faltan del evento, como la cantidad de tiempo invertido en la operación de búsqueda y (si existe) se usó el índice.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Estadísticas de búsqueda adicionales agregadas al evento 1644  
  
-   Usados índices  
  
-   Páginas de referencia  
  
-   Páginas que se leen desde el disco  
  
-   Páginas almacenadas leídas con anticipación desde el disco  
  
-   Limpias páginas modificadas  
  
-   Sucios páginas modificadas  
  
-   Tiempo de búsqueda  
  
-   Evitar la optimización de atributos  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuevo valor del registro de umbral de tiempo para el registro de eventos 1644  
En lugar de especificar los valores de umbral de resultados de búsqueda costoso e ineficiente, puedes especificar el umbral de tiempo de búsqueda.  Si puedes deseado para iniciar sesión todos los resultados de búsqueda que haya tomado 50 ms o mayor, se especificaría 50 decimal / 32 hexadecimal (además de establecer el valor de ingeniería de campo).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparación del evento antiguo y nuevo 1644 Id.  
ANTIGUO  
  
![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
Nuevo  
  
![Actualizaciones de servicios de directorio](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Lo prueba siguiente: Usar el registro de eventos para devolver estadísticas de consulta  
  
1.  Repite los siguientes destinados a tu controlador de dominio de Windows Server 2012 y el controlador de dominio de Windows Server 2012 R2. Observa el 1644s de identificador de evento en dos controladores de dominio después de cada búsqueda.  
  
2.  Usar regedit, habilitar el registro de 1644 de identificador de evento con un umbral de tiempo en el controlador de dominio de Windows Server 2012 R2 y el método anterior en el controlador de dominio de Windows Server 2012.  
  
3.  Realiza varias búsquedas LDAP que superan el umbral y observan la información estadística en la parte superior de los resultados.  Usa consultas LDAP que documentado anteriormente y repite las mismas búsquedas.  
  
4.  Realizar una búsqueda LDAP que el optimizador no es capaz de optimizar porque no se indizan uno o varios atributos.  
  
## <a name="BKMK_ADRepl"></a>Mejora de rendimiento de replicación de directorio activo  
  
### <a name="overview"></a>Introducción  
Replicación de AD utiliza RPC para su transporte de replicación. De manera predeterminada, RPC usa un búfer de transmisión de 8K y un tamaño de paquete de 5 KB. Esto tiene el efecto neto donde la instancia envía transmitir tres paquetes (aproximadamente 15K vale la pena de datos) y, a continuación, tendrás que esperar a una red viaje de ida y antes de enviar más. Suponiendo que un 3 MS tiempo de ida y vuelta, sería el mayor rendimiento alrededor 40Mbps, incluso en 1Gbps o 10 GB/s redes.  
  
> [!NOTE]  
> -   Esta actualización ajusta el rendimiento máximo de replicación de AD de 40Mbps a alrededor de 600 Mbps.  
>   
>     -   Aumenta el tamaño de búfer de envío RPC lo que reduce el número de red viajes de ida y  
> -   El efecto será más perceptible en alta velocidad, red de alta latencia.  
  
Estas actualizaciones aumentan el rendimiento máximo para alrededor de 600 Mbps cambiando el tamaño del búfer de envío RPC de 8K a 256KB.  Este cambio permite que el tamaño de ventana TCP supere 8K, viajes de ida y reducir el número de red.  
  
> [!NOTE]  
> No hay ningún opciones configurables para modificar el comportamiento.  
  
### <a name="additional-resources"></a>Recursos adicionales  
[Cómo funciona el modelo de réplica de Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


