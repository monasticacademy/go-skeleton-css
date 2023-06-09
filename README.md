# go-skeleton-css

This is a Go package that embeds the Skeleton CSS sources, for straightforward serving in simple projects. Install it with

    go get github.com/monasticacademy/go-skeleton-css

And then use it like this:

    package main

    import (
        _ "embed"
        "fmt"
        "net/http"
        "strings"
        "time"

        skeletoncss "github.com/monasticacedemy/go-skeleton-css"
    )

    var html = `<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Your page title here :)</title>
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="css/fontfaces.css">
        <link rel="stylesheet" href="css/normalize.css">
        <link rel="stylesheet" href="css/skeleton.css">
    </head>
    <body>
        <div class="container">
            <div class="row">
                <div class="one-half column" style="margin-top: 25%">
                    <h4>Basic Page</h4>
                    <p>This index.html page is a <b>placeholder</b> with the CSS, font and favicon. <i>It's just waiting for you to add some content!</i> If you need some help hit up the <a href="http://www.getskeleton.com">Skeleton documentation</a>.</p>
                </div>
            </div>
        </div>
    </body>
    </html>
    `

    func main() {
        http.Handle("/css/", http.StripPrefix("/css/", skeletoncss.FileServer))
        http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
            http.ServeContent(w, r, "index.html", time.Time{}, strings.NewReader(html))
        })

        fmt.Println("listening on :8000")
        http.ListenAndServe(":8000", nil)
    }

Note that the files served by `skeletoncss.FileServer` do not have any directory prefix in their names, so `http.StripPrefix` is critical to use correctly.

This package also statically embeds and serves the Raleway font, so websites served locally will work even when there is no internet connection.
