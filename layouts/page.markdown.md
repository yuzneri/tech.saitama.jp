{{- /* AI 向けの Markdown 版。front matter は .RawContent に含まれないため、イベント情報は HTML 版と同じ項目を書き出す */ -}}
# {{ .Title }}
{{ if eq .Section "events" }}
{{- with .Params.description }}
> {{ . }}
{{ end }}
- 日時: {{ partial "event-when.html" . }}
{{- with .Params.venue }}
- 会場: {{ . }}
{{- end }}
{{- with .Params.address }}
- 住所: {{ . }}
{{- end }}
{{- with .Params.fee }}
- 参加費: {{ . }}
{{- end }}
{{- with .Params.capacity }}
- 定員: {{ . }}名
{{- end }}
{{- with .Params.hashtag }}
- ハッシュタグ: #{{ . }}
{{- end }}
{{- with .Params.connpass }}
- 申し込み: {{ . }}
{{- end }}
{{ end }}
{{ trim .RawContent " \n" }}
{{ with .Params.connpass }}
参加枠・タイムテーブルなどの詳細と申し込みは [connpassイベントページ]({{ . }}) をご覧ください。
{{ end -}}
