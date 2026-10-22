# {{ .Title }}

{{ trim .RawContent " \n" }}
{{ $upcoming := where .RegularPages "Date" ">=" now -}}
{{ $past := where .RegularPages "Date" "<" now -}}
{{ with $upcoming.Reverse }}
## 開催予定

{{ range . }}{{ partial "event-item.md" . }}{{ end -}}
{{ end -}}
{{ with $past }}
## 過去のイベント

{{ range . }}{{ partial "event-item.md" . }}{{ end -}}
{{ end -}}
{{ if not .RegularPages }}
イベントはまだありません。
{{ end -}}
