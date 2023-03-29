# blog-hugo
My blog but [hugo](https://gohugo.io/getting-started/quick-start/) style
Hosted via [Netlify](https://app.netlify.com/)

For drafts
`hugo server --buildDrafts`

`git submodule update --init`

I changed this:
`{{ $url := .URL }}`
to
`{{ $url := .Permalink }}`

I changed this:
`{{ .Hugo.Generator }}`
to
`{{ hugo.Generator }}`
