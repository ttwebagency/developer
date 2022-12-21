# SEO (Search Engine Optimisation)

+ [Structured Data](#structured-data)
+ [Gatsby](#gatsby)
	+ [Add Structured Data](#add-structured-data)

## Structured Data
Structured data provides `search engines` with key information about a website which helps to improve SEO and helping to improve search results in `SERPs (Search Engine Results Pages)`. Structured data provides `markup` to pages to tell search engines what type of data our page contains and how it should be displayed. Structured data is an organised data structure that can be processed by computers and machine learning algorithms.

We can use the `Schema.org` vocabulary to add markup to pages. Search engines often use this data to create `rich snippets` in search results.

The following `data formats` can be used to embed `Schema.org` markup into a page:

+ `JSON-LD`
+ `Microdata`
+ `RDFa`

Google recommends using the `JSON-LD` format which uses a JavaScript object to embed the `Schema.org` markup into a page.

## Gatsby

### Add Structured Data
We can use `JSON.stringify()` and `Gatsby Head API` to add structured data to Gatsby. This is added within the `Head` component of the `Gatsby Head API`.

```javascript
export const Head = () => {
	// Create the schema as an object
	const schema = {
		"@context": `https://schema.org`,
		"@type": `Organization`,
		name: `Company Ltd`,
		...
	}
	
	// Stringify the schema object (adding "null, 2" will provide readable JSON)
	const schemaAsString = JSON.stringify(schema, null, 2)

	return (
		<>
			<script type="application/ld+json">{schemaAsString}</script>
		</>
	)
}
```
