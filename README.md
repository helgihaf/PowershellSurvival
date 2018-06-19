# PowershellSurvival

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

## Modules
```powershell
Import-Module .\MyModule.psm1
# use the module..., then:
Remove-Module MyModule
```
