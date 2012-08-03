S3 can host websites but it can't host "naked" domains. Heroku is happy to have your subdomains pointed at it with CNAME but [gets all wriggly](https://devcenter.heroku.com/articles/avoiding-naked-domains-dns-arecords) if you use A records with IP addresses.

So. This is a Heroku "app" that will happily redirect naked domains to their `www` equivalent. It takes advantage of the fact that the PHP buildpack vendors Apache and supports .htaccess files with mod_rewrite rules.

Do:

```
git clone git@github.com:RandomEtc/heroku-www-redirect.git
cd heroku-www-redirect
heroku create your-app-name
heroku domains:add example.com
git push heroku master
```

You can add as many domains as you like, so long as they don't start with `www`.

Then add Heroku's IP addresses as A records in your DNS settings panel. At the time of writing these are:

```
75.101.163.44
75.101.145.87
174.129.212.2
```

These are unlikely to change but given the above wriggling you should [check the Devcenter](https://devcenter.heroku.com/articles/custom-domains#naked-domains-mydomaincom) to confirm.


