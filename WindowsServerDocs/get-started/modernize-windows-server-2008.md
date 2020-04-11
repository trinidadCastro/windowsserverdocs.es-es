---
title: Actualizar Windows Server 2008 y Windows Server 2008 R2
description: Windows Server 2008 y 2008 R2 están acercándose al final de servicio. Aprende a actualizar localmente o realojar en Azure.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 3d2c55430a78eaabfe55b764275c6e61fa80368a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826218"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Actualizar Windows Server 2008 y Windows Server 2008 R2

El soporte técnico ampliado para Windows Server 2008 y Windows Server 2008 R2 termina el 14 de enero de 2020. Hay disponibles dos rutas de acceso de modernización: Actualización o migración local mediante rehospedaje en Azure. **Si rehospedas en Azure, puedes migrar las imágenes de servidor existentes de forma gratuita.**

![Diagrama de flujo que describe las rutas de actualización desde Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Actualización local
Si necesitas mantener los servidores localmente y ejecutas Windows Server 2008 o Windows Server 2008 R2, deberás [actualizar a Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) antes de poder [actualizar a Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Al actualizar, seguirás teniendo la opción de migrar a Azure mediante realojo.

Consulta [Actualización desde Windows Server 2008 R2 o Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008), para obtener más información sobre la actualización local.

Si estás ejecutando Windows Server 2003, tendrás que [actualizar a Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Consulta [rutas de actualización para Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) para obtener más información sobre las opciones de actualización local.


## <a name="migrate-to-azure"></a>Migrar a Azure
Puedes migrar los servidores locales de Windows Server 2008 y Windows Server 2008 R2 a Azure, donde puedes seguir ejecutándolos en máquinas virtuales. Con Azure, conservarás la conformidad, estarás más protegido y podrás agregar las innovaciones de la nube a tu trabajo. Las ventajas de migrar a Azure incluyen las siguientes:

- Actualizaciones de seguridad en Azure.
- Obtener tres años más de actualizaciones de seguridad críticas e importantes de Windows Server 2008 R2 o 2008, incluidas sin cargos adicionales. 
- Actualizaciones en Azure sin cargos.
- Adoptar más servicios en la nube conforme estés listo.
- Al migrar SQL Server a Azure Managed Instances o VM, obtendrás tres años más de actualizaciones de seguridad críticas de Windows Server 2008 R2 o 2008, incluidas sin cargos adicionales. 
- Aprovechar las licencias existentes de SQL Server y Windows Server para ahorros de nube únicos en Azure.

[![Iniciar la migración a Azure con una imagen especializada](./media/WS08-image-banner-small.png)](uploading-specialized-WS08-image-to-azure.md)

Para comenzar con la migración, consulta [Actualizar una imagen especializada de Windows Server 2008/2008 R2 a Azure](uploading-specialized-WS08-image-to-azure.md).

Para ayudarte a conocer cómo analizar los recursos de TI existentes e identificar las ventajas de mover aplicaciones y servicios específicos a la nube, o mantener las cargas de trabajo localmente, así como actualizar a la última versión de Windows Server, consulta [Guía de migración de Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).

## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Actualizar SQL Server 2008/2008 R2 en paralelo con los Windows Server

![Logotipo de SQL Server](media/sqlr2.jpg)

Si estás ejecutando SQL Server 2008/2008 R2, puedes actualizar a SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) o [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Recursos adicionales
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)