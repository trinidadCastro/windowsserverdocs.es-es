---
title: bitsadmin
description: Tema de los comandos de Windows para **bitsadmin** -bitsadmin es una herramienta de línea de comandos que puede usar para crear, descargar, o cargar los trabajos y supervisar sus progresos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da0f05ec716cffb7d7532ebac50a091729a6bb18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821076"
---
# <a name="bitsadmin"></a>bitsadmin

> **Se aplica a**: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin es una herramienta de línea de comandos que puede usar para crear la descarga o cargar los trabajos y supervisar sus progresos. La herramienta bitsadmin usa conmutadores para identificar el trabajo que realizar.  Puede llamar a `bitsadmin /?` o `bitsadmin /HELP` para obtener una lista de conmutadores.

La mayoría de los conmutadores requiere un \<trabajo\> parámetro que se establece en el nombre para mostrar del trabajo o GUID. Tenga en cuenta el nombre para mostrar de un trabajo no sea único. El **/ crear** y **/lista** conmutadores devuelven el GUID de un trabajo.

De forma predeterminada, puede tener acceso a información acerca de sus propios trabajos. Para obtener acceso a información de los trabajos de otro usuario, debe tener privilegios de administrador. Si se creó el trabajo en un estado con privilegios elevados, a continuación, debe ejecutar bitsadmin desde una ventana con privilegios elevados; en caso contrario, tendrá acceso de solo lectura para el trabajo.

Muchos de los modificadores correspondan a métodos en el [interfaces de BITS](/windows/desktop/bits/bits-interfaces). Para obtener detalles adicionales que pueden ser pertinentes para usar un modificador, vea el método correspondiente.

Utilice los modificadores siguientes para crear un trabajo, establecer y recuperar las propiedades de un trabajo y supervisar el estado de un trabajo. Para obtener ejemplos que muestran cómo usar algunos de estos modificadores para realizar tareas, consulte [bitsadmin ejemplos](bitsadmin-examples.md).

## <a name="switches"></a>Modificadores

[addfile Bitsadmin](bitsadmin-addfile.md)  
[Bitsadmin addfileset](bitsadmin-addfileset.md)  
[Bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)  
[caché Bitsadmin](bitsadmin-cache.md)  
[Cancelar Bitsadmin](bitsadmin-cancel.md)  
[Bitsadmin completa](bitsadmin-complete.md)  
[crear Bitsadmin](bitsadmin-create.md)  
[Bitsadmin getaclflags](bitsadmin-getaclflags.md)  
[bitsadmin getbytestotal](bitsadmin-getbytestotal.md)  
[Bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)  
[bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)  
[Bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)  
[getcreationtime Bitsadmin](bitsadmin-getcreationtime.md)  
[Bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)  
[getdescription Bitsadmin](bitsadmin-getdescription.md)  
[bitsadmin getdisplayname](bitsadmin-getdisplayname.md)  
[Bitsadmin geterror](bitsadmin-geterror.md)  
[bitsadmin geterrorcount](bitsadmin-geterrorcount.md)  
[bitsadmin getfilestotal](bitsadmin-getfilestotal.md)  
[Bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)  
[Bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)  
[Bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)  
[Bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
[getmaxdownloadtime bitsadmin](bitsadmin-getmaxdownloadtime.md)  
[bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)  
[Bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)  
[Bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)  
[bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)  
[Bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)  
[Bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)  
[bitsadmin getowner](bitsadmin-getowner.md)  
[bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)  
[getpriority Bitsadmin](bitsadmin-getpriority.md)  
[Bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)  
[Bitsadmin getproxylist](bitsadmin-getproxylist.md)  
[Bitsadmin getproxyusage](bitsadmin-getproxyusage.md)  
[bitsadmin getreplydata](bitsadmin-getreplydata.md)  
[bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)  
[Bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)  
[Bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)  
[bitsadmin getstate](bitsadmin-getstate.md)  
[bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)  
[bitsadmin gettype](bitsadmin-gettype.md)  
[bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)  
[Bitsadmin ayuda](bitsadmin-help.md)  
[Bitsadmin info](bitsadmin-info.md)  
[bitsadmin list](bitsadmin-list.md)  
[bitsadmin listfiles](bitsadmin-listfiles.md)  
[Bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
[bitsadmin monitor](bitsadmin-monitor.md)  
[nowrap Bitsadmin](bitsadmin-nowrap.md)  
[caché del mismo nivel Bitsadmin](bitsadmin-peercaching.md)  
[Bitsadmin elementos del mismo nivel](bitsadmin-peers.md)  
[bitsadmin rawreturn](bitsadmin-rawreturn.md)  
[bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)  
[Bitsadmin removecredentials](bitsadmin-removecredentials.md)  
[bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)  
[bitsadmin reset](bitsadmin-reset.md)  
[reanudar Bitsadmin](bitsadmin-resume.md)  
[Bitsadmin setaclflag](bitsadmin-setaclflag.md)  
[Bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)  
[Bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)  
[Bitsadmin setcredentials](bitsadmin-setcredentials.md)  
[Bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)  
[Bitsadmin setdescription](bitsadmin-setdescription.md)  
[Bitsadmin setdisplayname](bitsadmin-setdisplayname.md)  
[Bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)  
[Bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)  
[Bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
[setmaxdownloadtime bitsadmin](bitsadmin-setmaxdownloadtime.md)  
[bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)  
[Bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)  
[Bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)  
[Bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)  
[Bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)  
[setpriority Bitsadmin](bitsadmin-setpriority.md)  
[Bitsadmin setproxysettings](bitsadmin-setproxysettings.md)  
[Bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)  
[Bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)  
[Bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)  
[suspender Bitsadmin](bitsadmin-suspend.md)  
[Bitsadmin takeownership](bitsadmin-takeownership.md)  
[transferencia Bitsadmin](bitsadmin-transfer.md)  
[bitsadmin util](bitsadmin-util.md)  
[Bitsadmin encapsulado](bitsadmin-wrap.md)  
