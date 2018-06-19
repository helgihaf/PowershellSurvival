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
