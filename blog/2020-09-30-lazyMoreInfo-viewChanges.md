---
author: Zack Barett
authorURL: https://github.com/zsarnett
authorTwitter: zsarnett
title: "Lovelace: Lazy Loading More Info and Custom View Layouts"
---

Custom Element developers can now create Custom View Layouts that users can load and use!

In 0.116, we will be changing the way that we create Views in Lovelace. In the past, we had 2 views, a default view, and a panel view. When talking about adding Drag and Drop to Lovelace we decided we could do even better and start allowing custom view types.

Custom Developers will now be able to create a view that receives the following properties:

```ts
interface LovelaceViewElement {
  hass?: HomeAssistant;
  lovelace?: Lovelace;
  index?: number;
  cards?: Array<LovelaceCard | HuiErrorCard>;
  badges?: LovelaceBadge[];
  setConfig(config: LovelaceViewConfig): void;
}
```

The cards and badges will be already created and Hass will be updated on each card so that you do not need to worry about updating Hass on the Cards array for every Hass update.

Here is an example below: (note: this example does not have all of the properties but the necessities to show the example)

```ts
class MyNewView extends LitElement {
  @property({ attribute: false }) public cards: Array<LovelaceCard | HuiErrorCard> = [];

  public setConfig(_config: LovelaceViewConfig): void {}

  protected render(): TemplateResult {
    return html`${this.cards.map((card) => card)}`;
  }
}
```

And you can define this element in the Custom Element Registry just as you would with a Custom Card:

```ts
customElements.define("my-new-view", MyNewView);
```

You can find an example of this in our default view: `Masonry View` Located here: [frontend/src/panels/lovelace/views/hui-masonry-view.ts](https://github.com/home-assistant/frontend/blob/master/src/panels/lovelace/views/hui-masonry-view.ts)

A user who downloads and installs your new Custom View can then use it via editing their YAML configuration of their view to be:

```yaml
- title: Home View
  type: custom:my-new-view
  badges: ...
  cards: ...
```