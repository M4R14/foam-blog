# Js Web Scraping

ติดตั้งเครื่องมือ

```sh
$ yarn add request-promise cheerio
```

```js
// app.js
const rp = require('request-promise');
const cheerio = require('cheerio');
const fs = require('fs');

const domain = 'https://www.set.or.th';
```

get Data

```js
// app.js
async function getData(url) {
    const companies = [];
    const html = await rp(url)
    const $ = cheerio.load(html);
    const querySelector = $("#maincontent > div > div > div:nth-child(3) > div a")

    const links = []
    querySelector.each((key, s) => {
       links.push(s.attribs.href)
    })

    for (let index = 0; index < links.length; index++) {
        const page_url = `${domain}${links[index]}`;
        const html_page = await rp(page_url)
        const $html_page = cheerio.load(html_page)
        const $trs = $html_page('#maincontent > div > div > div.table-responsive > table > tbody tr')
        // > td:nth-child(1) > a

        $trs.each((key, tr) => {
            console.log($(tr).find('td:nth-child(2)').text())
            companies.push({
                link: $(tr).find('td:nth-child(1) > a')[0].attribs.href,
                name: $(tr).find('td:nth-child(2)').text(),
            })
        })

        console.log('##############\n')
    }

    console.log(companies.length);

    return companies
}
```

write Json

```js
function writeJson(data) {
    fs.writeFile("companies.json", JSON.stringify(data), function(err) {
        if (err) { console.log(err); }
    });
}
```

main function

```js
// app.js

(async function () {
    const url = `${domain}/set/commonslookup.do?language=th&country=TH&prefix=NUMBER`
    const companies = await getData(url)
    writeJson(companies)
})()
```