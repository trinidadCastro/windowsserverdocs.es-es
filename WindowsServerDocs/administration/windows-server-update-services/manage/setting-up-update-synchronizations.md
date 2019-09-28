---
title: Configuración de sincronizaciones de actualización
description: 'Tema de Windows Server Update Service (WSUS): Cómo configurar y configurar sincronizaciones de actualizaciones'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4559016388f9b0d765c8e4d76f76fa7ef0a7f0f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361601"
---
# <a name="setting-up-update-synchronizations"></a>Configuración de sincronizaciones de actualización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Durante la sincronización, un servidor WSUS descarga actualizaciones (archivos y metadatos de actualización) desde un origen de actualización. También se descargan las clasificaciones y categorías de producto nuevas, si las hay. Cuando el servidor WSUS se sincroniza por primera vez, descargará todas las actualizaciones que especificó al configurar las opciones de sincronización. Después de la primera sincronización, el servidor WSUS descarga solo las actualizaciones del origen de la actualización, así como las revisiones en los metadatos de las actualizaciones existentes y la expiración de las actualizaciones.

La primera vez que un servidor WSUS descarga actualizaciones puede tardar mucho tiempo. Si está configurando varios servidores WSUS, puede acelerar el proceso hasta cierto punto descargando todas las actualizaciones en un servidor WSUS y, a continuación, copiando las actualizaciones en los directorios de contenido de los demás servidores WSUS.

Puede copiar el contenido de un directorio de contenido del servidor WSUS en otro. La ubicación del directorio de contenido se especifica cuando se ejecuta el procedimiento posterior a la instalación de WSUS. Puede usar la herramienta WSUSUTIL. exe para exportar metadatos de actualización de un servidor WSUS a un archivo. Después, puede importar ese archivo en otros servidores WSUS.

## <a name="setting-up-update-synchronizations"></a>Configuración de sincronizaciones de actualización
La página **Opciones** es el punto de acceso central en la consola de administración de WSUS para personalizar el modo en que el servidor WSUS sincroniza las actualizaciones. Puede especificar las actualizaciones que se sincronizan automáticamente, el lugar en el que el servidor obtiene las actualizaciones, la configuración de la conexión y la programación de sincronización. También puede usar el Asistente para configuración de la página **Opciones** para configurar o volver a configurar el servidor WSUS en cualquier momento.

### <a name="synchronizing-update-by-product-and-classification"></a>Sincronizando la actualización por producto y clasificación
Un servidor WSUS descarga actualizaciones basadas en productos o familias de productos (por ejemplo, Windows, Windows Server 2008, Datacenter Edition) y clasificaciones (por ejemplo, actualizaciones críticas o actualizaciones de seguridad) que especifique. en la primera sincronización, el servidor WSUS descarga todas las actualizaciones disponibles en las categorías especificadas. En las sincronizaciones posteriores, el servidor WSUS descarga solo las actualizaciones más recientes (o los cambios en las actualizaciones que ya están disponibles en el servidor WSUS) para las categorías especificadas.

Puede especificar actualizar productos y clasificaciones en la página **Opciones** en **productos y clasificaciones**. Los productos se muestran en una jerarquía, agrupados por familias de productos. Si selecciona Windows, se seleccionan automáticamente todos los productos que se encuentran bajo esa jerarquía de productos. Al activar la casilla primario, se seleccionan todos los elementos que hay debajo de él, así como todas las versiones futuras. Si activa las casillas secundarias, no se activarán las casillas de verificación primarias. La configuración predeterminada para los productos es todos los productos de Windows y la configuración predeterminada para las clasificaciones es crítica y actualizaciones de seguridad.

Si un servidor WSUS se ejecuta en modo de réplica, no podrá realizar esta tarea. Para obtener más información acerca del modo de réplica, vea [ejecutar el modo de réplica de WSUS](running-wsus-replica-mode.md)y @no__t 1Step 1: Preparar la implementación de WSUS @ no__t-0.

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Para especificar productos y clasificaciones de actualización para la sincronización

1.  En la consola de administración de WSUS, haga clic en el nodo **Opciones** .

2.  Haga clic en **productos y clasificaciones**y, a continuación, haga clic en la pestaña **productos** .

3.  Active las casillas de verificación de los productos o familias de productos que desee actualizar con WSUS y, a continuación, haga clic en **Aceptar**.

4.  En la pestaña **clasificaciones** , active las casillas de las clasificaciones de actualizaciones que desea que sincronice el servidor WSUS y, a continuación, haga clic en **Aceptar**.

> [!NOTE]
> Puede quitar productos o clasificaciones de la misma manera. El servidor WSUS dejará de sincronizar las nuevas actualizaciones de los productos que ha desactivado. No obstante, las actualizaciones que se hayan sincronizado para esos productos antes de que se desactiven permanecerán en el servidor WSUS y se mostrarán como disponibles.
> 
> Para quitar estos productos, rechace la actualización, como se documenta en [operaciones de actualización](updates-operations.md)y, a continuación, use el Asistente para [la limpieza del servidor](the-server-cleanup-wizard.md) para quitarlas.

### <a name="synchronizing-updates-by-language"></a>Sincronizar actualizaciones por idioma
El servidor WSUS descarga las actualizaciones en función de los idiomas que especifique. Puede sincronizar las actualizaciones en todos los idiomas en los que están disponibles, o puede especificar un subconjunto de idiomas. Si tiene una jerarquía de servidores WSUS y necesita descargar actualizaciones en distintos idiomas, asegúrese de que ha especificado todos los idiomas necesarios en el servidor que precede en la cadena. En un servidor de nivel inferior, puede especificar un subconjunto de los idiomas especificados en el servidor que precede en la cadena.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Sincronizar actualizaciones desde el catálogo de Microsoft Update
para obtener más información acerca de la sincronización de actualizaciones desde el sitio del catálogo de Microsoft Update, consulte: [WSUS y el sitio del catálogo de](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Configuración del servidor proxy
Puede configurar el servidor WSUS para que use un servidor proxy durante la sincronización con un servidor que precede en la cadena o Microsoft Update. Esta configuración solo se aplicará cuando el servidor WSUS ejecute sincronizaciones. De forma predeterminada, el servidor WSUS intentará conectarse directamente al servidor que precede en la cadena o a Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Para especificar un servidor proxy para la sincronización

1.  En la consola de administración de WSUS, haga clic en **Opciones**y, a continuación, haga clic en **Actualizar origen y servidor proxy**.

2.  En la pestaña **servidor proxy** , active la casilla **usar un servidor proxy al sincronizar** y, a continuación, escriba el nombre del servidor y el número de puerto del servidor proxy.

    > [!NOTE]
    > Configure WSUS con el mismo número de puerto que el servidor proxy está configurado para usar.

    -   Si desea conectarse al servidor proxy con credenciales de usuario específicas, active la casilla **usar credenciales de usuario para conectarse al servidor proxy** y, a continuación, escriba el nombre de usuario, el dominio y la contraseña del usuario en los cuadros correspondientes.

    -   Si desea habilitar la autenticación básica para el usuario que se conecta al servidor proxy, active la casilla **permitir autenticación básica (la contraseña se envía en texto no cifrado)** .

3.  Haga clic en **Aceptar**.

    > [!NOTE]
    > Dado que WSUS inicia todo el tráfico de red, no es necesario configurar Firewall de Windows en un servidor WSUS conectado directamente a Microsoft Update.

## <a name="configuring-the-update-source"></a>Configurar el origen de la actualización
El origen de la actualización es la ubicación desde la que el servidor WSUS obtiene sus actualizaciones y metadatos de actualización. Puede especificar que el origen de actualización debe ser Microsoft Update u otro servidor WSUS (el servidor WSUS que actúa como origen de actualización es el servidor que precede en la cadena y el servidor es el servidor que sigue en la cadena).

Entre las opciones para personalizar el modo en que el servidor WSUS se sincroniza con el origen de actualización se incluyen las siguientes:

-   Puede especificar un puerto personalizado para la sincronización. Para obtener información sobre la configuración de puertos, consulte @no__t 0Step 3: Configure WSUS @ no__t-0 en la guía de implementación de WSUS.

-   Puede usar capas de sockets seguros (SSL) para proteger la sincronización de la información de actualización entre los servidores WSUS. Para obtener más información sobre el uso de SSL, consulte la sección "3,5. Proteger WSUS con el protocolo de Capa de sockets seguros "de [Step 3: Configure WSUS @ no__t-0 en la guía de implementación de WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Sincronizar manual o automáticamente
Puede sincronizar el servidor WSUS manualmente o especificar una hora para que se sincronice automáticamente.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Para sincronizar manualmente el servidor WSUS

1.  En la consola de administración de WSUS, haga clic en **Opciones**y, a continuación, en **programación de sincronización**.

2.  Haga clic en **sincronizar manualmente**y, a continuación, en **Aceptar**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Para configurar una programación de sincronización automática

1.  En la consola de administración de WSUS, haga clic en **Opciones**y, a continuación, en **programación de sincronización**.

2.  Haga clic en **sincronizar automáticamente**.

3.  Para la **primera sincronización**, seleccione la hora a la que desea que se inicie la sincronización cada día.

4.  en el caso de las **sincronizaciones por día**, seleccione el número de sincronizaciones que desea realizar cada día. Por ejemplo, si desea cuatro sincronizaciones al día a partir de las 3:00 A.M., las sincronizaciones se producirán a las 3:00 A.M., 9:00 A.M., 3:00 P.M. y 9:00 P.M. cada día. (Tenga en cuenta que se agregará un ajuste de tiempo aleatorio a la hora de sincronización programada para reducir el espacio de las conexiones de servidor a Microsoft Update).

5.  Haga clic en **Aceptar**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Para sincronizar el servidor WSUS inmediatamente

1.  En la consola de administración de WSUS, seleccione el nodo de servidor superior.

2.  En el panel de **información general** , en **Estado de sincronización**, haga clic en **sincronizar ahora**.

> [!NOTE]
> El servidor de bajada inicia la sincronización.
