---
title: Ajustar SQL y solución de problemas de latencia con AD FS
description: Este documento describe cómo optimizar SQL con AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322855"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ajustar SQL y solución de problemas de latencia con AD FS
En una actualización para [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) nos presenta las siguientes mejoras para reducir entre la latencia de la base de datos. Una próxima actualización para AD FS 2019 incluirá estas mejoras.

## <a name="in-memory-cache-update-in-background-thread"></a>Actualización de caché en memoria en el subproceso en segundo plano 
En las implementaciones de disponibilidad (AoA) de AlwaysOn anterior, latencia existía para cualquiera "Operación de lectura" como el nodo maestro se pueden ubicar en un centro de datos independiente. Dan como resultado la llamada entre dos centros de datos diferentes en la latencia.  

En la última actualización a AD FS, una reducción en la latencia está destinada a través de la adición de un subproceso en segundo plano para actualizar la caché de configuración de AD FS y una configuración para establecer el período de actualización. El tiempo empleado para realizar una búsqueda de la base de datos se ha reducido significativamente en el subproceso de solicitud, como las actualizaciones de caché de base de datos se mueven en el subproceso en segundo plano.  

Cuando el `backgroundCacheRefreshEnabled` se establece en true, AD FS permitirá que el subproceso en segundo plano ejecutar actualizaciones de la caché. Se puede personalizar la frecuencia de captura de datos de la memoria caché en un valor de tiempo estableciendo `cacheRefreshIntervalSecs`. El valor predeterminado se establece en 300 segundos cuando `backgroundCacheRefreshEnabled` está establecido en true. Después de que el conjunto de valor de duración, AD FS comienza a actualizar su caché y mientras la actualización está en curso, continuará los datos antiguos de la memoria caché que se usará.  

>[!NOTE]
> Datos de la memoria caché se actualizarán fuera de la `cacheRefreshIntervalSecs` valor si AD FS recibe una notificación de lo que significa que se ha producido un cambio en la base de datos SQL. Esta notificación desencadenará la memoria caché para que se actualice. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recomendaciones para configurar la actualización de la caché 
El valor predeterminado para la actualización de la caché es **cinco minutos**. Se recomienda establecerlo en **1 hora** para reducir una actualización de datos innecesarios por AD FS porque los datos de la memoria caché se actualizarán si se producen los cambios SQL.  

AD FS registra una devolución de llamada para los cambios SQL y, tras un cambio, AD FS recibe una notificación. Mediante este método, ADFS recibe cada nuevo cambio de SQL, en cuanto se produce. 

Si se produce un problema de red que los resultados en AD FS falta la notificación de SQL, AD FS se actualizará en el intervalo especificado por la caché de actualizar el valor. Si se sospecha de existencia de problemas de conectividad entre AD FS y SQL, se recomienda establecer el valor de la actualización de caché inferior a 1 hora.  

### <a name="configuration-instructions"></a>Instrucciones de configuración 
El archivo de configuración es compatible con varias entradas de caché. Lo siguiente que se enumeran a continuación puede todos configurarse según las necesidades de su organización. 

El ejemplo siguiente habilita la actualización de caché en segundo plano y establece el período de actualización de caché en 1.800 segundos, o 30 minutos. Esto debe hacerse en cada nodo ADFS y después se debe reiniciar el servicio de ADFS. Los cambios no afectar a otros nodos y probar el primer nodo antes de realizar el cambio en todos los nodos. 

  1. Navegue hasta el archivo de configuración de AD FS y en la sección "Microsoft.IdentityServer.Service", agregue la siguiente entrada:  
  
  - `backgroundCacheRefreshEnabled`  -Especifica si está habilitada la característica de caché en segundo plano. valores "true" o "false".
  - `cacheRefreshIntervalSecs` -Valor en segundos en el que ADFS actualizará la memoria caché. AD FS actualizará la memoria caché si hay algún cambio en SQL. AD FS recibirá una notificación de SQL y actualizar la memoria caché.  
 
 >[!NOTE]
 > Todas las entradas del archivo de configuración distinguen mayúsculas de minúsculas.  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
Valores configurables compatibles adicionales: 

   - **maxRelyingPartyEntries** - máximo número de entradas de terceros para usuario autenticado que AD FS se mantendrá en memoria. Este valor también se utiliza la caché de permiso de aplicación de oAuth. Si hay más permisos de aplicación de RP y si todo se almacenarán en memoria, este valor debe ser el número de permisos de la aplicación. El valor predeterminado es 1000.
   - **maxIdentityProviderEntries** : este es el número máximo de notificaciones de AD FS se mantendrá en memoria de entradas de proveedor. El valor predeterminado es 200. 
   - **maxClientEntries** : este es el número máximo de entradas de cliente OAuth AD FS se mantendrá en memoria. El valor predeterminado es 500. 
   - **maxClaimDescriptorEntries** - máximo número de entradas del descriptor de notificación de AD FS se mantendrá en memoria. El valor predeterminado es 500. 
   - **maxNullEntries** -se usa como caché negativa. Cuando AD FS busca una entrada en la base de datos y no se encuentra, AD FS se incorpora en caché negativa. Este es el tamaño máximo de esa caché. No hay caché negativa para cada tipo de objetos, no es una memoria caché única para todos los objetos. El valor predeterminado es 50,0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Compatibilidad con varios artefactos de base de datos en centros de datos 
Las configuraciones anteriores de varios centros de datos, AD FS ha solamente admite una sola base de datos del artefacto, causando la latencia de center entre centros de datos durante las llamadas de recuperación.  

Para reducir la latencia entre centros de datos, un administrador de AD FS puede ahora implementar varias instancias de base de datos del artefacto y, a continuación, modifique el archivo de configuración de un nodo de AD FS para que apunte a las instancias de base de datos de artefacto diferente. La cadena de conexión de base de datos de artefacto puede proporcionarse en el archivo de configuración que permite a una base de datos de artefacto por nodo. Si la cadena de conexión no está presente en el archivo de configuración, el nodo se revertirá al diseño anterior para usar la base de datos de artefacto que está presente en la base de datos de configuración.  
Entornos híbridos también son compatibles con esta configuración.  

### <a name="requirements"></a>Requisitos: 
Antes de configurar la compatibilidad con base de datos de varios artefactos, ejecute una actualización en todos los nodos y actualizar los archivos binarios desde que se produzcan las llamadas de varios nodos a través de esta característica. 
  1. Generar script de implementación para crear la base de datos de artefacto: Para implementar varias instancias de base de datos de artefacto, un administrador debe generar el script de implementación de SQL para la base de datos de artefacto. Como parte de esta actualización, existente `Export-AdfsDeploymentSQLScript`cmdlet se ha actualizado para tomar opcionalmente en un parámetro que especifica que la base de datos de AD FS para generar un script de implementación de SQL. 
 
 Por ejemplo, para generar el script de implementación para solo la base de datos de artefacto, especifique el `-DatabaseType` parámetro y pase el valor "Artefacto". El elemento opcional `-DatabaseType` parámetro especifica el tipo de base de datos de AD FS y puede establecerse en: Todos (predeterminado), artefacto o configuración. Si no hay ningún `-DatabaseType` parámetro se especifica, el script configurará el artefacto y la configuración de secuencias de comandos.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
El script generado se debe ejecutar en el equipo SQL para crear las bases de datos necesarios y asigne a la cuenta de servicio de AD FS, los permisos de SA de SQL para esas bases de datos.

 2. Crear la base de datos de artefacto con el script de implementación. Copie los scripts de implementación CreateDB.sql y SetPermissions.sql recién generados en la máquina del servidor SQL y ejecutarlas para crear la base de datos local de artefacto. 
 
 3. Modifique el archivo de configuración para agregar la conexión de base de datos de artefacto. 
 Vaya al archivo de configuración del nodo de AD FS y, en la sección "Microsoft.IdentityServer.Service", agregue un punto de entrada a la ArtifactDB recién configurado. 

 >[!NOTE] 
 > artifactStore y connectionString son valores distinguen mayúsculas de minúsculas. Asegúrese de que están configurados correctamente. &lt;artifactStore connectionString = "origen de datos =. \SQLInstance; Integrated Security = True; Initial Catalog = AdfsArtifactStore" /&gt; 
>
>Use un valor de origen de datos que coincida con la conexión de sql.



 4. Reinicie el servicio de AD FS para que los cambios surtan efecto. 
 
 >[!NOTE] 
 > No se recomienda utilizar la replicación de SQL o la sincronización entre las bases de datos de artefacto. La recomendación es establecer una base de datos de artefacto por centro de datos. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Entre los centros de datos de conmutación por error y recuperación  
Se recomienda crear bases de datos de artefacto de conmutación por error en el mismo centro de datos como la base de datos maestra del artefacto. Si se produce una conmutación por error, no habrá ningún aumento en la latencia. No se recomienda las bases de datos de artefacto de conmutación por error entre centros de datos. La siguiente detalla cómo las llamadas para OAuth, SAML, ESL y token de reproducción la función de detección con varias bases de datos de artefacto. 
 - **OAuth y SAML** 

   Para las solicitudes de artefacto de OAuth y SAML, el nodo creará el artefacto en el artefacto de la base de datos presentes en el archivo de configuración. Si el archivo de configuración no tiene una conexión de base de datos de artefacto, usará el artefacto comunes de base de datos. Cuando la siguiente solicitud para capturar el artefacto que se va a otro nodo, el otro nodo hará que la API de rest al nodo 1 para capturar el artefacto de la base de datos del artefacto. Esto es necesario, como nodos diferentes podrían tener bases de datos de artefacto diferente y no sabe acerca de los nodos. Si el nodo 1 está inactivo, se producirá un error en la resolución de artefactos. Debido a este diseño, no es necesario replicar el artefacto de la base de datos en diferentes centros de datos. Si un centro de datos completo está inactivo, más probable es que el nodo que se creó el artefacto es también hacia abajo, lo que significa que ese artefacto ya no se puede resolver.  

 - **Bloqueo de extranet** 

    La base de datos de artefacto al que hace referencia en el archivo de configuración se usará para los datos de bloqueo de Extranet. Sin embargo, para la característica ESL, AD FS elige a un patrón que escribe los datos en la base de datos del artefacto. Todos los nodos de llamar a una API de REST en el nodo principal para obtener y establecer la información más reciente acerca de cada usuario. Si se usan varios artefactos DB, el administrador debe seleccionar un nodo maestro para cada artefacto DB o el centro de datos. 

    Para seleccionar un nodo para el maestro ESL, vaya al archivo de configuración del nodo ADFS y, en la sección "Microsoft.IdentityServer.Service", agregue lo siguiente:       
    
    En el maestro de agregue la siguiente entrada. Tenga en cuenta que todas las tres claves distinguen mayúsculas de minúsculas. 

    &lt;useractivityfarmrole masterFQDN = [nombre de dominio completo de la principal seleccionada] isMaster = "true" /&gt;
    
    En los demás nodos agregue la siguiente entrada:

   &lt;useractivityfarmrole masterFQDN = [nombre de dominio completo de la principal seleccionada] isMaster = "false" /&gt;
 
    >[!NOTE] 
    >Dado que varias bases de datos de artefacto no sincronizar los datos, los valores de ESL no se sincronizarán entre bases de datos de artefacto.
    Un usuario puede encontrarse con un centro de datos diferente para una solicitud, lo que depende del número de bases de datos de artefacto, ExtranetLockoutThreshold el ExtranetLockoutThreshold * número de bases de artefacto. 
 
  - **Detección de reproducción de tokens** 
    
    Datos de detección de reproducción de tokens siempre se llama desde la base de datos central de artefacto. AD FS guarda el token de la confianza del proveedor de notificaciones, lo que garantiza que no se puede reproducir el mismo token. Si un atacante intenta reproducir el mismo token, AD FS comprueba si el token existe en la base de datos de artefacto. Si el token está presente, se rechazará la solicitud. Se utiliza la base de datos central de artefacto para la seguridad, ya que no se replican los datos de la base de datos de artefacto, un atacante podría enviar la solicitud a otro centro de datos y un token de reproducción. Crear copias adicionales de solo lectura de la ArtifactDB no impedirá latencia entre centros de datos en este escenario, tal como se usa solo la base de datos artefacto central.    
 
 