![{{ .Get "alt" }}]({{ .Get "src" | absURL }}){{ with .Get "caption" }}

*{{ . }}*{{ end }}
