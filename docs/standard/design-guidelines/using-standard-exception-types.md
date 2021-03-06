---
title: Verwenden von Standardausnahmetypen
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- throwing exceptions, standard types
- catching exceptions
- exceptions, catching
- exceptions, throwing
ms.assetid: ab22ce03-78f9-4dca-8824-c7ed3bdccc27
caps.latest.revision: "17"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 5098db5131c2e47c0b73efaac51477ef3b107761
ms.sourcegitcommit: e7f04439d78909229506b56935a1105a4149ff3d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2017
---
# <a name="using-standard-exception-types"></a>Verwenden von Standardausnahmetypen
Dieser Abschnitt beschreibt die Standardausnahmen, die durch das Framework und die Details zu ihrer Verwendung bereitgestellt. Die Liste ist nicht vollständig. Finden Sie in der .NET Framework-Referenzdokumentation für die Verwendung von anderen Framework-Ausnahmetypen.  
  
## <a name="exception-and-systemexception"></a>Diese Ausnahme und SystemException  
 **X nicht** auslösen <xref:System.Exception?displayProperty=nameWithType> oder <xref:System.SystemException?displayProperty=nameWithType>.  
  
 **X nicht** catch `System.Exception` oder `System.SystemException` in Frameworkcode, es sei denn, Sie erneut auslösen möchten.  
  
 **X vermeiden** abfangen `System.Exception` oder `System.SystemException`, außer bei Ausnahmehandler der obersten Ebene.  
  
## <a name="applicationexception"></a>ApplicationException  
 **X nicht** lösen oder eine Ableitung von <xref:System.ApplicationException>.  
  
## <a name="invalidoperationexception"></a>InvalidOperationException  
 **✓ FÜHREN** Auslösen einer <xref:System.InvalidOperationException> , wenn das Objekt in einem unzulässigen Zustand ist.  
  
## <a name="argumentexception-argumentnullexception-and-argumentoutofrangeexception"></a>ArgumentException, ArgumentNullException und ArgumentOutOfRangeException  
 **Führen Sie ✓** auslösen <xref:System.ArgumentException> oder einem seiner Untertypen, wenn ungültige Argumente auf einen Member übergeben werden. Bevorzugen Sie am weitesten abgeleiteten Ausnahmetyp, falls zutreffend.  
  
 **✓ FÜHREN** legen Sie die `ParamName` Eigenschaft, die beim Auslösen einer die Unterklasse von `ArgumentException`.  
  
 Diese Eigenschaft stellt den Namen des Parameters, der die Ausnahme ausgelöst wird, verursacht hat. Beachten Sie, dass die Eigenschaft mit einer der Konstruktorüberladungen festgelegt werden kann.  
  
 **Führen Sie ✓** verwenden `value` für den Namen des Parameters impliziter Wert der Eigenschaftensetter.  
  
## <a name="nullreferenceexception-indexoutofrangeexception-and-accessviolationexception"></a>NullReferenceException IndexOutOfRangeException und AccessViolationException  
 **X nicht** ermöglichen öffentlich aufrufbare APIs explizit oder implizit auslöst <xref:System.NullReferenceException>, <xref:System.AccessViolationException>, oder <xref:System.IndexOutOfRangeException>. Diese Ausnahmen sind reserviert und das Ausführungsmodul ausgelöst und in den meisten Fällen einen Fehler anzugeben.  
  
 Führen Sie Argument wird überprüft, um zu vermeiden, diese Ausnahmen auslösen. Lösen Sie diese Ausnahmen stellt Details zur Implementierung der Methode, die mit der Zeit ändern können.  
  
## <a name="stackoverflowexception"></a>StackOverflowException  
 **X nicht** explizit auslösen <xref:System.StackOverflowException>. Die Ausnahme sollte nur von der CLR explizit ausgelöst werden.  
  
 **X nicht** catch `StackOverflowException`.  
  
 Es ist nahezu unmöglich, verwalteten Code schreiben, der bei beliebigen Stapelüberläufe konsistent bleibt. Über Prüfpunkte verschoben werden, klar definierte stellen Stapelüberläufe statt aus beliebigen Stapelüberläufe Rückgängigmachen bleiben der nicht verwalteten Teile der CLR konsistent.  
  
## <a name="outofmemoryexception"></a>OutOfMemoryException  
 **X nicht** explizit auslösen <xref:System.OutOfMemoryException>. Diese Ausnahme wird nur von der CLR-Infrastruktur ausgelöst werden.  
  
## <a name="comexception-sehexception-and-executionengineexception"></a>ComException und SEHException ExecutionEngineException  
 **X nicht** explizit auslösen <xref:System.Runtime.InteropServices.COMException>, <xref:System.ExecutionEngineException>, und <xref:System.Runtime.InteropServices.SEHException>. Diese Ausnahmen werden nur von der CLR-Infrastruktur ausgelöst werden.  
  
 *Teilen © 2005, 2009 Microsoft Corporation. Alle Rechte vorbehalten.*  
  
 *Nachdruck mit Genehmigung von Pearson-Education, Inc. aus [Framework-Entwurfsrichtlinien: Konventionen, Idiome und Muster für Wiederverwendbaren .NET-Bibliotheken, 2nd Edition](http://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) Krzysztof Cwalina und Brad Abrams veröffentlicht 22 Oktober 2008 durch Addison Wesley Professional als Teil der Microsoft Windows-Entwicklung Reihe.*  
  
## <a name="see-also"></a>Siehe auch  
 [Frameworkentwurfsrichtlinien](../../../docs/standard/design-guidelines/index.md)  
 [Entwurfsrichtlinien für Ausnahmen](../../../docs/standard/design-guidelines/exceptions.md)
