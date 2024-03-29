# Oxygen Documentation Action
This action triggers <i>Oxygen Scripting</i> to generate documentation for schema files on your repository. It works with [XML Schema (XSD)](https://www.oxygenxml.com/xml_editor/xml_schema_documentation.html), [XSL](https://www.oxygenxml.com/xml_editor/xml_schema_documentation.html), [WSDL](https://www.oxygenxml.com/xml_editor/xml_schema_documentation.html),
[JSON Schema](https://www.oxygenxml.com/xml_editor/json_converter.html#generate-json-schema-documentation) and [OpenAPI](https://www.oxygenxml.com/xml_editor/openapi.html#openapi-documentation-generator) files.
All you have to do is to include this action in your workflow and choose the schema to document. Find more info about workflows on https://docs.github.com/en/actions/using-workflows.

👀 See [Oxygen Scripting](https://www.oxygenxml.com/doc/versions/25.0/ug-editor/topics/scripting_oxygen.html) for more details about this script, and other useful tools.

# Requirements
In order to use this action, you need to obtain an <i>Oxygen Scripting</i> license key from https://www.oxygenxml.com/xml_scripting/pricing.html (you can also request a [trial](https://www.oxygenxml.com/xml_scripting/register.html)). Add it as a secret to your repository (Settings &rarr; Secrets &rarr; Actions &rarr; New repository secret), and name it "SCRIPTING_LICENSE_KEY".
Then use it as an environment variable:
```yaml
env:
  SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
```

# Sample usage

If you don't already have a workflow defined in your repository, you can use one of the samples below.

💡 This workflow requires manual trigger from the 'Actions' tab:
```yaml
name: Run Documentation Script
on:
  workflow_dispatch:
    inputs:
      schemaFile:
        description: 'Schema file to generate documentation for.'
        required: true # e.g. resources/schema.xsd
jobs:
  generate-doc:
    runs-on: ubuntu-latest
    steps:
      - name: Oxygen Documentation Script
        uses: oxygenxml/oxygen-script-documentation-action@v1.0.0
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          schemaFile: ${{ inputs.schemaFile }}
```
💡 This workflow starts whenever an XSD file is changed, and runs the Documentation Script for the main XSD file.
```yaml
name: Run Documentation Script (for XSD files)
on:
  push:
    branches:
      - main
    paths:
      - '**.xsd'
jobs:
  generate-doc:
    runs-on: ubuntu-latest
    steps:
      - name: Oxygen Documentation Script
        uses: oxygenxml/oxygen-script-documentation-action@v1.0.0
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          schemaFile: schemas/main.xsd
```

👀 Check [Oxygen Scripting - Documentation template](https://github.com/oxygenxml/oxygen-script-documentation-template) for a ready-to-use template with some sample schema files.

# Deployment to GitHub Pages
After a successful run of the Documentation Script, the documentation files are pushed to the <i>gh-pages</i> branch. 
If you want the documentation to be published to GitHub Pages, all you have to do is go to Settings &rarr; Pages, and under <i>Build and deployment</i> section select the <i>gh-pages</i> branch instead of the <i>main</i> branch. 

The deployment workflow should automatically start and the documentation should be available shortly at: https://{userid}.github.io/{reponame}/{schemaFile}.html.
