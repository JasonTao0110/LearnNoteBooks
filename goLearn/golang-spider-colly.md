
pkg:
```markdown
"github.com/gocolly/colly"


go get "..."
```


main.go
```golang
package main

import (
	"fmt"
	"github.com/gocolly/colly"
	"github.com/gocolly/colly/extensions"
	_ "github.com/gocolly/colly/extensions"
	"time"
)

func main() {
	url := "https://v.qq.com/"
	client := colly.NewCollector(
		colly.UserAgent("Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36"),
		//colly.MaxDepth(1),
		//colly.Debugger(&debug.LogDebugger{}),  //"github.com/gocolly/colly/debug"
	)
	client2 := client.Clone()

	// 异步
	client2.Async = true

	// 限速
	err := client2.Limit(&colly.LimitRule{
		DomainRegexp: "",
		DomainGlob:   "*v.qq.com/x/cover/*",
		Delay:        10 * time.Second,
		RandomDelay:  0,
		Parallelism:  1,
	})
	if err != nil {
		return
	}

	// 采集器  节目链接
	client.OnHTML("div[class='mod_column_bd']", func(e *colly.HTMLElement) {
		e.ForEach("a", func(i int, item *colly.HTMLElement) {
			href := item.Attr("href")
			ctx := colly.NewContext()
			ctx.Put("href", href)
			err := client2.Request("GET", href, nil, ctx, nil)
			if err != nil {
				return 
			}
			fmt.Println(href)

		})

	})

	//设置随机useragent
	extensions.RandomUserAgent(client)
	//设置登录cookie
	//err2 := client.SetCookies(url, []*http.Cookie{
	//	{
	//		Name:     "remember_user_token",
	//		Value:    "wNDUxOV0sIiQyYSQxMSRwdkhqWVhHYmxXaDJ6dEU3NzJwbmsuIiwiMTU",
	//		Path:     "/",
	//		Domain:   ".jianshu.com",
	//		Secure:   true,
	//		HttpOnly: true,
	//	},
	//})
	//if err2 != nil {
	//	return
	//}

	// 采集器2 节目详情
	client2.OnHTML("div[class='mod_row_box']", func(e *colly.HTMLElement) {
		title := e.ChildText("h2[class='title']")
		actor := e.ChildText("div[class='director']")
		description := e.ChildText("p[class='summary']")
		fmt.Println(title)
		fmt.Println(actor)
		fmt.Println(description)
		fmt.Println()

	})


	client2.OnRequest(func(r *colly.Request) {
		fmt.Println("c2爬取页面：", r.URL)
	})

	client.OnRequest(func(r *colly.Request) {
		fmt.Println("c1爬取页面：", r.URL)

	})
	client.OnError(func(r *colly.Response, err error) {
		fmt.Println("Request URL:", r.Request.URL, "failed with response:", r, "\nError:", err)
	})

	if client.Visit(url) != nil {
		fmt.Println(err.Error())
	}

	client2.Wait()

}

```

生成go.mod
```shell
go mod init spider-test
go mod tidy
```

go.mod
```mod
module spider-test

go 1.16

require (
	github.com/PuerkitoBio/goquery v1.7.1 // indirect
	github.com/antchfx/htmlquery v1.2.3 // indirect
	github.com/antchfx/xmlquery v1.3.6 // indirect
	github.com/gobwas/glob v0.2.3 // indirect
	github.com/gocolly/colly v1.2.0
	github.com/kennygrant/sanitize v1.2.4 // indirect
	github.com/saintfish/chardet v0.0.0-20120816061221-3af4cd4741ca // indirect
	github.com/temoto/robotstxt v1.1.2 // indirect
	golang.org/x/net v0.0.0-20210813160813-60bc85c4be6d // indirect
	google.golang.org/appengine v1.6.7 // indirect
)

```