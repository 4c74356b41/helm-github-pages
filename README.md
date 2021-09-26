## Basic information

1. releases are handled by github actions workflow located here: `.github/workflows/release.yaml`
2. Istio charts are updated manually:

```powershell
$version = '1.11.2'
Invoke-WebRequest "https://github.com/istio/istio/releases/download/$version/istio-$version-win.zip" -OutFile /tmp/istio.zip
Expand-Archive /tmp/istio.zip -DestinationPath /tmp/istio -Force
New-Item istio -Type Directory
"base", "ingress", "discovery" | ForEach-Object {
    Get-ChildItem -Directory -Recurse -Filter "*$PSItem*" /tmp/istio | Move-Item -Destination ./$PSItem
}
```

you'd also need to alter Charts.yaml and rename `istio-discovery` and `istio-ingress` to `discovery` and `ingress` for the purpose of consistency
