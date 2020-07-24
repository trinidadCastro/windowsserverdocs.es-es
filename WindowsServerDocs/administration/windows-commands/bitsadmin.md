---
title: bitsadmin
description: Artículo de referencia para el comando bitsadmin, que es una herramienta de línea de comandos que se usa para crear, descargar o cargar trabajos y supervisar su progreso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51a64b059dd9d07dd6bd0ecccb1cd99382bdfaa5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955377"
---
# <a name="bitsadmin"></a>bitsadmin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows 10

Bitsadmin es una herramienta de línea de comandos que se usa para crear, descargar o cargar trabajos, y para supervisar su progreso. La herramienta bitsadmin usa modificadores para identificar el trabajo que se va a realizar. Puede llamar a `bitsadmin /?` o `bitsadmin /help` para obtener una lista de modificadores.

La mayoría de los conmutadores requieren un `<job>` parámetro, que se establece en el nombre para mostrar del trabajo, o GUID. El nombre para mostrar de un trabajo no tiene que ser único. Los modificadores **/Create** y **/List** devuelven el GUID de un trabajo.

De forma predeterminada, puede tener acceso a información sobre sus propios trabajos. Para tener acceso a la información de los trabajos de otro usuario, debe tener privilegios de administrador. Si el trabajo se ha creado en un estado con privilegios elevados, debe ejecutar **bitsadmin** desde una ventana con privilegios elevados. de lo contrario, tendrá acceso de solo lectura al trabajo.

Muchos de los modificadores se corresponden con los métodos de las [interfaces bits](/windows/win32/bits/bits-interfaces). Para obtener detalles adicionales que pueden ser pertinentes para el uso de un modificador, vea el método correspondiente.

Use los modificadores siguientes para crear un trabajo, establecer y recuperar las propiedades de un trabajo, y supervisar el estado de un trabajo. Para obtener ejemplos que muestran cómo usar algunos de estos modificadores para realizar tareas, consulte [ejemplos de bitsadmin](bitsadmin-examples.md).

## <a name="available-switches"></a>Modificadores disponibles

- [bitsadmin/AddFile](bitsadmin-addfile.md)
- [bitsadmin/addfileset](bitsadmin-addfileset.md)
- [bitsadmin/addfilewithranges](bitsadmin-addfilewithranges.md)
- [bitsadmin/cache](bitsadmin-cache.md)
- [bitsadmin/cache/Delete](bitsadmin-cache-and-delete.md)
- [bitsadmin/cache/deleteurl](bitsadmin-cache-and-deleteurl.md)
- [bitsadmin/cache/getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
- [bitsadmin/cache/getlimit](bitsadmin-cache-and-getlimit.md)
- [bitsadmin/cache/Help](bitsadmin-cache-and-help.md)
- [bitsadmin/cache/info](bitsadmin-cache-and-info.md)
- [bitsadmin/cache/List](bitsadmin-cache-and-list.md)
- [bitsadmin/cache/setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
- [bitsadmin/cache/setlimit](bitsadmin-cache-and-setlimit.md)
- [bitsadmin/cache/Clear](bitsadmin-cache-clear.md)
- [bitsadmin/CANCEL](bitsadmin-cancel.md)
- [bitsadmin/complete](bitsadmin-complete.md)
- [bitsadmin/Create](bitsadmin-create.md)
- [bitsadmin/examples](bitsadmin-examples.md)
- [bitsadmin/getaclflags](bitsadmin-getaclflags.md)
- [bitsadmin/getbytestotal](bitsadmin-getbytestotal.md)
- [bitsadmin/getbytestransferred](bitsadmin-getbytestransferred.md)
- [bitsadmin/getclientcertificate](bitsadmin-getclientcertificate.md)
- [bitsadmin/getcompletiontime](bitsadmin-getcompletiontime.md)
- [bitsadmin/GetCreationTime](bitsadmin-getcreationtime.md)
- [bitsadmin/getcustomheaders](bitsadmin-getcustomheaders.md)
- [bitsadmin/getDescription](bitsadmin-getdescription.md)
- [bitsadmin/getDisplayName](bitsadmin-getdisplayname.md)
- [bitsadmin/getError](bitsadmin-geterror.md)
- [bitsadmin/geterrorcount](bitsadmin-geterrorcount.md)
- [bitsadmin/getfilestotal](bitsadmin-getfilestotal.md)
- [bitsadmin/getfilestransferred](bitsadmin-getfilestransferred.md)
- [bitsadmin/gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
- [bitsadmin/gethelpertokensid](bitsadmin-gethelpertokensid.md)
- [bitsadmin/gethttpmethod](bitsadmin-gethttpmethod.md)
- [bitsadmin/getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
- [bitsadmin/getminretrydelay](bitsadmin-getminretrydelay.md)
- [bitsadmin/getmodificationtime](bitsadmin-getmodificationtime.md)
- [bitsadmin/getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
- [bitsadmin/getnotifycmdline](bitsadmin-getnotifycmdline.md)
- [bitsadmin/getnotifyflags](bitsadmin-getnotifyflags.md)
- [bitsadmin/getnotifyinterface](bitsadmin-getnotifyinterface.md)
- [bitsadmin/GetOwner](bitsadmin-getowner.md)
- [bitsadmin/getpeercachingflags](bitsadmin-getpeercachingflags.md)
- [bitsadmin/GetPriority](bitsadmin-getpriority.md)
- [bitsadmin/getproxybypasslist](bitsadmin-getproxybypasslist.md)
- [bitsadmin/getproxylist](bitsadmin-getproxylist.md)
- [bitsadmin/getproxyusage](bitsadmin-getproxyusage.md)
- [bitsadmin/getreplydata](bitsadmin-getreplydata.md)
- [bitsadmin/getreplyfilename](bitsadmin-getreplyfilename.md)
- [bitsadmin/getreplyprogress](bitsadmin-getreplyprogress.md)
- [bitsadmin/getsecurityflags](bitsadmin-getsecurityflags.md)
- [bitsadmin/GetState](bitsadmin-getstate.md)
- [bitsadmin/gettemporaryname](bitsadmin-gettemporaryname.md)
- [bitsadmin/GetType](bitsadmin-gettype.md)
- [bitsadmin/getvalidationstate](bitsadmin-getvalidationstate.md)
- [bitsadmin/Help](bitsadmin-help.md)
- [bitsadmin/info](bitsadmin-info.md)
- [bitsadmin/List](bitsadmin-list.md)
- [bitsadmin/ListFiles](bitsadmin-listfiles.md)
- [bitsadmin/makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
- [bitsadmin/monitor](bitsadmin-monitor.md)
- [bitsadmin/NoWrap](bitsadmin-nowrap.md)
- [bitsadmin/peercaching](bitsadmin-peercaching.md)
- [bitsadmin/peercaching/getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
- [bitsadmin/peercaching/Help](bitsadmin-peercaching-and-help.md)
- [bitsadmin/peercaching/setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
- [bitsadmin/Peers](bitsadmin-peers.md)
- [bitsadmin/Peers/Clear](bitsadmin-peers-and-clear.md)
- [bitsadmin/Peers/Discover](bitsadmin-peers-and-discover.md)
- [bitsadmin/Peers/Help](bitsadmin-peers-and-help.md)
- [bitsadmin/Peers/List](bitsadmin-peers-and-list.md)
- [bitsadmin/rawreturn](bitsadmin-rawreturn.md)
- [bitsadmin/removeclientcertificate](bitsadmin-removeclientcertificate.md)
- [bitsadmin/removecredentials](bitsadmin-removecredentials.md)
- [bitsadmin/replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
- [bitsadmin/RESET](bitsadmin-reset.md)
- [bitsadmin/resume](bitsadmin-resume.md)
- [bitsadmin/setaclflag](bitsadmin-setaclflag.md)
- [bitsadmin/setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
- [bitsadmin/setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
- [bitsadmin/SetCredentials](bitsadmin-setcredentials.md)
- [bitsadmin/setcustomheaders](bitsadmin-setcustomheaders.md)
- [bitsadmin/setDescription](bitsadmin-setdescription.md)
- [bitsadmin/setDisplayName](bitsadmin-setdisplayname.md)
- [bitsadmin/sethelpertoken](bitsadmin-sethelpertoken.md)
- [bitsadmin/sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
- [bitsadmin/sethttpmethod](bitsadmin-sethttpmethod.md)
- [bitsadmin/setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
- [bitsadmin/setminretrydelay](bitsadmin-setminretrydelay.md)
- [bitsadmin/setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
- [bitsadmin/setnotifycmdline](bitsadmin-setnotifycmdline.md)
- [bitsadmin/setnotifyflags](bitsadmin-setnotifyflags.md)
- [bitsadmin/setpeercachingflags](bitsadmin-setpeercachingflags.md)
- [bitsadmin/SetPriority](bitsadmin-setpriority.md)
- [bitsadmin/setproxysettings](bitsadmin-setproxysettings.md)
- [bitsadmin/setreplyfilename](bitsadmin-setreplyfilename.md)
- [bitsadmin/setsecurityflags](bitsadmin-setsecurityflags.md)
- [bitsadmin/setvalidationstate](bitsadmin-setvalidationstate.md)
- [bitsadmin/Suspend](bitsadmin-suspend.md)
- [bitsadmin/TakeOwnerShip](bitsadmin-takeownership.md)
- [bitsadmin/Transfer](bitsadmin-transfer.md)
- [bitsadmin/util](bitsadmin-util.md)
- [bitsadmin/util/enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
- [bitsadmin/UTIL/GETIEPROXY](bitsadmin-util-and-getieproxy.md)
- [bitsadmin/util/Help](bitsadmin-util-and-help.md)
- [bitsadmin/util/repairservice](bitsadmin-util-and-repairservice.md)
- [bitsadmin/UTIL/SETIEPROXY](bitsadmin-util-and-setieproxy.md)
- [bitsadmin/util/version](bitsadmin-util-and-version.md)
- [bitsadmin/Wrap](bitsadmin-wrap.md)
