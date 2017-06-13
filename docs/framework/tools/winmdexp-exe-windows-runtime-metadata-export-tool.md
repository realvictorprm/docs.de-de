---
title: "Winmdexp.exe (Windows Runtime Metadata Export Tool) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "Windows Runtime Metadata Export Tool"
  - "Winmdexp.exe"
ms.assetid: d2ce0683-343d-403e-bb8d-209186f7a19d
caps.latest.revision: 16
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 15
---
# Winmdexp.exe (Windows Runtime Metadata Export Tool)
Das [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Metadatenexport\-Tool \(Winmdexp.exe\) transformiert ein .NET Framework\-Modul in eine Datei, die [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Metadaten enthält.  Obwohl .NET Framework\-Assemblys und [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Metadatendateien dasselbe physische Format verwenden, gibt es Unterschiede bezüglich des Inhalts der Metadatentabellen, d. h. dass .NET Framework\-Assemblys nicht automatisch als [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Komponenten verwendet werden können.  Der Prozess des Konvertierens eines .NET Framework\-Moduls in eine [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Komponente wird als *Exportieren* bezeichnet.  In [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] und [!INCLUDE[net_v451](../../../includes/net-v451-md.md)] enthält die resultierende Windows\-Metadatendatei \(.winmd\) sowohl Metadaten als auch Implementierung.  
  
 Wenn Sie die Vorlage **Komponente für [!INCLUDE[wrt](../../../includes/wrt-md.md)]** verwenden, die unter **Windows Store** für C\# und Visual Basic in [!INCLUDE[vs_dev12](../../../includes/vs-dev12-md.md)] oder [!INCLUDE[vs_dev11_ext](../../../includes/vs-dev11-ext-md.md)] zu finden ist, ist das Compilerziel eine WINMDOBJ\-Datei, und ein darauf folgender Buildschritt ruft "Winmdexp.exe" auf, um die WINMDOBJ\-Datei in eine WINMD\-Datei zu exportieren.  Dies ist die empfohlene Methode, eine Komponente für [!INCLUDE[wrt](../../../includes/wrt-md.md)] zu erstellen.  Verwenden Sie "Winmdexp.exe" direkt, wenn Sie mehr Kontrolle über den Buildvorgang möchten, als Visual Studio bietet.  
  
 Dieses Tool wird automatisch mit Visual Studio installiert.  Zum Ausführen des Tools verwenden Sie die Developer\-Eingabeaufforderung \(oder die Visual Studio\-Eingabeaufforderung in Windows 7\).  Weitere Informationen finden Sie unter [Eingabeaufforderungen](../../../docs/framework/tools/developer-command-prompt-for-vs.md).  
  
 Geben Sie an der Eingabeaufforderung Folgendes ein:  
  
## Syntax  
  
```  
  
winmdexp [options] winmdmodule  
```  
  
#### Parameter  
  
|Argument oder Option|Beschreibung|  
|--------------------------|------------------|  
|`winmdmodule`|Gibt das zu exportierende Modul \(.winmdobj\) an.  Nur ein Modul ist erlaubt.  Um dieses Modul zu erstellen, verwenden Sie die `/target`\-Compileroption mit dem `winmdobj`\-Ziel.  Siehe [\/target:winmdobj \(Eine Zwischendatei für die Windows\-Runtime erstellen\)](../Topic/-target:winmdobj%20\(C%23%20Compiler%20Options\).md) oder [\/target](../Topic/-target%20\(Visual%20Basic\).md) in der MSDN Library.|  
|`/docfile:` `docfile`<br /><br /> `/d:` `docfile`|Gibt die XML\-Dokumentations\-Ausgabedatei an, die "Winmdexp.exe" erzeugt.  In [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] entspricht die Ausgabedatei im Wesentlichen der XML\-Dokumentations\-Eingabedatei.|  
|`/moduledoc:` `docfile`<br /><br /> `/md:` `docfile`|Gibt den Namen der XML\-Dokumentationsdatei an, die der Compiler mit `winmdmodule` produzierte.|  
|`/modulepdb:` `symbolfile`<br /><br /> `/mp:` `symbolfile`|Gibt den Namen der Programmdatenbankdatei \(PDB\-Datei\) an, die Symbole für `winmdmodule` enthält.|  
|`/nowarn:` `warning`|Unterdrückt die angegebene Warnungsanzahl.  Für *warning* stellen Sie nur den numerischen Teil des Fehlercodes bereit, ohne führende Nullen.|  
|`/out:` `file`<br /><br /> `/o:` `file`|Gibt den Namen der Windows\-Metadatendatei \(.winmd\) für die Ausgabe an.|  
|`/pdb:` `symbolfile`<br /><br /> `/p:` `symbolfile`|Gibt den Namen der Programmdatenbank\-Ausgabedatei \(PDB\-Datei\) an, die die Symbole für die exportierte Windows\-Metadatendatei \(.winmd\) enthält.|  
|`/reference:` `winmd`<br /><br /> `/r:` `winmd`|Gibt eine Metadatendatei \(WINMD\-Datei oder Assembly\) an, auf die während des Exports verwiesen wird.  Wenn Sie auf Assemblys in "\\Programme \(x86\)\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5" \("\\Programme\\..." auf 32\-Bit\-Computern\) verweisen, fügen Sie Verweise sowohl auf "System.Runtime.dll" als auch auf "mscorlib.dll" mit ein.|  
|`/utf8output`|Gibt an, dass die Ausgabemeldungen UTF\-8\-Codierung aufweisen sollten.|  
|`/warnaserror+`|Gibt an, dass alle Warnungen als Fehler behandelt werden sollen.|  
|**@** `responsefile`|Gibt eine Antwortdatei \(.rsp\) an, die Optionen enthält \(und optional `winmdmodule`\).  Jede Zeile in `responsefile` sollte ein einzelnes Argument oder eine einzelne Option enthalten.|  
  
## Hinweise  
 "Winmdexp.exe" kann nicht verwendet werden, um eine beliebige .NET Framework\-Assembly in eine WINMD\-Datei zu konvertieren.  Ein Modul ist erforderlich, das mit der `/target:winmdobj`\-Option kompiliert wird, und es gelten zusätzliche Einschränkungen.  Die wichtigste dieser Einschränkungen besteht darin, dass alle Typen, die in der API\-Schnittstelle der Assembly verfügbar gemacht werden, [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Typen sein müssen.  Weitere Informationen finden Sie im Abschnitt "Deklarieren von Typen in Windows\-Runtime\-Komponenten" des Artikels [Erstellen von Windows\-Runtime\-Komponenten in C\# und Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=238313) im Windows Developer Center.  
  
 Wenn Sie eine [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]\-App oder eine Komponente für [!INCLUDE[wrt](../../../includes/wrt-md.md)] mit C\# oder Visual Basic schreiben, bietet .NET Framework Unterstützung für das einfachere Programmieren mit der [!INCLUDE[wrt](../../../includes/wrt-md.md)].  Dies wird im Artikel [.NET Framework\-Unterstützung für Windows Store\-Apps und Windows\-Runtime](../../../docs/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md) erläutert.  Während dieses Vorgangs werden einige häufig verwendete [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Typen .NET Framework\-Typen zugeordnet.  "Winmdexp.exe" kehrt diesen Prozess um und erzeugt eine API\-Schnittstelle, die die entsprechenden [!INCLUDE[wrt](../../../includes/wrt-md.md)]\-Typen verwendet.  So werden beispielsweise Typen, die aus der <xref:System.Collections.Generic.IList%601>\-Schnittstelle erstellt werden, Typen zugeordnet, die aus der [!INCLUDE[wrt](../../../includes/wrt-md.md)] [IVector\<T\>](http://go.microsoft.com/fwlink/p/?LinkId=251132)\-Schnittstelle erstellt werden.  
  
## Siehe auch  
 [.NET Framework\-Unterstützung für Windows Store\-Apps und Windows\-Runtime](../../../docs/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)   
 [Erstellen von Windows\-Runtime\-Komponenten in C\# und Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=238313)   
 [Winmdexp.exe Error Messages](../../../docs/framework/tools/winmdexp-exe-error-messages.md)   
 [Build, Deployment, and Configuration Tools \(.NET Framework\)](http://msdn.microsoft.com/de-de/b8c921be-6012-4181-b8d4-ab15813fc9a7)