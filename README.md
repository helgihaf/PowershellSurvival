# PowershellSurvival

## Basic Commands

### Find file
```powershell
Get-Childitem -Include *HSG* -File -Recurse -ErrorAction SilentlyContinue | Where-Object { $_.LastWriteTime -ge $FindDate }
```

### Grep - Find File Content
```powershell
Get-ChildItem -Filter *.log -Recurse | Select-String "Contoso"
```

### Remove Directory and Contents
```powershell
Remove-Item $folderPath -Force -Recurse -ErrorAction SilentlyContinue
```

### List Environment Variables
```powershell
gci Env:* | sort name
```

### Set Environment Variable
```powershell
$Env:AWS_REGION='eu-central-1'
```

## XML

### Load an XML document
```powershell
[xml]$doc = Get-Content c:\temp\mystuff.nuspec -Encoding UTF8
```

### Get element value
```powershell
$id = $doc.package.metadata.id
```

### Add an element with text value
```powershell
$el = $doc.CreateElement('releaseNotes', $doc.package.metadata.NamespaceURI)
$el.InnerText = 'Fixed some bugs.'
$doc.package.metadata.AppendChild($el) | Out-Null   # Pipe to null to avoid dumping large output to console
```

### Get default namespace of a document
```powershell
$defaultNamespace = $doc.DocumentElement.NamespaceURI
```

### Select nodes using namespace
```powershell
[System.Xml.XmlNamespaceManager] $nsMgr = New-Object -TypeName System.Xml.XmlNamespaceManager -ArgumentList $xmlDoc.NameTable
$nsMgr.AddNamespace("ns", "http://schemas.microsoft.com/developer/msbuild/2003");
$rpNodes = $xmlDoc.SelectNodes("/ns:Project/ns:PropertyGroup/ns:RestorePackages", $nsMgr);
```

## Modules
```powershell
Import-Module .\MyModule.psm1
# use the module..., then:
Remove-Module MyModule
```
## Pipeline
Given this function, saved in Get-CountX.ps1
```powershell
[CmdletBinding()]
param(
    [Parameter(ValueFromPipeline)]
    [System.IO.FileInfo]$Path
)
begin {
    Write-Host 'Begin'
    $prefix = '-felix-'
}
process {
    Write-Host "$prefix $($Path.FullName.Length)"
}
end {
    Write-Host 'End'
}
```
...you can run this:
```powershell
Get-ChildItem | .\Get-CountX.ps1
```
...and get a result like this:
```
Begin
-felix- 27
-felix- 37
-felix- 42
End
```
Or you can run this:
```powershell
.\Get-CountX.ps1 -Path c:\temp\goofy.txt
```
...and get a result like this:
```
Begin
-felix- 17
End
```
