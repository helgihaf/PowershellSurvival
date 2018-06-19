# PowershellSurvival

## XML

### Load an XML document
```powershell
[xml]$doc = Get-Content c:\temp\mystuff.nuspec
```

### Get element value
```powershell
$id = $doc.package.metadata.id
```

### Add an element with text value
```powershell
$el = $doc.CreateElement('releaseNotes', $doc.package.metadata.NamespaceUri)
$el.InnerXml = 'Fixed some bugs.'
$doc.package.metadata.AppendChild($el) | Out-Null   # Pipe to null to avoid dumping large output to console
```
