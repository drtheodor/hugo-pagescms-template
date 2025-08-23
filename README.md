# Hugo Pages CMS Template

## Installation
1. Add this repo as a submodule to your repository
2. In your CI add `hugo -s <submodule>`
3. Add this to your `hugo.yaml`:
```yaml
modules:
    mounts:
    - source: content
      target: content
    - source: <submodule>/public/src
      target: content
```
4. Add the `.pages.yml` file for Pages CMS
<details>
<summary>Example Configuration</summary>

```yaml
media:
  input: content
  output: /
  categories: [image]
settings:
  content:
    merge: true
content:
  - name: wiki
    label: Wiki
    type: collection
    path: prebuild/src
    format: yaml
    filename: "{primary}.yaml"
    view:
      fields: [ title ]
    fields:
      - { name: title, label: Title, type: string, required: true }
      - { name: description, label: Description, type: string }
      - name: blocks
        component: any-block
      - name: weight
        label: Weight
        type: number
      - name: next
        label: Next page (slug)
        type: string
      - name: prev
        label: Previous page (slug)
        type: string
      - name: params
        label: Parameters
        type: object
        fields:
          - name: images
            label: Images
            list: true
            type: image
      - name: sidebar
        label: Sidebar Options
        type: object
        fields:
          - name: open
            label: Is open by default?
            description: Only available for index pages!
            type: boolean
          - name: exclude
            label: Should be excluded from the sidebar?
            type: boolean
components:
  any-block:
    label: Body
    type: block
    list:
      collapsible:
        collapsed: true # Default collapsed state
        summary: "{index}: {fields.alt}{fields.title}{fields.type}"
    blockKey: _type
    blocks:
      - name: local-image
        label: Add Local Image
        component: local-image
      - name: remote-image
        label: Add Remote Image
        component: remote-image
      - name: text
        label: Add Text Block
        component: markdown
      - name: callout
        label: Add Callout
        component: callout
      - name: card
        label: Add Card
        component: card
      - name: cards
        label: Add Card Group
        component: cards
      - name: steps
        label: Add Steps
        component: steps
      - name: details
        label: Add Details
        component: details
  any-image:
    label: Any Image
    type: block
    list: false
    blockKey: _type
    blocks:
      - name: local-image
        label: Add Local Image
        component: local-image
      - name: remote-image
        label: Add Remote Image
        component: remote-image
  markdown:
    label: Markdown
    type: object
    fields:
      - name: alt
        label: Section Label
        description: Not actually included in the wiki. Exists only to make the collapsed sections more readable.
        type: string
        default: Content
      - name: value
        label: Text Content
        type: rich-text
  local-image:
    label: Image
    type: object
    fields:
      - name: src
        label: Image
        type: image
      - name: alt
        label: ALT text
        type: string
  remote-image:
    label: Remote image
    type: object
    fields:
      - name: src
        label: Image
        type: string
      - name: alt
        label: ALT text
        type: string
  callout:
    label: Callout
    type: object
    fields:
      - name: type
        label: Type
        default: default
        type: select
        options:
          values: [default, info, warning, error]
      - name: value
        label: Text Content
        type: rich-text
  cards:
    label: Cards
    type: object
    fields:
      - name: alt
        type: string
        default: Card Group
        hidden: true
      - name: cols
        type: number
        label: Columns
        default: 3
        options:
          min: 1
      - name: value
        component: card
        list: true
  card:
    label: Card
    type: object
    fields:
      - name: title
        label: Title
        type: string
      - name: subtitle
        label: Subtitle
        type: rich-text
      - name: link
        label: Link
        type: string
      - name: image
        component: any-image
  steps:
    label: Steps
    type: object
    fields:
      - name: value
        label: Text Content
        type: rich-text
        default: |
          ### Step 1
  details:
    label: Details
    type: object
    fields:
      - name: title
        label: Title
        type: string
      - name: closed
        label: Closed
        type: boolean
        default: true
      - name: value
        label: Text Content
        type: rich-text
```

</details>
