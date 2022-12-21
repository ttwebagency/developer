# SEO (Search Engine Optimisation)

+ [Gatsby](#gatsby)
	+ [Add Structured Data](#add-structured-data)

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
