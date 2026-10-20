# ngx-treeview
An Angular Bootstrap treeview component with checkboxes

> This is a fork of [@charmedme/ngx-treeview](https://github.com/charmedme/ngx-treeview) (originally [leovo2708/ngx-treeview](https://github.com/leovo2708/ngx-treeview)), upgraded to Angular 22 and published as `@linewlarsen/ngx-treeview`.
> For Angular 21, use [@charmedme/ngx-treeview@3](https://www.npmjs.com/package/@charmedme/ngx-treeview).

## Dependencies

- [Angular](https://angular.dev)
- [Lodash](https://lodash.com)
- [Bootstrap 5](https://getbootstrap.com)

For Bootstrap 4, use [@charmedme/ngx-treeview@1.0.0](https://www.npmjs.com/package/@charmedme/ngx-treeview/v/1.0.0)

You can customize CSS yourself to break down dependencies to Bootstrap.

## Compatibility

| @linewlarsen/ngx-treeview | Angular | Bootstrap | Node.js |
|---|---|---|---|
| 22.x | `^22.0.0` | `^5.0.0` | 22.22+ or 24.13+ |

## Features

- Unlimited tree level
- State: disabled / collapse, expand
- Filtering
- Internationalization (i18n) support
- Template
- Checkbox with tri-state

## Status
[![NPM Version](https://img.shields.io/npm/v/@linewlarsen/ngx-treeview.svg)](https://www.npmjs.com/package/@linewlarsen/ngx-treeview)

## Demo
The API is unchanged from the upstream package, so the upstream demo applies:
[https://charmedme.github.io/ngx-treeview/](https://charmedme.github.io/ngx-treeview)

## Installation

After installing the above dependencies, install `ngx-treeview` via:

```shell
npm i @linewlarsen/ngx-treeview --save
```

### NgModule-based applications

```ts
import { TreeviewModule } from '@linewlarsen/ngx-treeview';

@NgModule({
  declarations: [AppComponent, ...],
  imports: [TreeviewModule.forRoot(), ...],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```

### Standalone applications

Register the providers once when bootstrapping:

```ts
import { importProvidersFrom } from '@angular/core';
import { TreeviewModule } from '@linewlarsen/ngx-treeview';

bootstrapApplication(AppComponent, {
  providers: [importProvidersFrom(TreeviewModule.forRoot())]
});
```

Then import `TreeviewModule` in the components that use it:

```ts
@Component({
  imports: [TreeviewModule],
  ...
})
export class MyComponent {}
```

### Migrating from @charmedme/ngx-treeview

Replace the package and update the import paths. No other code changes are needed.

```shell
npm uninstall @charmedme/ngx-treeview
npm i @linewlarsen/ngx-treeview
```

```diff
- import { TreeviewModule, TreeviewItem } from '@charmedme/ngx-treeview';
+ import { TreeviewModule, TreeviewItem } from '@linewlarsen/ngx-treeview';
```

## Usage

#### Treeview:

```html
<ngx-treeview
  [config]="config"
  [items]="items"
  (selectedChange)="onSelectedChange($event)"
  (filterChange)="onFilterChange($event)"
>
</ngx-treeview>
```

#### Treeview with dropdown:

```html
<ngx-dropdown-treeview
  [buttonClass]="buttonClass"
  [config]="config"
  [items]="items"
  (selectedChange)="onSelectedChange($event)"
  (filterChange)="onFilterChange($event)"
>
</ngx-dropdown-treeview>
```

`config` is optional. This is the default configuration:

```js
{
   hasAllCheckBox: true,
   hasFilter: false,
   hasCollapseExpand: false,
   decoupleChildFromParent: false,
   maxHeight: 500
}
```

You can change default configuration easily because TreeviewConfig is injectable.

#### Pipe `ngxTreeview`:

To map your JSON objects to TreeItem objects.

```html
<ngx-dropdown-treeview
  [config]="config"
  [items]="items | ngxTreeview:'textField'"
  (selectedChange)="onSelectedChange($event)"
>
</ngx-dropdown-treeview>
```

#### Create a TreeviewItem:

```ts
const itCategory = new TreeviewItem(
{
  text: "Software",
  value: 9,
  children: [
    {
      text: "Programming",
      value: 91,
      children: [
        {
          text: "Frontend",
          value: 911,
          children: [
            { text: "Angular 20", value: 9114 },
            { text: "Angular 21", value: 9115 },
            { text: "Angular 22", value: 9116, disabled: true },
            { text: "ReactJS", value: 9120 },
          ],
        },
        {
          text: "Backend",
          value: 912,
          children: [
            { text: "C#", value: 9121 },
            { text: "Java", value: 9122 },
            { text: "Python", value: 9123, checked: false },
          ],
        },
      ],
    },
  ],
},
{
  text: "Networking",
  value: 92,
  children: [
    { text: "Internet", value: 921 },
    { text: "Security", value: 922 },
    { text: "Switches", value: 923 },
  ],
});
```

You can pass the second parameter 'autoCorrectChecked' with value=true (default is false) in constructor of TreeviewItem to correct checked value of it and all of its descendants. In some cases, you need to push or pop children flexibly, checked of parent may be not correct. Then you need to call function correctChecked() to help to correct from root to its descendants.

```ts
const vegetableCategory = new TreeviewItem({
  text: "Vegetable",
  value: 2,
  children: [
    { text: "Salad", value: 21 },
    { text: "Potato", value: 22 },
  ],
});
vegetableCategory.children.push(
  new TreeviewItem({ text: "Mushroom", value: 23, checked: false })
);
vegetableCategory.correctChecked(); // need this to make 'Vegetable' node to change checked value from true to false
```

#### TreeviewEventParser:

Extract data from list of checked TreeviewItem and send it in parameter of event selectedChange. Some built-in TreeviewEventParser:

- DefaultTreeviewEventParser: return values of checked items.
- DownlineTreeviewEventParser: return list of checked items in original order with their ancestors.
- OrderDownlineTreeviewEventParser: return list of checked items in checked order with their ancestors. Note that: value of a leaf must be different from value of other leaves.

#### Templating:

See example 4 & 5 in the demo.

## Contributing

Ideas, proposals and bug reports are welcome as [GitHub issues](https://github.com/LineWLarsen/ngx-treeview/issues). Changes that are useful upstream are also offered to [charmedme/ngx-treeview](https://github.com/charmedme/ngx-treeview).