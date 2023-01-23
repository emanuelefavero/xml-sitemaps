# XML Sitemaps

XML sitemaps are a way to inform search engines about the pages on your website that they should crawl and index. They are written in XML format and provide information about the structure of your website, including the URLs of each page and the frequency with which they are updated.

## Table of Contents

- [How to Create an XML Sitemap](#how-to-create-an-xml-sitemap)
- [XML Sitemaps example](#xml-sitemaps-example)
- [XML Sitemaps in Next.js](#xml-sitemaps-in-nextjs)
- [Generate sitemaps with `getServerSideProps`](#generate-sitemaps-with-getserversideprops)
- [Additional information](#additional-information)

&nbsp;

---

&nbsp;

## How to Create an XML Sitemap

1. Create a list of all the URLs on your website that you want search engines to crawl and index.
2. Use a sitemap generator tool to create your sitemap in XML format. There are many free online tools available, such as [XML Sitemaps](https://www.xml-sitemaps.com/).
3. Upload the generated sitemap to the root directory of your website.
4. Submit your sitemap to Google Search Console and Bing Webmaster Tools.

5. It is important to keep your sitemap up-to-date as you add or remove pages from your website, and resubmit it to the search engines.

> Note:
>
> - Make sure to validate your sitemap to ensure it is well-formed, and that there are no errors in the URLs.
> - If you have a large website, you can create multiple sitemaps and submit them all to the search engines.

## XML Sitemaps example

- If you have a simple static site with a few pages, you can create an XML sitemap manually.

```xml
<xml version="1.0" encoding="UTF-8">
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>http://www.example.com/foo</loc>
    <lastmod>2021-06-01</lastmod>
  </url>
</urlset>
</xml>
```

## XML Sitemaps in Next.js

- You can put your sitemap in the `public` directory, which will be served as a static file at the root of your website.
- You can also generate your sitemap dynamically at build time using the `getServerSideProps` function in `pages/sitemap.xml.js`. This is useful if you want to include dynamic URLs in your sitemap, such as pages with [dynamic routes](https://nextjs.org/docs/routing/dynamic-routes).

## Generate sitemaps with `getServerSideProps`

- Create a `pages/sitemap.xml.js` file.
- Add the following code to the file:

```js
//pages/sitemap.xml.js
const EXTERNAL_DATA_URL = 'https://jsonplaceholder.typicode.com/posts'

function generateSiteMap(posts) {
  return `<?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <!--We manually set the two URLs we know already-->
     <url>
       <loc>https://jsonplaceholder.typicode.com</loc>
     </url>
     <url>
       <loc>https://jsonplaceholder.typicode.com/guide</loc>
     </url>
     ${posts
       .map(({ id }) => {
         return `
       <url>
           <loc>${`${EXTERNAL_DATA_URL}/${id}`}</loc>
       </url>
     `
       })
       .join('')}
   </urlset>
 `
}

function SiteMap() {
  // getServerSideProps will do the heavy lifting
}

export async function getServerSideProps({ res }) {
  // We make an API call to gather the URLs for our site
  const request = await fetch(EXTERNAL_DATA_URL)
  const posts = await request.json()

  // We generate the XML sitemap with the posts data
  const sitemap = generateSiteMap(posts)

  res.setHeader('Content-Type', 'text/xml')
  // we send the XML to the browser
  res.write(sitemap)
  res.end()

  return {
    props: {},
  }
}

export default SiteMap
```

> Learn more about [Sitemaps in Next.js](https://nextjs.org/learn/seo/crawling-and-indexing/xml-sitemaps)

## Additional information

- XML sitemaps also provide additional information, such as the priority of URLs, the last time they were modified, and the change frequency.
- XML sitemaps are not the only way to tell search engines about your website, but they are a good way to ensure that all of your pages are indexed and to help search engines understand the structure of your site.
- It is also important to have a robots.txt file on the root of your website to control which pages and files the search engine crawlers should access, and which they should ignore.

&nbsp;

---

&nbsp;

[**Go To Top &nbsp; ⬆️**](#xml-sitemaps)
