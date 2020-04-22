<p align="center">
  <a style="padding-right: 16px;" href="https://forestry.io">
    <img src="https://app.forestry.io/assets/forestry-logotype-pos-c71a6bd237d9199d0457ba2811553997ff5bab0d2cd0e740686ab26c00d9c240.svg" width="112" height="28">
  </a>
  <a href="https://www.gatsbyjs.org/">
    <img src="/static/gatsby_logo.svg" width="112" height="28">
  </a>
</p>
<p align="center">
</p>
<h1 align="center">
  Gatsby Forest Blog
</h1>
<p align="center"></p>

## About
[![Netlify Status](https://api.netlify.com/api/v1/badges/b9674fab-0072-467c-b8c9-3a44c636ba69/deploy-status)](https://app.netlify.com/sites/wizardly-knuth-b8a540/deploys)

Gatsby Forest Blog is a static CMS blog. This means that every post to the blog via forestry is actually a git commit!

After posting an article, it takes only a minute or two for the changes to go live and the update to be pushed into the github repo.

#### Setting Up & Using This Template:
Make sure you have the [Gatsby CLI](https://www.gatsbyjs.org/docs/quick-start/#install-the-gatsby-cli) installed.

In your terminal, navigate to where you would like this blog to live, then run 
```bash
gatsby new gatsby-foresty-blog https://github.com/BlaineAndersonDev/forest.git
cd gatsby-foresty-blog
yarn dev 
```
A new browser window should open with the dev server running or you can navigate to localhost:8000 

### Credit:
This project is based on [Brevifolia](https://github.com/kendallstrautman/brevifolia-gatsby-forestry), the minimalist blog starter to get you going using [Forestry](https://forestry.io/) with [Gatsby](https://www.gatsbyjs.org/).

# Gatsby Basics Review:
### Gatsby Tutorial - Notes on section 1 - Building Blocks
* Any .js file in src/pages/ automatically has a route attached with the same name.
  * I.E. `about.js` would be reachable by using `http://localhost:8000/about`.
* Linking to other Gatsby 'src/pages/' can be done by adding the requirement then adding the Link into JSX:
  * `import { Link } from "gatsby"`
  * `<Link to="/contact/">Contact</Link>`
  * *Note - For external links, use the normal <a></a> tag.*
* Gatsby File Structure:
  ```
  ├── package.json
  ├── src
  │   ├── components
  │   │  └── button.js
  │   ├── pages
  │   │  └── index.js
  │   ├── styles
  │   │  └── index.css
  │   ├── templates
  │   │  └── blog-post.js
  │   └── utils
  │      └── typography.js
  ```
    * `components`: Houses reused bits of code that is pre-styled and usually accepts props. I.E. A Button.
    * `pages`: Contains all the individual pages available to the user. 
    * `styles`: Can contain stylings for pages/components, or can be omitted in favor of keeping stylings near their page or component.
    * `utils`: Used to store any configuration files for Plugins.
* Deploying with Surge:
  * `gatsby build`.
  * `surge public` then press `enter`.
  * Check the generated domain.

### Gatsby Tutorial - Notes on section 2 - Styling
* Gatsby allows the use of 'CSS Modules' by default. This means that the CSS file would end with `.module.css` instead of the traditional `.css`.
  * CSS Modules automatically generate unique class or id names to ensure css is never repeated throughout the project.
* It is also best practice to either store the css file in the same directory as the files being styled OR storing all the css files in 'src/styles/', but do not mix.

### Gatsby Tutorial - Notes on section 3 - Layouts & Plugins
* 'Gatsby Plugins' are Javascript packages that add functionality to a Gatsby site.
  * Plugins must be added to the `gatsby-config.js` when installed. This is so Gatsby knows what plugins its using. I.E. installing the `gatsby-plugin-typography` Plugin.
    * `npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates`
    ```
    /* gatsby-config.js */
    module.exports = {
      plugins: [
        {
          resolve: `gatsby-plugin-typography`,
          options: {
            pathToConfigModule: `src/utils/typography`,
          },
        },
      ],
    }
    ```
    ```
    /* src/utils/typography.js */
    import Typography from "typography"
    import fairyGateTheme from "typography-theme-fairy-gates"

    const typography = new Typography(fairyGateTheme)

    export const { scale, rhythm, options } = typography
    export default typography
    ```
* 'Layout Components' are for sections of your Gatsby site you wish to share across multiple pages (I.E. Navigation or Footer).
  * Create the reusable `layout.js` file in your components directory:
    * *Note - The layout component could also have its css in a seperate file to make it more legible.*
    ```
    /* src/components/layout.js */
    import React from "react"
    import { Link } from "gatsby"
    const ListLink = props => (
      <li style={{ display: `inline-block`, marginRight: `1rem` }}>
        <Link to={props.to}>{props.children}</Link>
      </li>
    )

    export default ({ children }) => (
      <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
        <header style={{ marginBottom: `1.5rem` }}>
          <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
            <h3 style={{ display: `inline` }}>MySweetSite</h3>
          </Link>
          <ul style={{ listStyle: `none`, float: `right` }}>
            <ListLink to="/">Home</ListLink>
            <ListLink to="/about/">About</ListLink>
            <ListLink to="/contact/">Contact</ListLink>
          </ul>
        </header>
        {children}
      </div>
    )
    ```
  * Wrap any 'pages' in the layout to incorporate the changes:
    ```
    import React from "react"
    import Layout from "../components/layout"

    export default () => (
      <Layout>
        <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
        <p>
          What do I like to do? Lots of course but definitely enjoy building
          websites.
        </p>
      </Layout>
    )
    ```

### Gatsby Tutorial - Notes on section 4 - Data / GraphQL
* Gatsby (by default) uses GraphQL to enable components to declare the data they need.
  * To use GraphQL in an example, we will add metadata to our `gatsby-config.js`:
    ```
    /* gatsby-config.js */
    module.exports = {
      siteMetadata: {
        title: `Title from siteMetadata`,
      },
      plugins: [
        `gatsby-plugin-emotion`,
        {
          resolve: `gatsby-plugin-typography`,
          options: {
            pathToConfigModule: `src/utils/typography`,
          },
        },
      ],
    }
    ```
  * Then we use a 'page query' to 'pull' that information using GraphQL (this example uses the about.js page): 
    ```
    /* src/pages/about.js */
    import React from "react"
    import { graphql } from "gatsby"
    import Layout from "../components/layout"

    export default ({ data }) => (
      <Layout>
        <h1>About {data.site.siteMetadata.title}</h1>
        <p>
          We're the only site running on your computer dedicated to showing the best
          photos and videos of pandas eating lots of food.
        </p>
      </Layout>
    )

    export const query = graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
    ```
    * *Note - Page queries live outside of the component definition — by convention at the end of a page component file — and are only available on page components.*
* As of Gatsby 2.0, non-pages components (Components NOT in the /src/pages/ directory) can also use GraphQL to query using a special function called `StaticQuery`. I.E. The Layout:
  ```
  /* src/components/layout.js */
  import React from "react"
  import { css } from "@emotion/core"
  import { useStaticQuery, Link, graphql } from "gatsby"

  import { rhythm } from "../utils/typography"
  export default ({ children }) => {
    const data = useStaticQuery(
      graphql`
        query {
          site {
            siteMetadata {
              title
            }
          }
        }
      `
    )
    return (
      <div
        css={css`
          margin: 0 auto;
          max-width: 700px;
          padding: ${rhythm(2)};
          padding-top: ${rhythm(1.5)};
        `}
      >
        <Link to={`/`}>
          <h3
            css={css`
              margin-bottom: ${rhythm(2)};
              display: inline-block;
              font-style: normal;
            `}
          >
            {data.site.siteMetadata.title}
          </h3>
        </Link>
        <Link
          to={`/about/`}
          css={css`
            float: right;
          `}
        >
          About
        </Link>
        {children}
      </div>
    )
  }
  ```
* 
* 

### Gatsby Tutorial - Notes on section 5 - Source Plugins and Rendering Queried Data
* Obtain a query based on your needs by using `http://localhost:8000/___graphql`
* Implement that into a PageQuery:
  ```
  import React from "react"
  import { graphql } from "gatsby"
  import Layout from "../components/layout"

  export default ({ data }) => {
    console.log(data)
    return (
      <Layout>
        <div>
          <h1>My Site's Files</h1>
          <table>
            <thead>
              <tr>
                <th>relativePath</th>
                <th>prettySize</th>
                <th>extension</th>
                <th>birthTime</th>
              </tr>
            </thead>
            <tbody>
              {data.allFile.edges.map(({ node }, index) => (
                <tr key={index}>
                  <td>{node.relativePath}</td>
                  <td>{node.prettySize}</td>
                  <td>{node.extension}</td>
                  <td>{node.birthTime}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </Layout>
    )
  }

  export const query = graphql`
    query {
      allFile {
        edges {
          node {
            relativePath
            prettySize
            extension
            birthTime(fromNow: true)
          }
        }
      }
    }
  `
  ```
* 

### Gatsby Tutorial - Notes on section 6 - Transformer Plugins
* [Tutorial Section](https://www.gatsbyjs.org/tutorial/part-six/)
* Transformer plugins transform the raw content brought by source plugins.
  * The combination of source plugins and transformer plugins can handle all data sourcing and data transformation you might need when building a Gatsby site.
* Source Plugins **Bring in raw data** to Gatsby.
* Transformer Plugins **Transform that raw data** into usable information.

### Gatsby Tutorial - Notes on section 7 - Programmatically Create Pages from Data
* A ‘slug’ is the unique identifying part of a web address, such as the /tutorial/part-seven part of the page https://www.gatsbyjs.org/tutorial/part-seven/.
  * *Note that a slug is also referred to as a 'path'.* 
* We will use two Gatsby APIs: `onCreateNode` & `createPages`. To implement an API, you export a function with the name of the API from gatsby-node.js:
  ```
  /* gatsby-node.js */
  const path = require(`path`)
  const { createFilePath } = require(`gatsby-source-filesystem`)

  exports.onCreateNode = ({ node, getNode, actions }) => {
    const { createNodeField } = actions
    if (node.internal.type === `MarkdownRemark`) {
      const slug = createFilePath({ node, getNode, basePath: `pages` })
      createNodeField({
        node,
        name: `slug`,
        value: slug,
      })
    }
  }

  exports.createPages = async ({ graphql, actions }) => {
    const { createPage } = actions
    const result = await graphql(`
      query {
        allMarkdownRemark {
          edges {
            node {
              fields {
                slug
              }
            }
          }
        }
      }
    `)

    result.data.allMarkdownRemark.edges.forEach(({ node }) => {
      createPage({
        path: node.fields.slug,
        component: path.resolve(`./src/templates/blog-post.js`),
        context: {
          // Data passed to context is available
          // in page queries as GraphQL variables.
          slug: node.fields.slug,
        },
      })
    })
  }
  ```
  * The `onCreateNode` function will be called by Gatsby whenever a new node is created (or updated) and makes it into a slug (filepath).
  * The `createPage` function takes those nodes and generates a page using the specified template, and provided slug for URL navigation.
* The `blog-post.js` template is used for this, which is located at src/templates:
  ```
  /* src/templates/blog-post.js */
  import React from "react"
  import { graphql } from "gatsby"
  import Layout from "../components/layout"

  export default ({ data }) => {
    const post = data.markdownRemark
    return (
      <Layout>
        <div>
          <h1>{post.frontmatter.title}</h1>
          <div dangerouslySetInnerHTML={{ __html: post.html }} />
        </div>
      </Layout>
    )
  }

  export const query = graphql`
    query($slug: String!) {
      markdownRemark(fields: { slug: { eq: $slug } }) {
        html
        frontmatter {
          title
        }
      }
    }
  `
  ```
* Navigation can also be added to to each blog-post so the user can open them in their own page:
  ```
  /* src/pages/index.js */
  import React from "react"
  import { css } from "@emotion/core"
  import { Link, graphql } from "gatsby"
  import { rhythm } from "../utils/typography"
  import Layout from "../components/layout"

  export default ({ data }) => {
    return (
      <Layout>
        <div>
          <h1
            css={css`
              display: inline-block;
              border-bottom: 1px solid;
            `}
          >
            Amazing Pandas Eating Things
          </h1>
          <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
          {data.allMarkdownRemark.edges.map(({ node }) => (
            <div key={node.id}>
              <Link
                to={node.fields.slug}
                css={css`
                  text-decoration: none;
                  color: inherit;
                `}
              >
                <h3
                  css={css`
                    margin-bottom: ${rhythm(1 / 4)};
                  `}
                >
                  {node.frontmatter.title}{" "}
                  <span
                    css={css`
                      color: #555;
                    `}
                  >
                    — {node.frontmatter.date}
                  </span>
                </h3>
                <p>{node.excerpt}</p>
              </Link>
            </div>
          ))}
        </div>
      </Layout>
    )
  }

  export const query = graphql`
    query {
      allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
        totalCount
        edges {
          node {
            id
            frontmatter {
              title
              date(formatString: "DD MMMM, YYYY")
            }
            fields {
              slug
            }
            excerpt
          }
        }
      }
    }
  `
  ```

### Gatsby Tutorial - Notes on section 8 - Preparing a Site to Go Live
* Generate and run a production build locally:
  * `gatsby build`: Build the Gatsby site as static.
  * `gatsby serve`: Serve the static site locally.
    * *Note - Served production sites are run by default on `http://localhost:9000`*
* Use Googles [Lighthouse Auditor](https://developers.google.com/web/tools/lighthouse/) to receive audit scores for performance, accessibility, progressive web apps, SEO and more.
  * Open the running production build on an incognito tab of Chrome (This prevents interferance from extensions).
  * Open Googles Developer Tools and select the 'audit' tab.
  * Click on 'Generate report' and wait less than 90 seconds for results.
    * *Note - While Google Devtools are available on other browsers, Lighthouse will __only__ work on Google Chrome.*
* PWA ([Progressive Web App](https://web.dev/progressive-web-apps/)) Requirements:
  PWAs are regular websites that take advantage of modern browser functionality to augment the web experience with app-like features and benefits.
    * Requirement 1: [A Web App Manifest](https://web.dev/add-manifest/)
      * Install the `gatsby-plugin-manifest` if you have not already:
        * `npm install --save gatsby-plugin-manifest`
      * Add an icon to src/images/ to use as a favicon (512x512 recommended).
        * [Gatsby provided example image](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png)
      * Make sure to add the required config to gatsby-config.js:
        ```
        {
          resolve: `gatsby-plugin-manifest`,
          options: {
            // https://web.dev/add-manifest/#manifest-properties
            name: `GatsbyJS`,
            short_name: `GatsbyJS`,
            start_url: `/`,
            background_color: `#6b37bf`,
            theme_color: `#6b37bf`,
            display: `standalone`, 
            icon: `src/images/icon.png`,
          },
        },
        ```
    * Requirement 2: [Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) for Offline Support:
      * A service worker runs in the background, deciding to serve network or cached content based on connectivity, allowing for a seamless, managed offline experience.
      * Install the `gatsby-plugin-offline` if you have not already:
        * `npm install --save gatsby-plugin-offline`
      * Make sure to add the required config to gatsby-config.js:
        * `gatsby-plugin-offline`.
        * *Note - This should be listed __after__ the manifest so the offline cache contains the manifest data.*
    * Requirement 3: [Metadata](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head):
      * Adding metadata to pages (such as a title or description) is key in helping search engines like Google understand your content and decide when to surface it in search results.
      * Install the `gatsby-plugin-react-helmet` & `React Helmet` if you have not already:
        * `npm install --save gatsby-plugin-react-helmet react-helmet`
      * Make sure to add the required config at the bottom __and__ metadata section at the top to gatsby-config.js:
        ```
        siteMetadata: {
          title: `Site/App Name`,
          description: `A simple description about the Site/App`,
          author: `Your Name`,
        },
        ```
        * `gatsby-plugin-react-helmet`.
      * This also requires that you create an seo.js file in src/components/:
        ```
        /* src/components/seo.js */
        import React from "react"
        import PropTypes from "prop-types"
        import Helmet from "react-helmet"
        import { useStaticQuery, graphql } from "gatsby"

        function SEO({ description, lang, meta, title }) {
          const { site } = useStaticQuery(
            graphql`
              query {
                site {
                  siteMetadata {
                    title
                    description
                    author
                  }
                }
              }
            `
          )

          const metaDescription = description || site.siteMetadata.description

          return (
            <Helmet
              htmlAttributes={{
                lang,
              }}
              title={title}
              titleTemplate={`%s | ${site.siteMetadata.title}`}
              meta={[
                {
                  name: `description`,
                  content: metaDescription,
                },
                {
                  property: `og:title`,
                  content: title,
                },
                {
                  property: `og:description`,
                  content: metaDescription,
                },
                {
                  property: `og:type`,
                  content: `website`,
                },
                {
                  name: `twitter:card`,
                  content: `summary`,
                },
                {
                  name: `twitter:creator`,
                  content: site.siteMetadata.author,
                },
                {
                  name: `twitter:title`,
                  content: title,
                },
                {
                  name: `twitter:description`,
                  content: metaDescription,
                },
              ].concat(meta)}
            />
          )
        }

        SEO.defaultProps = {
          lang: `en`,
          meta: [],
          description: ``,
        }

        SEO.propTypes = {
          description: PropTypes.string,
          lang: PropTypes.string,
          meta: PropTypes.arrayOf(PropTypes.object),
          title: PropTypes.string.isRequired,
        }

        export default SEO
        ```  
      * The seo.js file can be called in your pages and templates. Passing props to it will increase your SEO score with Lighthouse.
        * *Note - This code automatically queries and sets up most of the common requirements for SEO, but must be called and sent props to actually work. In the event that no props are sent, seo.js will default to the site metadata provided in gatsby-config.js.*
### Gatsby Tutorial - Creating an Image Source Plugin
* 