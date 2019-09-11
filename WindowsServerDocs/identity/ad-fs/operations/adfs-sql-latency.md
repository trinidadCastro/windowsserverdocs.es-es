---
title: Ajustar SQL y abordar problemas de latencia con AD FS
description: En este documento se explica cómo ajustar SQL con AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 29c8e8ba52f62a335ab136756e759b6114ecfb20
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865609"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ajustar SQL y abordar problemas de latencia con AD FS
En una actualización de [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) se presentaron las siguientes mejoras para reducir la latencia entre bases de datos. Una próxima actualización de AD FS 2019 incluirá estas mejoras.

## <a name="in-memory-cache-update-in-background-thread"></a>Actualización de caché en memoria en el subproceso en segundo plano 
En las implementaciones anteriores de disponibilidad de AlwaysOn (AoA), la latencia existía para cualquier operación de "lectura", ya que el nodo principal se podía encontrar en un centro de recursos independiente. La llamada entre dos centros de recursos diferentes dio lugar a una latencia.  

En la actualización más reciente de AD FS, una reducción de la latencia se destina a través de la adición de un subproceso en segundo plano para actualizar la memoria caché de configuración de AD FS y una configuración para establecer el período de tiempo de actualización. El tiempo empleado para una búsqueda en la base de datos se reduce significativamente en el subproceso de solicitud, ya que las actualizaciones de la caché de base de datos se mueven al subproceso en segundo plano.  

`backgroundCacheRefreshEnabled` Cuando se establece en true, AD FS permitirá al subproceso en segundo plano ejecutar actualizaciones de caché. La frecuencia de captura de datos de la memoria caché se puede personalizar a un valor `cacheRefreshIntervalSecs`de tiempo estableciendo. El valor predeterminado se establece en 300 segundos cuando `backgroundCacheRefreshEnabled` se establece en true. Después de la duración del valor establecido, AD FS comienza a actualizar su memoria caché y mientras la actualización está en curso, se seguirán usando los datos de la memoria caché antiguos.  

>[!NOTE]
> Los datos de la memoria caché se actualizarán fuera del `cacheRefreshIntervalSecs` valor si ADFS recibe una notificación de SQL que indica que se ha producido un cambio en la base de datos. Esta notificación desencadenará la memoria caché que se va a actualizar. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recomendaciones para establecer la actualización de la memoria caché 
El valor predeterminado de la actualización de la memoria caché es de **cinco minutos**. Se recomienda establecerlo en **1 hora** para reducir una actualización de datos innecesaria por AD FS porque los datos de la memoria caché se actualizarán en caso de que se produzcan cambios en SQL.  

AD FS registra una devolución de llamada para los cambios de SQL y, tras un cambio, ADFS recibe una notificación. A través de este método, ADFS recibe cada nuevo cambio de SQL en cuanto se produce. 

En caso de un problema de red que tenga como resultado AD FS falta la notificación de SQL, AD FS se actualizará en el intervalo especificado por el valor de actualización de la memoria caché. Si se sospecha algún problema de conectividad entre AD FS y SQL, se recomienda establecer el valor de actualización de la memoria caché en un valor inferior a 1 hora.  

### <a name="configuration-instructions"></a>Instrucciones de configuración 
El archivo de configuración admite varias entradas de caché. La siguiente lista se puede configurar en función de las necesidades de su organización. 

En el ejemplo siguiente se habilita la actualización de la caché en segundo plano y se establece el período de actualización de la memoria caché en 1800 segundos o 30 minutos. Esto se debe hacer en cada nodo de ADFS y el servicio ADFS debe reiniciarse después. Los cambios no afectan a otros nodos y prueban el primer nodo antes de realizar el cambio en todos los nodos. 

  1. Navegue hasta el archivo de configuración de AD FS y, en la sección "Microsoft. IdentityServer. Service", agregue la entrada siguiente:  
  
  - `backgroundCacheRefreshEnabled`: Especifica si la característica de caché de fondo está habilitada. valores "true/false".
  - `cacheRefreshIntervalSecs`: Valor en segundos en el que ADFS actualizará la memoria caché. AD FS actualizará la memoria caché si hay algún cambio en SQL. AD FS recibirá una notificación de SQL y actualizará la memoria caché.  
 
 >[!NOTE]
 > Todas las entradas del archivo de configuración distinguen mayúsculas de minúsculas.  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
Valores configurables adicionales admitidos: 

   - **maxRelyingPartyEntries** : número máximo de entradas de usuario de confianza que AD FS conservarán en la memoria. Este valor también lo usa la caché de permisos de la aplicación oAuth. Si hay más permisos de aplicación que RPs y si todos se almacenan en memoria, este valor debe ser el número de permisos de la aplicación. El valor predeterminado es 1000.
   - **maxIdentityProviderEntries** : este es el número máximo de entradas de proveedor de notificaciones AD FS mantendrá en la memoria. El valor predeterminado es 200. 
   - **maxClientEntries** : este es el número máximo de entradas de cliente de OAuth AD FS mantendrá en la memoria. El valor predeterminado es 500. 
   - **maxClaimDescriptorEntries** : número máximo de entradas de descriptor de notificaciones AD FS mantendrá en la memoria. El valor predeterminado es 500. 
   - **maxNullEntries** : se usa como caché negativa. Cuando AD FS busca una entrada en la base de datos y no se encuentra, AD FS agrega en caché negativa. Este es el tamaño máximo de la memoria caché. Hay una caché negativa para cada tipo de objetos, no es una memoria caché única para todos los objetos. El valor predeterminado es 50, 0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Compatibilidad con varias bases de los distintos centros de recursos 
En el caso de las configuraciones anteriores de varios centros de datos, AD FS solo ha admitido una sola base de datos de artefacto, lo que provoca una latencia del centro de datos entre centros durante las llamadas de recuperación.  

Para reducir la latencia entre centros de recursos, un administrador de AD FS ahora puede implementar varias instancias de base de archivos de artefacto y, a continuación, modificar el archivo de configuración de un nodo AD FS para que apunte a instancias de base de referencia de artefacto diferentes. La cadena de conexión de la base de datos de artefactos se puede proporcionar en el archivo de configuración que permite una base de datos de artefacto por nodo. Si la cadena de conexión no está presente en el archivo de configuración, el nodo revertirá al diseño anterior para usar la base de datos de artefactos que se encuentra en la base de datos de configuración.  
Los entornos híbridos también se admiten con esta configuración.  

### <a name="requirements"></a>Requisitos: 
Antes de configurar la compatibilidad con varias bases de datos de artefactos, ejecute una actualización en todos los nodos y actualice los archivos binarios, ya que las llamadas de varios nodos se producen a través de esta característica. 
  1. Generar script de implementación para crear la base de de artefacto: Para implementar varias instancias de base de los artefactos, un administrador tendrá que generar el script de implementación de SQL para la base de de artefacto. Como parte de esta actualización, el cmdlet `Export-AdfsDeploymentSQLScript`existente se ha actualizado para tomar opcionalmente un parámetro que especifica la base de datos de AD FS para la que se va a generar un script de implementación de SQL. 
 
 Por ejemplo, para generar el script de implementación solo para la base de archivo de `-DatabaseType` artefacto, especifique el parámetro y pase el valor "artefacto". El parámetro `-DatabaseType` opcional especifica el tipo de base de datos AD FS y se puede establecer en: All (valor predeterminado), artefacto o Configuration. Si no `-DatabaseType` se especifica ningún parámetro, el script configurará el artefacto y los scripts de configuración.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
El script generado se debe ejecutar en el equipo de SQL para crear las bases de datos necesarias y dar a la cuenta de servicio de AD FS, permisos de SA de SQL para esas bases de datos.

 2. Cree la base de de artefacto mediante el script de implementación. Copie los scripts de implementación de CreateDB. SQL y SetPermissions. SQL recién generados en el equipo de SQL Server y ejecútelo para crear la base de de artefacto local. 
 
 3. Modifique el archivo de configuración para agregar la conexión de la base de de los artefactos. 
 Navegue hasta el archivo de configuración del nodo de AD FS y, en la sección "Microsoft. IdentityServer. Service", agregue un punto de entrada a la ArtifactDB recién configurada. 

 >[!NOTE] 
 > artifactStore y connectionString son valores que distinguen mayúsculas de minúsculas. Asegúrese de que están configurados correctamente. &lt;artifactStore connectionString = "Data Source = .\SQLInstance; Integrated Security = true; Initial Catalog = AdfsArtifactStore"/&gt; 
>
>Use un valor de origen de datos que coincida con la conexión SQL.



 4. Reinicie el servicio AD FS para que los cambios surtan efecto. 
 
 >[!NOTE] 
 > No se recomienda usar la replicación de SQL o la sincronización entre las bases de datos de artefactos. La recomendación es configurar una base de datos de artefacto por centro de datos. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Conmutación por error de varios centros de datos y recuperación de base de datos  
Se recomienda crear bases de datos de artefactos de conmutación por error en el mismo centro de datos que la base de datos de artefactos maestros. Si se produce una conmutación por error, no habrá aumento de la latencia. No se recomienda utilizar bases de datos de artefactos de conmutación por error entre centros de datos. A continuación se detallan las llamadas a la función de detección de reproducción de tokens y OAuth, SAML, ESL y de repetición con varias bases de datos de artefactos. 
 - **OAuth y SAML** 

   En el caso de las solicitudes de artefactos de OAuth y SAML, el nodo creará el artefacto en la base de de artefacto presente en el archivo de configuración. Si el archivo de configuración no contiene una conexión de base de datos de artefacto, utilizará la base de datos de artefacto común. Cuando la siguiente solicitud para capturar el artefacto va a otro nodo, el otro nodo realizará la API de REST en el primer nodo para capturar el artefacto de la base de de artefacto. Esto es necesario porque diferentes nodos pueden tener diferentes bases de datos de artefactos y los nodos no lo saben. Si el primer nodo está inactivo, se producirá un error en la resolución del artefacto. Debido a este diseño, no es necesario replicar la base de de los artefactos entre distintos centros de recursos. Si un centro de centros de recursos entero está inactivo, lo más probable es que el nodo que creó el artefacto también esté inactivo, lo que significa que el artefacto ya no se puede resolver.  

 - **Bloqueo de extranet** 

    La base de datos de artefactos a la que se hace referencia en el archivo de configuración se usará para los datos de bloqueo de la extranet. Sin embargo, para la característica ESL, AD FS elige un maestro que escribe los datos en la base de datos del artefacto. Todos los nodos realizan una llamada API de REST al nodo maestro para obtener y establecer la información más reciente sobre cada usuario. Si hay varias bases de BD de artefactos en uso, el administrador debe seleccionar un nodo maestro para cada base de de de artefacto o centro de centros de recursos. 

    Para seleccionar un nodo para que sea el maestro de ESL, vaya al archivo de configuración del nodo de ADFS y, en la sección "Microsoft. IdentityServer. Service", agregue lo siguiente:       
    
    En el patrón, agregue la siguiente entrada. Tenga en cuenta que las tres claves distinguen mayúsculas de minúsculas. 

    &lt;useractivityfarmrole masterFQDN = [FQDN del principal seleccionado] isMaster = "true"/&gt;
    
    En los otros nodos, agregue la siguiente entrada:

   &lt;useractivityfarmrole masterFQDN = [FQDN del principal seleccionado] isMaster = "false"/&gt;
 
    >[!NOTE] 
    >Dado que varias bases de datos de artefactos no sincronizan los datos, los valores de ESL no se sincronizarán entre las bases de datos de artefacto.
    Un usuario puede alcanzar un centro de centros de recursos diferente para una solicitud, por lo que la ExtranetLockoutThreshold depende del número de bases de los artefactos, ExtranetLockoutThreshold * número de bases de los artefactos. 
 
  - **Detección de reproducción de tokens** 
    
    Siempre se llama a los datos de detección de reproducción de tokens desde la base de datos central de artefactos. AD FS guarda el token de la confianza del proveedor de notificaciones, asegurándose de que no se pueda reproducir el mismo token. Si un atacante intenta reproducir el mismo token, AD FS comprueba si el token existe en la base de los artefactos. Si el token está presente, se rechazará la solicitud. La base de datos de artefacto central se usa para la seguridad, ya que los datos de la base de datos de artefactos no se replican, un atacante podría enviar la solicitud a otro centro de datos y reproducir un token. La creación de copias de solo lectura adicionales de ArtifactDB no impedirá la latencia entre centros de datos en este escenario, ya que solo se utiliza la base de datos de artefacto central.    
 
 