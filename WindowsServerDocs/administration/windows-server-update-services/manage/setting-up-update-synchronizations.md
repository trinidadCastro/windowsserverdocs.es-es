---
title: Configuración de sincronizaciones de actualización
description: 'Tema de Windows Server Update Service (WSUS): cómo instalar y configurar las sincronizaciones de actualización'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fdfaaf1af2b74fe15530095700005a422b64986
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719625"
---
# <a name="setting-up-update-synchronizations"></a>Configuración de sincronizaciones de actualización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Durante la sincronización, un servidor WSUS descarga actualizaciones (actualización de metadatos y archivos) desde un origen de actualización. También se descarga nuevas clasificaciones de producto y categorías, si existe. Cuando el servidor WSUS sincroniza por primera vez, se descargarán todas las actualizaciones que especificó al configurar las opciones de sincronización. Después de la primera sincronización, el servidor WSUS descarga solo las actualizaciones desde el origen de la actualización, así como las revisiones en los metadatos de las actualizaciones existentes y la caducidad para las actualizaciones.

La primera vez que un servidor WSUS descarga actualizaciones puede tardar mucho tiempo. Si está configurando varios servidores WSUS, puede acelerar el proceso hasta cierto punto mediante la descarga todas las actualizaciones en un servidor WSUS y, a continuación, copia las actualizaciones en los directorios de contenido de los demás servidores WSUS.

Puede copiar el contenido del directorio de contenido de un servidor WSUS a otro. La ubicación del directorio de contenido se especifica al ejecutar el procedimiento de instalación de WSUS post. Puede usar la herramienta wsusutil.exe para exportar metadatos de actualización de un servidor WSUS en un archivo. A continuación, puede importar ese archivo en otros servidores WSUS.

## <a name="setting-up-update-synchronizations"></a>Configuración de sincronizaciones de actualización
El **opciones** página es el punto de acceso central en la consola de administración de WSUS para personalizar cómo el servidor WSUS sincroniza las actualizaciones. Puede especificar qué actualizaciones se sincronizan automáticamente, donde el servidor obtiene actualizaciones, configuración de conexión y la programación de sincronización. También puede usar el Asistente para configuración de la **opciones** página para configurar o volver a configurar el servidor WSUS en cualquier momento.

### <a name="synchronizing-update-by-product-and-classification"></a>Sincronización de actualizaciones por producto y clasificación
Un servidor WSUS descarga actualizaciones en función de los productos o familias de productos (por ejemplo, Windows o Windows Server 2008 Datacenter edition) y clasificaciones (por ejemplo, actualizaciones críticas o las actualizaciones de seguridad) que especifique. en la primera sincronización, el servidor WSUS descarga todas las actualizaciones disponibles en las categorías que haya especificado. En las sincronizaciones posteriores, el servidor descargas sólo las actualizaciones más recientes de WSUS (o los cambios realizados en las actualizaciones ya están disponibles en el servidor WSUS) para las categorías ha especificado.

Puede especificar la actualización de productos y clasificaciones en el **opciones** página **productos y clasificaciones**. Los productos se muestran en una jerarquía, agrupada por familia de productos. Si selecciona Windows, selecciona automáticamente todos los productos que se encuentra en esa jerarquía de productos. Si activa la casilla de verificación principal seleccione todos los elementos debajo de él, así como todas las versiones futuras. Seleccione las casillas de verificación secundario no seleccionará las casillas de verificación principal. El valor predeterminado de productos es todos los productos de Windows y la configuración predeterminada para las clasificaciones es crítica y actualizaciones de seguridad.

Si un servidor WSUS se está ejecutando en modo de réplica, no podrá realizar esta tarea. Para obtener más información acerca del modo de réplica, vea [el modo de réplica de WSUS ejecutando](running-wsus-replica-mode.md), y [paso 1: Preparar la implementación de WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Para especificar la actualización de productos y clasificaciones para la sincronización

1.  En la consola de administración de WSUS, haga clic en el **opciones** nodo.

2.  Haga clic en **productos y clasificaciones**y, a continuación, haga clic en el **productos** ficha.

3.  Active las casillas de los productos y familias de productos que desea actualizar con WSUS y, a continuación, haga clic en **Aceptar**.

4.  En el **clasificaciones** ficha, active las casillas de las clasificaciones de actualización que desee sincronizar y, a continuación, haga clic en su servidor WSUS **Aceptar**.

> [!NOTE]
> Puede quitar productos o clasificaciones de la misma manera. El servidor WSUS dejará de sincronizar nuevas actualizaciones para los productos que haya borrado. Sin embargo, las actualizaciones que se han sincronizado para esos productos antes de borrar ellos permanecerán en el servidor WSUS y se mostrarán como disponibles.
> 
> Para quitar estos productos, rechazar la actualización, como se documenta en [actualizaciones operaciones](updates-operations.md)y, a continuación, use el [limpieza del servidor el Asistente para](the-server-cleanup-wizard.md) para quitarlos.

### <a name="synchronizing-updates-by-language"></a>Sincronizar las actualizaciones por idioma
El servidor WSUS descarga actualizaciones basadas en los idiomas que especifique. Puede sincronizar las actualizaciones en todos los idiomas que están disponibles, o puede especificar un subconjunto de idiomas. Si tiene una jerarquía de servidores WSUS, y tiene que descargar actualizaciones en idiomas diferentes, asegúrese de que ha especificado todos los idiomas necesarios en el servidor que precede. Puede especificar un subconjunto de los idiomas especificados en el servidor que precede en un servidor de nivel inferior.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Sincronizar las actualizaciones desde el catálogo de Microsoft Update
Para obtener más información acerca de cómo sincronizar las actualizaciones desde el sitio del catálogo de Microsoft Update, consulte: [WSUS y el sitio del catálogo](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Configuración del servidor Proxy
Puede configurar el servidor WSUS para utilizar un servidor proxy durante la sincronización con un servidor que precede o Microsoft Update. Esta configuración se aplicará solo cuando el servidor WSUS ejecuta las sincronizaciones. De forma predeterminada, el servidor WSUS intentará conectarse directamente al servidor que precede o Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Para especificar un servidor proxy para la sincronización

1.  En la consola de administración de WSUS, haga clic en **opciones**y, a continuación, haga clic en **Actualizar origen y servidor Proxy**.

2.  En el **servidor Proxy** ficha, seleccione el **utilizar un servidor proxy al sincronizar** casilla y, a continuación, escriba el nombre del servidor y número de puerto del servidor proxy.

    > [!NOTE]
    > Configurar WSUS con el mismo número de puerto que el servidor proxy está configurado para usar.

    -   Si desea conectarse al servidor proxy con credenciales de usuario específicas, seleccione el **usar las credenciales de usuario para conectarse al servidor proxy** casilla de verificación y, a continuación, escriba el nombre de usuario, dominio y contraseña del usuario en los cuadros correspondientes .

    -   Si desea habilitar la autenticación básica para el usuario se conecta al servidor proxy, seleccione el **permitir autenticación básica (la contraseña se envía en texto no cifrado)** casilla de verificación.

3.  Haga clic en **Aceptar**.

    > [!NOTE]
    > Dado que WSUS inicia todo su tráfico de red, no hay ninguna necesidad de configurar el Firewall de Windows en un servidor WSUS conectado directamente a Microsoft update.

## <a name="configuring-the-update-source"></a>Configurar el origen de actualización
El origen de actualización es la ubicación desde la que el servidor WSUS obtiene sus actualizaciones y actualizar los metadatos. Puede especificar que el origen de actualización debe ser Microsoft Update u otro servidor WSUS (el servidor WSUS que actúa como el origen de actualización es el servidor de nivel superior y el servidor es el servidor de nivel inferior).

Las opciones para personalizar cómo el servidor WSUS sincroniza con el origen de actualización incluyen lo siguiente:

-   Puede especificar un puerto personalizado para la sincronización. Para obtener información acerca de cómo configurar puertos, consulte [paso 3: Configurar WSUS](../deploy/2-configure-wsus.md) en la Guía de implementación de WSUS.

-   Puede usar capas de sockets seguros (SSL) en la sincronización segura de información sobre la actualización entre servidores WSUS. Para obtener más información sobre el uso de SSL, consulte la sección "3.5. Proteger WSUS con el protocolo de capa de Sockets seguros"de [paso 3: Configurar WSUS](../deploy/2-configure-wsus.md) en la Guía de implementación de WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Sincronización de forma manual o automática
Puede sincronizar manualmente el servidor WSUS o especifique una hora para lo que se sincronice automáticamente.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Para sincronizar manualmente el servidor WSUS

1.  En la consola de administración de WSUS, haga clic en **opciones**y, a continuación, haga clic en **programación de sincronización**.

2.  Haga clic en **sincronizar manualmente**y, a continuación, haga clic en **Aceptar**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Para configurar una programación de sincronización automática

1.  En la consola de administración de WSUS, haga clic en **opciones**, a continuación, haga clic en **programación de sincronización**.

2.  Haga clic en **sincronizar automáticamente**.

3.  Para **primera sincronización**, seleccione el tiempo que desee comenzar cada día de la sincronización.

4.  para **sincronizaciones por día**, seleccione el número de sincronizaciones que desea hacer cada día. Por ejemplo, si desea cuatro sincronizaciones partida día a las 3:00 A.M., las sincronizaciones se producirán a las 3:00 A.M., 9:00 A.M., 3:00 P.M. y 9:00 P.M. cada día. (Tenga en cuenta que un desplazamiento de tiempo aleatorio se agregarán a la hora de sincronización programada con el fin de espacio de las conexiones del servidor a Microsoft Update.)

5.  Haga clic en **Aceptar**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Para sincronizar inmediatamente el servidor WSUS

1.  En la consola de administración de WSUS, seleccione el nodo de servidor superior.

2.  En el **Introducción** panel, en **estado de sincronización**, haga clic en **sincronizar ahora**.

> [!NOTE]
> La sincronización se inicia el servidor de nivel inferior.
