__TL;DR:__ If you make `www.example.com` the canonical domain for your site then you should serve redirects when someone hits the naked `example.com` domain. If `www` is canonical then bookmarks and searches will always find the correct URL so ideally you'll only need to serve redirects when people type the naked address manually. As such this app should be able to handle the redirects for dozens of domains for all but the biggest sites, saving you the trouble of coding up the redirects on a per-app basis.

### What

This is a Heroku "app" that will happily redirect "naked" domains to their `www` equivalent. It takes advantage of the fact that the [default Heroku PHP buildpack](https://github.com/heroku/heroku-buildpack-php) uses Apache and [supports .htaccess files with mod_rewrite rules](http://kennethreitz.com/static-sites-on-heroku-cedar.html).

### Why

This app is especially useful with S3, which can host websites these days (with a default index.html file) but can't host "naked" domains, only CNAMEs. Likewise, Heroku is happy to have your subdomains pointed at it with CNAME but [gets all wriggly](https://devcenter.heroku.com/articles/avoiding-naked-domains-dns-arecords) if you use A records with IP addresses. In most web servers/frameworks it is trivial to redirect if the host isn't canonical but using this app I can configure it all in one place and handle cases such as S3 that don't support naked domains at all.

### How

Do:

    git clone git@github.com:RandomEtc/heroku-www-redirect.git
    cd heroku-www-redirect
    heroku create your-app-name
    heroku domains:add example.com
    git push heroku master

You can __add as many domains as you like__, so long as they don't start with `www`.

Then add Heroku's IP addresses as A records in your DNS settings panel. At the time of writing these are:

    75.101.163.44
    75.101.145.87
    174.129.212.2

These are unlikely to change but given the above wriggling you should [check the Devcenter](https://devcenter.heroku.com/articles/custom-domains#naked-domains-mydomaincom) to confirm.

### Caveats

Single process Heroku apps are free at the time of writing but might not be forever. If they don't get much traffic they get spun down so the first request that wakes them up can be quite slow.

I haven't tested this with `https`, or even thought about it except while typing these words.


