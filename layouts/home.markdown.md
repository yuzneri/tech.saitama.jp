# {{ site.Title }}

> {{ site.Params.description }}

{{ trim .RawContent " \n" }}
{{ $events := where site.RegularPages "Section" "events" -}}
{{ with (where $events "Date" ">=" now).Reverse }}
## 開催予定のイベント

{{ range . }}{{ partial "event-item.md" . }}{{ end -}}
{{ end -}}
