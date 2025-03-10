author: Prabhath Nanisetty
id: build-an-mmm-model-using-meridian-mmm-and-snowflake-notebooks
summary: Build a Marketing Mix Model using Meridian MMM and Snowflake Notebooks
categories: marketing, data-science, data-science-&-ml
environments: web
status: Published 
feedback link: https://github.com/Snowflake-Labs/sfguides/issues
tags: Marketing, Data Science, Data Science & ML, Notebooks, Marketing Mix, MMM, Media Mix

# Build a Marketing Mix Model with Meridian MMM and Snowflake Notebooks
<!-- ------------------------ -->
## Overview 
Duration: 5

Marketing Mix Models (MMMs) are an important tool in any Marketing Effectiveness Program. They give companies a full view of advertising and marketing spend while also providing the ability to simulate various scenarios and guide incremental investment decisions.

They were ubiquitous ~10-15 years ago but have sometimes been replaced by methods such as Multi-Touch Attribution (MTA) with the rise of digital marketing and the ease of tracking customer journeys at the individual-level. Nowadays, however, the slow decline of third-party cookies and the rise of "Walled Garden" publishers have brought MMMs back into vogue. You can read more about the history in Jim Warner's excellent article ["MMM is having its moment"](https://medium.com/snowflake/mmm-is-having-its-moment-8125eab6f405).

MMMs have changed a lot since and many have adopted Bayesian approaches. The benefit is the ability to leverage priors, a powerful concept that helps with adstock and other carryover responses. Niall Oulton has an [excellent summary](https://medium.com/@nialloulton/a-comprehensive-guide-to-bayesian-marketing-mix-modeling-a37da5fde7c4) of Bayesian MMMs.

Several companies have released marketing mix models, the latest - as of this Quickstart publish date - is Meridian MMM developed by Google. For more information on this mode, we encourage you to check out their [site](https://developers.google.com/meridian). This model has been open-sourced under the Apache 2.0 license.

### Business Problem
While MMM's solve many business problems, the problem we are solving in this Quickstart is one of access to data and the process to develop a model. Typically an MMM needs access to large quantities of advertising data, sales data, and access to GPUs.

Typically advertising data and sales data are stored in silos across data providers, agencies, or publishers in many different formats. In this Quickstart, we do not cover the data acquisition process, however, there are [several solutions available](https://www.snowflake.com/en/developers/solutions-center/?tags=department%2Fmarketing-analytics) that cover this topic.

Next, being able to properly model the data often requires data scientists to copy that data into other systems for analysis, sometimes even personal devices. Lastly, with more granular data being available, often consumer-grade hardware simply is not up to the task and GPUs are needed - the challengs is that a complicated technology/cloud stack needs to be developed to make all this possible.

This Quickstart will show you how Snowflake makes this easy and brings your models directly to where the data resides.


### Prerequisites
- Knowledge of Marketing Mix Models, this Quickstart does not go into detail about MMM model development and Bayesian statistics.
- Familiarity with Snowflake usage (worksheets, notebooks) and Snowsight.

### What You’ll Learn 
- How to create **Compute Pools** for GPU access.
- How to create **External Access Integrations** (EAIs) to allow Notebooks to connect and install third-party packages.
- Installing and running the Meridian MMM tutorial within a **Snowflake container-runtime Notebook**.

### What You’ll Need 
- Snowflake account in a cloud/region that supports [container runtime notebooks](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks-on-spcs).
- Access to a Role that can create databases and schemas, create warehouse objects, create compute pools, create & manage Notebooks, and create EAIs.

### What You’ll Build 
- A Snowflake Notebook with the Meridian MMM tutorial.
- A working Marketing Mix Model.

<!-- ------------------------ -->
## Establishing Our Environment
Duration: 10




- **summary**: This is a sample Snowflake Guide 
  - This should be a short, 1 sentence description of your guide. This will be visible on the main landing page. 
- **id**: sample 
  - make sure to match the id here with the name of the file, all one word.
- **categories**: data-science 
  - You can have multiple categories, but the first one listed is used for the icon.
- **environments**: web 
  - `web` is default. If this will be published for a specific event or  conference, include it here.
- **status**: Published
  - (`Draft`, `Published`, `Deprecated`, `Hidden`) to indicate the progress and whether the sfguide is ready to be published. `Hidden` implies the sfguide is for restricted use, should be available only by direct URL, and should not appear on the main landing page.
- **feedback link**: https://github.com/Snowflake-Labs/sfguides/issues
- **tags**: Getting Started, Data Science, Twitter 
  - Add relevant  tags to make your sfguide easily found and SEO friendly.
- **authors**: Daniel Myers 
  - Indicate the author(s) of this specific sfguide.

---

You can see the source metadata for this guide you are reading now, on [the github repo](https://raw.githubusercontent.com/Snowflake-Labs/sfguides/master/site/sfguides/sample.md).


<!-- ------------------------ -->
## Creating a Step
Duration: 2

A single sfguide consists of multiple steps. These steps are defined in Markdown using Header 2 tag `##`. 

```markdown
## Step 1 Title
Duration: 3

All the content for the step goes here.

## Step 2 Title
Duration: 1

All the content for the step goes here.
```

To indicate how long each step will take, set the `Duration` under the step title (i.e. `##`) to an integer. The integers refer to minutes. If you set `Duration: 4` then a particular step will take 4 minutes to complete. 

The total sfguide completion time is calculated automatically for you and will be displayed on the landing page. 

<!-- ------------------------ -->
## Code Snippets, Info Boxes, and Tables
Duration: 2

Look at the [markdown source for this sfguide](https://raw.githubusercontent.com/Snowflake-Labs/sfguides/master/site/sfguides/sample.md) to see how to use markdown to generate code snippets, info boxes, and download buttons. 

### JavaScript
```javascript
{ 
  key1: "string", 
  key2: integer,
  key3: "string"
}
```

### Java
```java
for (statement 1; statement 2; statement 3) {
  // code block to be executed
}
```

### Info Boxes
> aside positive
> 
>  This will appear in a positive info box.


> aside negative
> 
>  This will appear in a negative info box.

### Buttons
<button>

  [This is a download button](link.com)
</button>

### Tables
<table>
    <thead>
        <tr>
            <th colspan="2"> **The table header** </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The table body</td>
            <td>with two columns</td>
        </tr>
    </tbody>
</table>

### Hyperlinking
[Youtube - Halsey Playlists](https://www.youtube.com/user/iamhalsey/playlists)

<!-- ------------------------ -->
## Images, Videos, and Surveys, and iFrames
Duration: 2

Look at the [markdown source for this guide](https://raw.githubusercontent.com/Snowflake-Labs/sfguides/master/site/sfguides/sample.md) to see how to use markdown to generate these elements. 

### Images
![Puppy](assets/SAMPLE.jpg)

### Videos
Videos from youtube can be directly embedded:
<video id="KmeiFXrZucE"></video>

### Inline Surveys
<form>
  <name>How do you rate yourself as a user of Snowflake?</name>
  <input type="radio" value="Beginner">
  <input type="radio" value="Intermediate">
  <input type="radio" value="Advanced">
</form>

### Embed an iframe
![https://codepen.io/MarioD/embed/Prgeja](https://en.wikipedia.org/wiki/File:Example.jpg "Try Me Publisher")

<!-- ------------------------ -->
## Conclusion And Resources
Duration: 1

At the end of your Snowflake Guide, always have a clear call to action (CTA). This CTA could be a link to the docs pages, links to videos on youtube, a GitHub repo link, etc. 

If you want to learn more about Snowflake Guide formatting, checkout the official documentation here: [Formatting Guide](https://github.com/googlecodelabs/tools/blob/master/FORMAT-GUIDE.md)

### What You Learned
- creating steps and setting duration
- adding code snippets
- embedding images, videos, and surveys
- importing other markdown files

### Related Resources
- <link to github code repo>
- <link to documentation>
