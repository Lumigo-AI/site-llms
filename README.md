# site-llms.xml Standard for eCommerce Product Catalogs

## Introduction

This document defines the "site-llms.xml" standard, a **fork and extension** of the original "llms.txt" standard available at [https://github.com/AnswerDotAI/llms-txt](https://github.com/AnswerDotAI/llms-txt).

While the original "llms.txt" standard provides a simple text-based format for exposing product or content information to Large Language Models (LLMs), this fork adapts and extends the concept specifically for large-scale eCommerce websites by introducing a **LLM-friendly XML sitemap** format.

The "site-llms.xml" standard enables eCommerce platforms to efficiently expose product-specific "llms.txt" files via a scalable, structured XML sitemap, facilitating better indexing and retrieval by LLMs.

## Motivation

Modern eCommerce sites often host thousands or millions of products. Traditional crawling or parsing of product pages is inefficient and impractical for LLMs due to:

- **Context window limitations**: LLMs can only process a limited amount of text at once
- **HTML complexity**: Product pages contain numerous UI elements, scripts, and styling that create noise
- **Crawling inefficiency**: Navigating through pagination and category structures is resource-intensive
- **Data freshness challenges**: Ensuring LLMs access the most current product information

We at [Lumigo](https://lumigo.ai), with the fork of the original “llms.txt” approach, want to introduce an XML sitemap format, “site-llms.xml”, providing a scalable, machine-readable index pointing LLMs directly to product-specific “llms.txt” files, improving accessibility and accuracy of responses.

## Relationship to the Original llms.txt Standard

This project is a **fork** of the original "llms.txt" standard, extending its capabilities while maintaining compatibility:

### Original llms.txt Standard Overview

The original "llms.txt" standard defines a simple text file format to expose product or content metadata to LLMs. It follows these key principles:

1. **Markdown-based**: Uses Markdown for human and LLM readability while maintaining a precise format
2. **Root location**: Located at `/llms.txt` in the website root (similar to robots.txt and sitemap.xml)
3. **Structured format**: Contains specific sections in a defined order:
   - H1 title (required)
   - Blockquote with short summary
   - Optional detailed information paragraphs
   - H2 headers with file lists containing hyperlinks to detailed content

### How site-llms.xml Extends the Original Standard

- This project **forks** the original concept by creating a **site-wide XML sitemap** ("site-llms.xml") that lists URLs to individual "llms.txt" files for each product
- The "llms.txt" files themselves remain compliant with the original standard
- This approach maintains backward compatibility while enabling scalability for large catalogs
- The XML format leverages existing sitemap infrastructure and expertise in eCommerce platforms

## Standard Specifications

### File Location and Naming

- **Location:** The "site-llms.xml" file should be hosted at the root of the website, e.g., `https://www.example.com/site-llms.xml`
- **Format:** Valid XML sitemap format, following the sitemap protocol
- **Encoding:** UTF-8 encoding is required

### XML Structure

- The file must be a valid XML document
- The root element must be `<urlset>` with the namespace attribute `xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"`
- Each product's "llms.txt" file is represented by a `<url>` element

### URL Structure

- Each `<url>` element must contain a `<loc>` tag with the URL pointing to the product's "llms.txt" file
- The "llms.txt" URL is derived by appending `/llms.txt` to the product page URL  
  Example:  
  Product page URL: `https://www.example.com/product-123`  
  Corresponding llms.txt URL: `https://www.example.com/product-123/llms.txt`

### Optional Elements

Optional tags such as `<lastmod>`, `<changefreq>`, and `<priority>` may be included as per standard sitemap conventions:

- `<lastmod>`: The date of last modification of the "llms.txt" file (W3C Datetime format)
- `<changefreq>`: How frequently the "llms.txt" file is likely to change
- `<priority>`: The priority of the "llms.txt" file relative to other URLs on the site

## Example XML Sitemap

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.example.com/product-1/llms.txt</loc>
    <lastmod>2025-04-01T12:00:00+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://www.example.com/product-2/llms.txt</loc>
    <lastmod>2025-04-02T14:30:00+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://www.example.com/product-3/llms.txt</loc>
    <lastmod>2025-04-03T09:15:00+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <!-- Additional product URLs -->
</urlset>
```

## Sitemap Index Support

For very large catalogs (exceeding 50,000 products), the "site-llms.xml" can be split into multiple files and referenced through a sitemap index:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://www.example.com/site-llms-1.xml</loc>
    <lastmod>2025-04-15T12:00:00+00:00</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://www.example.com/site-llms-2.xml</loc>
    <lastmod>2025-04-15T12:00:00+00:00</lastmod>
  </sitemap>
  <!-- Additional sitemap references -->
</sitemapindex>
```

## Product-specific llms.txt Files

Each product's "llms.txt" file should follow the original llms.txt standard format:

```markdown
# Product Name

> Brief description of the product with key information necessary for understanding its features and benefits.

Additional details about the product, including important notes or context.

## Specifications

- [Detailed specifications](https://www.example.com/product-123/specs.md): Complete technical specifications
- [User manual](https://www.example.com/product-123/manual.md): Instructions for use and maintenance

## Optional

- [Reviews](https://www.example.com/product-123/reviews.md): Customer reviews and ratings
- [Accessories](https://www.example.com/product-123/accessories.md): Compatible accessories and add-ons
```

Note that the "Optional" section has a special meaning in the llms.txt standard—if it's included, the URLs provided there can be skipped if a shorter context is needed. Use it for secondary information which can often be skipped.

## Implementation Tips and Best Practices

### Generation and Maintenance

- **Automate generation**: Create "site-llms.xml" using scripts or CMS/database integrations
- **Dynamic generation**: For large catalogs, consider dynamic generation with caching to optimize performance
- **Update frequency**: Regenerate the sitemap when products are added, removed, or significantly updated
- **Compression**: For large sitemaps, consider using gzip compression (site-llms.xml.gz) to reduce file size

### Discovery and Integration

- **Robots.txt**: Add a reference to your "site-llms.xml" in your robots.txt file:
  ```
  Sitemap: https://www.example.com/site-llms.xml
  ```
- **Main sitemap**: Include "site-llms.xml" in your main sitemap index (sitemap.xml) to facilitate discovery by crawlers and LLMs
- **HTTP headers**: Ensure proper HTTP headers are set, including Content-Type: application/xml

### Performance Considerations

- **Caching**: Implement server-side caching to reduce generation overhead
- **Incremental updates**: For large catalogs, consider implementing incremental updates rather than regenerating the entire sitemap
- **Load testing**: Verify that your server can handle the additional requests for "llms.txt" files

### Creating Effective llms.txt Files

Following the guidelines from the original standard:

- Use concise, clear language
- When linking to resources, include brief, informative descriptions
- Avoid ambiguous terms or unexplained jargon
- Test your llms.txt files with various language models to ensure they can answer questions about your content

## License Compliance: Creative Commons Attribution-ShareAlike (CC BY-SA)

The sitemap protocol, including the XML sitemap format used by "site-llms.xml", is licensed under the Creative Commons Attribution-ShareAlike 3.0 License (CC BY-SA 3.0). This means:

- You must give appropriate credit to the sitemap protocol authors (see [http://www.sitemaps.org/protocol.html#license](http://www.sitemaps.org/protocol.html#license))
- If you modify or build upon the sitemap format (as done in this fork), you must distribute your contributions under the same license
- When publishing or distributing the "site-llms.xml" standard or generated files, include a clear attribution statement and a link to the original sitemap license
- Ensure that any derivative works respect the ShareAlike clause, promoting open collaboration and sharing

### Example Attribution Statement

Include this statement in your documentation or repository:

```
This project uses and extends the sitemap XML protocol licensed under Creative Commons 
Attribution-ShareAlike 3.0 License. For details, see 
http://www.sitemaps.org/protocol.html#license.
```

## Example Code Snippets

### Python Script to Generate "site-llms.xml" Dynamically

```python
import xml.etree.ElementTree as ET
from datetime import datetime
import os
import markdown
import json

def generate_site_llms_xml(product_data, output_file="site-llms.xml"):
    """
    Generate a site-llms.xml file from product data.
    
    Args:
        product_data (list): List of product dictionaries with 'url', 'name', 'description', etc.
        output_file (str): Output filename for the sitemap
    """
    # Create the root element with namespace
    urlset = ET.Element("urlset", xmlns="http://www.sitemaps.org/schemas/sitemap/0.9")
    
    # Current date for lastmod
    current_date = datetime.now().strftime("%Y-%m-%dT%H:%M:%S+00:00")
    
    # Add each product's llms.txt URL
    for product in product_data:
        # Generate llms.txt file for this product
        product_url = product['url']
        generate_product_llms_txt(product)
        
        # Add to sitemap
        url = ET.SubElement(urlset, "url")
        
        # Location of the llms.txt file
        loc = ET.SubElement(url, "loc")
        loc.text = f"{product_url}/llms.txt"
        
        # Optional elements
        lastmod = ET.SubElement(url, "lastmod")
        lastmod.text = current_date
        
        changefreq = ET.SubElement(url, "changefreq")
        changefreq.text = "weekly"
        
        priority = ET.SubElement(url, "priority")
        priority.text = "0.8"
    
    # Create the XML tree and write to file
    tree = ET.ElementTree(urlset)
    ET.indent(tree, space="  ", level=0)  # Pretty print (Python 3.9+)
    
    # Write to file with XML declaration
    with open(output_file, "wb") as f:
        f.write(b'<?xml version="1.0" encoding="UTF-8"?>\n')
        tree.write(f, encoding="utf-8")
    
    print(f"Generated {output_file} with {len(product_data)} product URLs")

def generate_product_llms_txt(product):
    """
    Generate a llms.txt file for a specific product.
    
    Args:
        product (dict): Product data including 'url', 'name', 'description', etc.
    """
    # Extract product info
    product_url = product['url']
    product_name = product['name']
    product_description = product['description']
    product_details = product.get('details', '')
    
    # Create directory structure if needed
    product_path = product_url.replace('https://www.example.com', '/var/www/html')
    os.makedirs(product_path, exist_ok=True)
    
    # Create llms.txt content following the standard format
    llms_content = f"""# {product_name}

> {product_description}

{product_details}

## Specifications

- [Technical specifications]({product_url}/specs.md): Complete product specifications
- [Dimensions and weight]({product_url}/dimensions.md): Physical characteristics

## Optional

- [Customer reviews]({product_url}/reviews.md): Ratings and feedback
- [Related products]({product_url}/related.md): Similar or complementary items
"""
    
    # Write the llms.txt file
    with open(f"{product_path}/llms.txt", "w", encoding="utf-8") as f:
        f.write(llms_content)
    
    print(f"Generated llms.txt for {product_name}")

# Example usage
if __name__ == "__main__":
    # Example product data (in a real scenario, this would come from a database)
    products = [
        {
            "url": "https://www.example.com/product-1",
            "name": "Premium Wireless Headphones",
            "description": "High-quality wireless headphones with noise cancellation and 30-hour battery life.",
            "details": "These headphones feature Bluetooth 5.0 connectivity, active noise cancellation, and premium audio drivers for exceptional sound quality."
        },
        {
            "url": "https://www.example.com/product-2",
            "name": "Smart Home Hub",
            "description": "Central control device for your smart home ecosystem, compatible with major voice assistants.",
            "details": "Control lights, thermostats, security systems, and more from a single interface. Works with Alexa, Google Assistant, and HomeKit."
        },
        {
            "url": "https://www.example.com/product-3",
            "name": "Ergonomic Office Chair",
            "description": "Adjustable office chair designed for comfort during long work sessions.",
            "details": "Features include lumbar support, adjustable armrests, height adjustment, and breathable mesh back."
        }
    ]
    
    generate_site_llms_xml(products)
```

### PHP Example for Dynamic Generation with Database Integration

```php
<?php
/**
 * Generate site-llms.xml dynamically from product database
 */
class SiteLlmsGenerator {
    private $db;
    private $baseUrl;
    private $outputPath;
    
    /**
     * Constructor
     * 
     * @param PDO $db Database connection
     * @param string $baseUrl Base URL of the website
     * @param string $outputPath Server path to write files
     */
    public function __construct($db, $baseUrl, $outputPath) {
        $this->db = $db;
        $this->baseUrl = rtrim($baseUrl, '/');
        $this->outputPath = rtrim($outputPath, '/');
    }
    
    /**
     * Generate the site-llms.xml file
     */
    public function generateSitemap() {
        // Get products from database
        $products = $this->getProductsFromDatabase();
        
        // Start XML output
        $xml = '<?xml version="1.0" encoding="UTF-8"?>' . "\n";
        $xml .= '<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">' . "\n";
        
        // Current date for lastmod
        $currentDate = date('c');
        
        // Process each product
        foreach ($products as $product) {
            // Generate llms.txt file for this product
            $this->generateProductLlmsTxt($product);
            
            // Add to sitemap
            $productUrl = $this->baseUrl . '/product/' . $product['slug'];
            $llmsTxtUrl = $productUrl . '/llms.txt';
            
            $xml .= "  <url>\n";
            $xml .= "    <loc>" . htmlspecialchars($llmsTxtUrl) . "</loc>\n";
            $xml .= "    <lastmod>" . $currentDate . "</lastmod>\n";
            $xml .= "    <changefreq>weekly</changefreq>\n";
            $xml .= "    <priority>0.8</priority>\n";
            $xml .= "  </url>\n";
        }
        
        // Close XML
        $xml .= '</urlset>';
        
        // Write to file
        file_put_contents($this->outputPath . '/site-llms.xml', $xml);
        
        return count($products);
    }
    
    /**
     * Generate llms.txt file for a specific product
     */
    private function generateProductLlmsTxt($product) {
        // Create product directory if it doesn't exist
        $productPath = $this->outputPath . '/product/' . $product['slug'];
        if (!is_dir($productPath)) {
            mkdir($productPath, 0755, true);
        }
        
        // Create llms.txt content
        $content = "# " . $product['name'] . "\n\n";
        $content .= "> " . $product['short_description'] . "\n\n";
        $content .= $product['description'] . "\n\n";
        
        // Add specifications section
        $content .= "## Specifications\n\n";
        $content .= "- [Technical details](" . $this->baseUrl . "/product/" . $product['slug'] . "/specs.md): Full specifications\n";
        $content .= "- [Usage instructions](" . $this->baseUrl . "/product/" . $product['slug'] . "/usage.md): How to use this product\n\n";
        
        // Add optional section
        $content .= "## Optional\n\n";
        $content .= "- [Customer reviews](" . $this->baseUrl . "/product/" . $product['slug'] . "/reviews.md): User feedback\n";
        $content .= "- [FAQ](" . $this->baseUrl . "/product/" . $product['slug'] . "/faq.md): Frequently asked questions\n";
        
        // Write to file
        file_put_contents($productPath . '/llms.txt', $content);
    }
    
    /**
     * Get products from database
     */
    private function getProductsFromDatabase() {
        $stmt = $this->db->prepare("SELECT id, name, slug, short_description, description FROM products WHERE active = 1");
        $stmt->execute();
        return $stmt->fetchAll(PDO::FETCH_ASSOC);
    }
}

// Example usage
try {
    // Database connection
    $db = new PDO('mysql:host=localhost;dbname=ecommerce', 'username', 'password');
    $db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    // Create generator
    $generator = new SiteLlmsGenerator(
        $db,
        'https://www.example.com',
        '/var/www/html'
    );
    
    // Generate sitemap
    $count = $generator->generateSitemap();
    echo "Generated site-llms.xml with $count products\n";
    
} catch (PDOException $e) {
    echo "Database error: " . $e->getMessage();
}
?>
```

### Node.js Example with Express Integration

```javascript
const fs = require('fs');
const path = require('path');
const { create } = require('xmlbuilder2');
const express = require('express');
const mongoose = require('mongoose');

// MongoDB Product Schema
const productSchema = new mongoose.Schema({
  name: String,
  slug: String,
  shortDescription: String,
  description: String,
  active: Boolean,
  updatedAt: Date
});

const Product = mongoose.model('Product', productSchema);

/**
 * Generate site-llms.xml file
 * @param {Array} products - Array of product documents from MongoDB
 * @param {string} baseUrl - Base URL of the website
 * @param {string} outputPath - Server path to write files
 */
async function generateSiteLlmsXml(products, baseUrl, outputPath) {
  // Create XML document
  const doc = create({ version: '1.0', encoding: 'UTF-8' })
    .ele('urlset', { xmlns: 'http://www.sitemaps.org/schemas/sitemap/0.9' });
  
  // Process each product
  for (const product of products) {
    // Generate llms.txt file for this product
    await generateProductLlmsTxt(product, baseUrl, outputPath);
    
    // Add to sitemap
    const productUrl = `${baseUrl}/product/${product.slug}`;
    const llmsTxtUrl = `${productUrl}/llms.txt`;
    
    doc.ele('url')
      .ele('loc').txt(llmsTxtUrl).up()
      .ele('lastmod').txt(product.updatedAt.toISOString()).up()
      .ele('changefreq').txt('weekly').up()
      .ele('priority').txt('0.8');
  }
  
  // Convert to string with pretty formatting
  const xml = doc.end({ prettyPrint: true });
  
  // Write to file
  const sitemapPath = path.join(outputPath, 'site-llms.xml');
  fs.writeFileSync(sitemapPath, xml);
  
  console.log(`Generated site-llms.xml with ${products.length} products`);
  return sitemapPath;
}

/**
 * Generate llms.txt file for a specific product
 */
async function generateProductLlmsTxt(product, baseUrl, outputPath) {
  // Create product directory if it doesn't exist
  const productPath = path.join(outputPath, 'product', product.slug);
  if (!fs.existsSync(productPath)) {
    fs.mkdirSync(productPath, { recursive: true });
  }
  
  // Create llms.txt content
  const content = `# ${product.name}

> ${product.shortDescription}

${product.description}

## Specifications

- [Technical details](${baseUrl}/product/${product.slug}/specs.md): Full specifications
- [Usage instructions](${baseUrl}/product/${product.slug}/usage.md): How to use this product

## Optional

- [Customer reviews](${baseUrl}/product/${product.slug}/reviews.md): User feedback
- [FAQ](${baseUrl}/product/${product.slug}/faq.md): Frequently asked questions
`;
  
  // Write to file
  const llmsTxtPath = path.join(productPath, 'llms.txt');
  fs.writeFileSync(llmsTxtPath, content);
  
  console.log(`Generated llms.txt for ${product.name}`);
}

// Express app setup
const app = express();
const port = process.env.PORT || 3000;

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/ecommerce')
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('MongoDB connection error:', err));

// Route to dynamically generate and serve site-llms.xml
app.get('/site-llms.xml', async (req, res) => {
  try {
    // Get active products from database
    const products = await Product.find({ active: true });
    
    // Set content type
    res.header('Content-Type', 'application/xml; charset=utf-8');
    
    // Create XML document
    const doc = create({ version: '1.0', encoding: 'UTF-8' })
      .ele('urlset', { xmlns: 'http://www.sitemaps.org/schemas/sitemap/0.9' });
    
    // Base URL from request
    const baseUrl = `${req.protocol}://${req.get('host')}`;
    
    // Add each product URL
    for (const product of products) {
      const productUrl = `${baseUrl}/product/${product.slug}`;
      const llmsTxtUrl = `${productUrl}/llms.txt`;
      
      doc.ele('url')
        .ele('loc').txt(llmsTxtUrl).up()
        .ele('lastmod').txt(product.updatedAt.toISOString()).up()
        .ele('changefreq').txt('weekly').up()
        .ele('priority').txt('0.8');
    }
    
    // Convert to string with pretty formatting
    const xml = doc.end({ prettyPrint: true });
    
    // Send response
    res.send(xml);
    
  } catch (error) {
    console.error('Error generating site-llms.xml:', error);
    res.status(500).send('Error generating site-llms.xml');
  }
});

// Route to serve llms.txt files for products
app.get('/product/:slug/llms.txt', async (req, res) => {
  try {
    const product = await Product.findOne({ slug: req.params.slug, active: true });
    
    if (!product) {
      return res.status(404).send('Product not found');
    }
    
    // Base URL from request
    const baseUrl = `${req.protocol}://${req.get('host')}`;
    
    // Create llms.txt content
    const content = `# ${product.name}

> ${product.shortDescription}

${product.description}

## Specifications

- [Technical details](${baseUrl}/product/${product.slug}/specs.md): Full specifications
- [Usage instructions](${baseUrl}/product/${product.slug}/usage.md): How to use this product

## Optional

- [Customer reviews](${baseUrl}/product/${product.slug}/reviews.md): User feedback
- [FAQ](${baseUrl}/product/${product.slug}/faq.md): Frequently asked questions
`;
    
    // Set content type and send response
    res.header('Content-Type', 'text/markdown; charset=utf-8');
    res.send(content);
    
  } catch (error) {
    console.error('Error generating llms.txt:', error);
    res.status(500).send('Error generating llms.txt');
  }
});

// Start server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

## Comparison with Original llms.txt Standard

| Feature | Original llms.txt | site-llms.xml (This Fork) |
|---------|-------------------|---------------------------|
| **Format** | Markdown text file | XML sitemap with links to Markdown files |
| **Location** | `/llms.txt` at website root | `/site-llms.xml` at website root |
| **Scalability** | Limited for large catalogs | Highly scalable for millions of products |
| **Discovery** | Manual or via robots.txt | Via robots.txt and sitemap.xml |
| **Content Structure** | H1, blockquote, paragraphs, H2 sections | XML structure pointing to compliant llms.txt files |
| **Integration with Web Standards** | Standalone | Leverages existing sitemap protocol |
| **Primary Use Case** | General websites, documentation | eCommerce product catalogs |
| **Content Granularity** | Site-wide information | Product-specific information |

## Benefits of the site-llms.xml Standard

### For eCommerce Platforms

- **Scalability**: Efficiently handle large product catalogs with thousands or millions of items
- **Performance**: Reduce server load by providing a structured path to product information
- **Control**: Maintain control over what product information is exposed to LLMs
- **Freshness**: Ensure LLMs access the most current product information
- **SEO alignment**: Leverage existing sitemap infrastructure and expertise

### For LLMs and AI Services

- **Efficient indexing**: Quickly locate and retrieve product-specific information
- **Reduced noise**: Access clean, structured product data without HTML parsing
- **Improved accuracy**: Generate more accurate responses based on authoritative product information
- **Context optimization**: Make better use of limited context windows by focusing on relevant data
- **Standardization**: Process product information in a consistent format across different eCommerce sites

### For End Users

- **Better answers**: Receive more accurate and up-to-date information about products
- **Reduced hallucinations**: Minimize AI-generated misinformation about products
- **Improved shopping experience**: Get reliable assistance with product selection and comparison

## Practical Implementation Scenarios

### Scenario 1: Large eCommerce Platform

A large eCommerce platform with millions of products can implement site-llms.xml by:

1. Creating a database-driven generator that produces both the site-llms.xml sitemap and individual llms.txt files for each product
2. Using a sitemap index to split the catalog into manageable chunks (e.g., by category or alphabetically)
3. Setting up a scheduled task to regenerate the sitemap daily or when product information changes
4. Implementing caching to reduce server load from LLM requests

### Scenario 2: Small to Medium Online Store

A smaller online store can implement site-llms.xml by:

1. Using a plugin or extension for their eCommerce platform (e.g., WooCommerce, Shopify)
2. Generating a single site-llms.xml file that points to dynamically generated llms.txt files
3. Manually reviewing and enhancing the most important product descriptions for LLM consumption

### Scenario 3: Marketplace with Multiple Vendors

A marketplace with products from multiple vendors can implement site-llms.xml by:

1. Creating a standardized template for vendor product information
2. Generating llms.txt files that include both marketplace-specific and vendor-provided information
3. Using the sitemap to organize products by vendor, category, or other relevant attributes

## Conclusion

The "site-llms.xml" standard is a practical and scalable fork of the original "llms.txt" format, designed to meet the needs of large eCommerce platforms. By exposing product-specific "llms.txt" files through a dedicated XML sitemap, it enables LLMs to access product information efficiently and accurately, while fully respecting the licensing terms of the sitemap protocol.

This standard bridges the gap between the simplicity of the original "llms.txt" approach and the scalability requirements of large eCommerce sites, providing a path forward for better integration between online stores and AI assistants.

## Next Steps

- Develop tools and integrations to automate "site-llms.xml" generation for popular eCommerce platforms
- Promote adoption of this forked standard within the eCommerce and AI communities
- Maintain backward compatibility with the original "llms.txt" files
- Ensure ongoing compliance with licensing requirements
- Gather feedback from implementers to refine and improve the standard

---

**Note**: This documentation extends and forks the original "llms.txt" standard from [https://github.com/AnswerDotAI/llms-txt](https://github.com/AnswerDotAI/llms-txt) and complies with the Creative Commons Attribution-ShareAlike 3.0 License of the sitemap protocol ([http://www.sitemaps.org/protocol.html#license](http://www.sitemaps.org/protocol.html#license)).
