{{- /* Markdown 版の一覧に並べるイベント 1 件。リンク先はイベントの Markdown 版 */ -}}
- [{{ .Title }}]({{ (.OutputFormats.Get "markdown").Permalink }}): {{ partial "event-when.html" . }}{{ with .Params.venue }} / {{ . }}{{ end }}
