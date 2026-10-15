{{- /* A page's source as clean Markdown with absolute links, for LLMs and readers. */ -}}
{{- $base := strings.TrimSuffix "/" site.BaseURL -}}
{{- $body := .RenderShortcodes -}}
{{- $body = replaceRE `\]\(/` (printf "](%s/" $base) $body -}}
{{- $body = replaceRE `(src|href)="/` (printf `$1="%s/` $base) $body -}}
# {{ .Title }}

{{ with .Description }}> {{ . }}

{{ end -}}
{{- if eq .Section "posts" -}}
- Author: {{ site.Params.author.name }} ({{ site.Params.author.url }})
- Published: {{ .Date.Format "2006-01-02" }}{{ if gt (.Lastmod.Sub .Date).Hours 24 }} (updated {{ .Lastmod.Format "2006-01-02" }}){{ end }}
- URL: {{ .Permalink }}
{{ with .Params.tags }}- Tags: {{ delimit . ", " }}
{{ end }}
{{ end -}}
{{ $body | strings.TrimSpace }}
